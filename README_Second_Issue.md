# Contribution #207: 🧪 Testing: Add unit test for accepted issue with label object

**Contribution Number:** #207

**Student:** Phi Nguyen  
**Issue:** [GitHub issue link](https://github.com/JoshuaKGoldberg/all-contributors-for-repository/issues/207)  
**Status:** Phase IV Complete

---

## Why I Chose This Issue

I chose issue #207: "🧪 Testing: Add unit test for accepted issue with label object" because it aligns with my JavaScript and Typescript experience 
and my goal to improve my testing skills. The issue are labeled "good first issue" and "accepting PRs" and has clear deployment and contributing steps. 

From reading the issue thread, I understand the current 
problem is that a unit test doesn't cover a branch in the code it is designed to. My job is to add a test case that covers the branch.

I left a comment on the issue introducing myself and awaiting the maintainer's response. Although the issue is from 2024, I have worked on an issue of the maintainer's other repo. 
I am confident he will respond.

---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]
- In `addAcceptedIssues.ts`, each issue's labels are normalized with `typeof label === "string" ? label : label.name`. This handles two shapes that the GitHub API can return for a label: a plain string, or an object with a `name` property. The existing test suite only ever passed labels as strings, so the object branch (`label.name`) was never exercised. The issue asks for a unit test that covers this missing branch.

### Expected Behavior

[What should happen?]
- When an accepted issue has a label supplied as an object (e.g. `{ name: "labelTypeBug" }`), the code should read `label.name`, match it against the configured label types, and add the contributor with the correct contribution type. A test should assert this behavior.

### Current Behavior

[What actually happens?]
- The `label.name` branch works in production, but no test verifies it. Coverage reports flag the else side of the ternary as uncovered, so a future regression to the object-handling path would go undetected.

### Affected Components

[Which parts of the codebase are involved?]
- `src/collect/adding/addAcceptedIssues.ts` — contains the `typeof label === "string" ? label : label.name` line that has the uncovered branch. No source change is required; the logic is already correct.
- `src/collect/adding/addAcceptedIssues.test.ts` — the test file missing a case for the object-shaped label. This is where the new test is added.

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]
- Create a new folder on VSCode.
- Fork and clone the repo to this folder.
- Run `pnpm install` and follow the contributing/development steps in the repo.
- Done!

### Steps to Reproduce

1. Run the test suite with coverage (`pnpm run test --coverage`) on `addAcceptedIssues.test.ts`.
2. Observe that all existing tests pass, but the coverage report shows the `label.name` branch of the ternary in `addAcceptedIssues.ts` is not covered.
3. Observed result: every existing test passes labels as strings, so the object branch is never hit.

### Reproduction Evidence

- **Commit showing reproduction:** [Link](https://github.com/AzineZ/all-contributors-for-repository/commit/c4fb92e12c1895fbb9a4cc85da5ef7ac30784681)
- **My findings:**
  - The `AcceptedIssue` type comes straight from the Octokit response, so labels legitimately arrive as either strings or `{ name }` objects. The object case is real, just untested.
  - No source change is needed — this is purely a test-coverage gap.

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]
- The root cause is not a bug in the source but a gap in coverage. Every existing test in `addAcceptedIssues.test.ts` constructs labels as strings (e.g. `labels: [options.labelTypeBug]`), so the `typeof label === "string"` check always takes the `true` branch. The `false` branch that reads `label.name` is never executed by a test.

### Proposed Solution

[High-level description of your fix approach]
- Add a single unit test that constructs an accepted issue whose label is an object (`{ name: options.labelTypeBug }`) and assert that the contributor is added with the `"bug"` contribution. This models the object-shaped label path and covers the missing branch.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
- Add a unit test that exercises the object-shaped label branch (`label.name`) in `addAcceptedIssues.ts`, which existing tests don't cover.

**Match:** 
- The existing string-label tests (e.g. "adds an issue when it has a bug label") are the perfect template. I mirror one of them but pass the label as an object instead of a string.

**Plan:**
1. Open `src/collect/adding/addAcceptedIssues.test.ts`.
2. Add a new `it(...)` case that builds a stub issue with `labels: [{ name: options.labelTypeBug }]`.
3. Assert `add` was called once with `(login, issue.number, "bug")`.
4. Run the test suite and confirm the new test passes and the branch is now covered.

**Implement:**
- [Add unit test for accepted issue with label object](https://github.com/AzineZ/all-contributors-for-repository/commit/c4fb92e12c1895fbb9a4cc85da5ef7ac30784681)

**Review:**
- The new test follows the exact structure and naming convention of the surrounding tests. Formatting is handled automatically by Prettier as recommended by the project.
- No source code was changed — only a test was added, keeping the change minimal and focused on the issue.

**Evaluate:**
- The new test must pass, and the previously uncovered `label.name` branch must now report as covered.

---

## Testing Strategy

### The Reviewer specifically asks for a single extra test that covers the uncovered branch so no more tests will be added. The testing section of this readme will be empty.

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [1] Progress

- Read through `addAcceptedIssues.ts` to understand how labels are normalized (string vs. object).
- Confirmed the `label.name` branch was the uncovered path.
- Added a unit test in `addAcceptedIssues.test.ts` mirroring the existing string-label tests but passing the label as an object.
- Ran the test suite; the new test passes and covers the branch.

- Challenges: The main work was reading the code carefully enough to identify exactly which branch lacked coverage, since the logic itself already worked.

### Code Changes

- **Files modified:**
  - src/collect/adding/addAcceptedIssues.test.ts
- **Key commits:**
  - [Add unit test for accepted issue with label object](https://github.com/AzineZ/all-contributors-for-repository/commit/c4fb92e12c1895fbb9a4cc85da5ef7ac30784681)
- **Approach decisions:**
  - I modeled the new test on the existing "adds an issue when it has a bug label" test so it reads consistently with the rest of the suite.
  - The change is intentionally minimal. One test, no source changes, because the issue only asks for the missing coverage.

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted](https://github.com/JoshuaKGoldberg/all-contributors-for-repository/pull/1244)

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

<!-- 👋 Hi, thanks for sending a PR to all-contributors-for-repository! 🤝
Please fill out all fields below and make sure each item is true and [x] checked.
Otherwise we may not be able to review your PR. -->

## PR Checklist

- [x] Addresses an existing open issue: fixes #207 
- [x] That issue was marked as [`status: accepting prs`](https://github.com/JoshuaKGoldberg/all-contributors-for-repository/issues?q=is%3Aopen+is%3Aissue+label%3A%22status%3A+accepting+prs%22)
- [x] Steps in [CONTRIBUTING.md](https://github.com/JoshuaKGoldberg/all-contributors-for-repository/blob/main/.github/CONTRIBUTING.md) were taken

## Overview

<!-- Description of what is changed and how the code change does that. -->

- Adds a unit test to `addAcceptedIssues.test.ts` covering the case where a
label is an object with a `name` property, rather than a string. Previously
only the string branch of `typeof label === "string" ? label : label.name`
was exercised; this closes that coverage gap.


**Maintainer Feedback:**
- [7/15/2026]: Submitted PR, waiting for review
- [7/18/2026]: Reviewer approved, PR merged and closed

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

- Typescript
- Unit testing and branch coverage
- Reading unfamiliar code to isolate an untested path
### Challenges Overcome

- Identifying the exact uncovered branch. The fix was small, but it required understanding how the GitHub API returns labels in two different shapes and why only one was being tested.

### What I'd Do Differently Next Time

[Reflection on your process]
- I think I did well on this one. The previous issue I worked on helped me practice the ability to read new codebase well, and this issue is smaller than the previous one. I don't think I have anything that I would do differently for this issue.

---

## Resources Used

- The existing tests in `addAcceptedIssues.test.ts`, which served as the template for my new test.
