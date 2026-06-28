# Contribution #22: Does not detect sentences ending with ! or ?

**Contribution Number:** #22  
**Student:** Phi Nguyen  
**Issue:** [GitHub issue link](https://github.com/JoshuaKGoldberg/sentences-per-line/issues/22)  
**Status:** Phase IV Complete

---

## Why I Chose This Issue

I chose issue #22 "Does not detect sentences ending with ! or ?" because 
it aligns with my JavaScript and Typescript experience and my goal to improve my frontend skills. 
The issue are labeled "good first issue" and "status: accepting prs" and has clear local deployment steps in the readme.
Finally, the project uses Vite, which is a stack I have used in the past.

The package ensures that every markdown line contains at most one sentence. From reading the issue thread, 
I understand the current problem is that the package is only capable of tracking sentences ending with a dot,
not question mark or exclamation mark. My goal is to ensure the package works on sentences ending with these two symbols.

I left a comment on the issue introducing myself and awaiting the maintainer's response. He was last active in December 2025 as 
of this writing checking out a PR to fix this issue. That PR was closed, unmerged to the main branch.



---

## Understanding the Issue

### Problem Description

The current implementation of the package only handles sentences ending in '.'. It doesn't detect sentences ending with '!' or '?'.

### Expected Behavior

The package should detect sentences ending in '.', '!' and '?'.

### Current Behavior

The package can only detects '.' in sentences.

### Affected Components

- The getIndexBeforeSecondSentence.ts file is responsibile for detecting the end of the first sentence. Right now, 
it can only detect sentences ending in '.'. I need to add functionality to this file so it can handle '?' and '!'
The getIndexBeforeSecondSentence.test.ts only covers cases of '.', I need to add new tests covering '?' and '!'.
- The modifyNodeIfMultipleSentencesInLine.ts file is also affected since it has a logic that checks for dot, which means it also needs
to be modified to checks for the two new punctuations.
- Test files that test the dot behavior are affected since they now need to test the other two punctuations.

---

## Reproduction Process

### Environment Setup

- Create a new folder on VSCode.
- Clone the forked repo to this folder.
- Follow the instructions in the DEVELOPMENT.md (shown on the readme page)
- Done!

### Steps to Reproduce

1. All tests will pass initially since they only account for the dot. Add more tests for the '?' and '!' in getIndexBeforeSecondSentence.test.ts.
2. Run this test file again, it will show the newly added tests failing.
3. Done! 
### Reproduction Evidence

- **Commit showing reproduction:** [Link](https://github.com/AzineZ/sentences-per-line/commit/fdb70438f9af5c0a3bb1a696ceeced56764af52e)
- **My findings:**
  - The markdownlint plugin actually checks for dot as well. I may have to modify this file in addition to getIndexBeforeSecondSentence.ts
to correctly implement ! and ?.
  - No separate integration tests but some plugin's tests rely on other plugins. This can work as integration tests.

---

## Solution Approach

### Analysis

In getIndexBeforeSecondSentence.ts, function getIndexBeforeSecondSentence only checks for dot. There is no if statements that can check for other two punctuations.

### Proposed Solution

- Add a logic to getIndexBeforeSecondSentence.ts to account for ! and ?. Check the existing implementation of the dot to make the work easier since they are all punctuations that end a sentence.
- Check other packages that interact with sentence-per-line package and see if they need changes to accommodate the two new punctuations.
- Add new tests for every plugin that need changes.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Fix the package so that punctuations like ! and ? are detected by the package.

**Match:** No solution exists prior to this since only dot was considered up until now.

**Plan:** [Step-by-step implementation plan]
1. Go to function getIndexBeforeSecondSentence in getIndexBeforeSecondSentence.ts.
2. Add a new if block handling the ! and ? punctuations. Put this if block above the existing one that handles the dot.
3. Run the solution against the new tests for this sentence-per-line package.
4. If that works, add modification to modifyNodeIfMultipleSentencesInLine.ts since it needs to check for ! and ? as well.
5. Run its tests.
6. Check other test files and add tests for ! and ? as needed.
7. Confirm that all tests pass.

**Implement:** [Link to your branch/commits as you work]
- [Part 0 (this commit verifies bug reproduction)](https://github.com/AzineZ/sentences-per-line/commit/fdb70438f9af5c0a3bb1a696ceeced56764af52e)
- [Part 1](https://github.com/AzineZ/sentences-per-line/commit/f09474592002c6b6f74b271cf97f6bc92aa8030a)
- [Part 2](https://github.com/AzineZ/sentences-per-line/commit/2bd66460c737064daa57954c61fca94fdd640a1b)
- [Part 3](https://github.com/JoshuaKGoldberg/sentences-per-line/pull/1175/changes/d6fb8ccaf133e9a3adad1ee470d7bfa9cec51e7e)
- [Part 4](https://github.com/JoshuaKGoldberg/sentences-per-line/pull/1175/changes/4076615ea830640459f5d89ee75c60a0bacfc0c4)

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]
- I modeled my fix similarly to the existing dot implementation. Formatting is automatically handled by Prettier, which is also
recommended by the author.
- All tests across all plugins are updated to handle ! and ?.

**Evaluate:** [How will you verify it works?]
- All tests need to pass.
- Since the plugins including sentence-per-line depend on each other, having each plugin's test file test the new ! and ? features will confirm if they work
across packages. This will be the integration tests of this project.
---

## Testing Strategy

### Unit Tests
- I added dozens of tests so I will put a few main ones here.
- [x] Test case 1: ["Hello world! Another sentence!", 12] - This checks for sentences ending with exclamation mark, return 12, where the first sentence ends.
- [x] Test case 2: ["`Hello?` World", undefined] - This checks for sentence wrapped in backticks. Return undefined since it's not a sentence.
- [x] Test case 3: ["Hey!? What are you doing?", 5] - This checks for consecutive ! and ?, which should still be valid. Return 5, where the ? is.

### Integration Tests
- The code doesn't have traditional integration tests but different plugins in the code do require functionality of other plugins. For example, markdownlintsentenceperline uses sentence-per-line plugin too. For that, I added tests for ! and ? in these plugins' own test file, and they all passed. 

### Manual Testing

I was recommended to add tests to the test files instead of manually test them so whenever I want to test a case, I add it to the test file and run the test suite.

---

## Implementation Notes

### Week [2] Progress

- Added an if statement in getIndexBeforeSecondSentence function to handle exclamation mark and question mark.
- Added checks for exclamation mark and question mark in the markdownlint file so the sentence node containing these new punctuations can be traversed.
- Added tests to all test files across plugins that test the period to now also test the new punctuations.

- Challenges: I had to check where else on the code was the sentence-per-line package use, which I found is in all other plugins in the code. So I had to go in every plugin and check if I have to add checks for ! and ?. These plugins' test files need to be able to pass sentences containing ! and ? as well.
- Submitted PR.


[Continue documenting as you work]

### Code Changes

- **Files modified:**
  - packages/eslint-plugin-sentences-per-line/src/rules/one.test.ts
  - packages/markdownlint-sentences-per-line/src/markdownlintSentencesPerLine.test.ts
  - packages/prettier-plugin-sentences-per-line/src/modifications/modifyNodeIfMultipleSentencesInLine.ts
  - packages/prettier-plugin-sentences-per-line/src/index.test.ts
  - packages/sentences-per-line/src/getIndexBeforeSecondSentence.test.ts
  - packages/sentences-per-line/src/getIndexBeforeSecondSentence.ts
  - packages/sentences-per-line/README.md
- **Key commits:**
  - [Part 1](https://github.com/AzineZ/sentences-per-line/commit/f09474592002c6b6f74b271cf97f6bc92aa8030a)
  - [Part 2](https://github.com/AzineZ/sentences-per-line/commit/2bd66460c737064daa57954c61fca94fdd640a1b)
- **Approach decisions:** [Why you chose certain approaches]
  - I designed ! and ? to behave similarly to the existing period implementation since they are all punctuations that end a sentence.
  - My code change is minimal, mostly just adding tests. The original code is designed to be easily contributed.

---

## Pull Request

**PR Link:** [[GitHub PR URL](https://github.com/JoshuaKGoldberg/sentences-per-line/pull/1175)]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]
<!-- 👋 Hi, thanks for sending a PR to sentences-per-line! 💖.
Please fill out all fields below and make sure each item is true and [x] checked.
Otherwise we may not be able to review your PR. -->

## PR Checklist

- [x] Addresses an existing open issue: Does not detect sentences ending with ! or ? #22 
- [x] That issue was marked as [`status: accepting prs`](https://github.com/JoshuaKGoldberg/sentences-per-line/issues?q=is%3Aopen+is%3Aissue+label%3A%22status%3A+accepting+prs%22)
- [x] Steps in [CONTRIBUTING.md](https://github.com/JoshuaKGoldberg/sentences-per-line/blob/main/.github/CONTRIBUTING.md) were taken

## Overview

<!-- Description of what is changed and how the code change does that. -->

fixes #22 

- Added to getIndexBeforeSecondSentence.ts the functionality to detect exclamation mark and question mark. It was initially handling just the period.
- Modified modifyNodeIfMultipleSentencesInLine.ts so the function can walk the sentence node containing the exclamation mark and question mark.
- Added tests for exclamation mark and question mark across test files in different plugins.

**Maintainer Feedback:**
- 6/17/2026: From maintainer: [Bug] This duplicates the logic from the following block, except it misses the doesEndWithIgnoredWord. You'll need to deduplicate the blocks and add tests to make sure this case isn't missed.
- 6/17/2026: combined logic in doesENdWithIgnoredWord. Removed unnecessary check that triggered lint error. Resubmitted re-review request.
- 6/24/2026: Reviewer asked if the capital letter check in the sentence tree traversal file can be extended to the existing period functionality. I fulfilled the request and are waiting for feedback.
- 6/27/2026: Approved and merged by reviewer. 

**Status:** [PR merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]
- Typescript
- Testing
- Debugging

### Challenges Overcome

[What was hard and how you solved it]
- Making sure your fix applies project-wise, not just file-wise. I had to check where else my fixed function can be called and adjust files at those places as needed.

### What I'd Do Differently Next Time

[Reflection on your process]
- I should have spent more time reading over the code. I was a little eager to begin implementing the fix.

---

## Resources Used
- None other than the maintainer. I was able to do it without looking up online.

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
