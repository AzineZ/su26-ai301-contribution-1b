# Contribution #22: Does not detect sentences ending with ! or ?

**Contribution Number:** #22  
**Student:** Phi Nguyen  
**Issue:** [GitHub issue link](https://github.com/JoshuaKGoldberg/sentences-per-line/issues/22)  
**Status:** Phase I Complete

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

The getIndexBeforeSecondSentence.ts file is responsibile for detecting the end of the first sentence. Right now, 
it can only detect sentences ending in '.'. I need to add functionality to this file so it can handle '?' and '!'
The getIndexBeforeSecondSentence.test.ts only covers cases of '.', I need to add new tests covering '?' and '!'.

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

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

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
