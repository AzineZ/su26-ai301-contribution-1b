# Contribution #22: Does not detect sentences ending with ! or ?

**Contribution Number:** #22  
**Student:** Phi Nguyen  
**Issue:** [GitHub issue link](https://github.com/JoshuaKGoldberg/sentences-per-line/issues/22)  
**Status:** Phase II Complete

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

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
