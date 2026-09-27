---
name: bug-fixing
description: A structured approach to fix bugs. Use when issues need analysis, reproduction, testing and fixing.
---

# Bug Fixing

## Reproduction
- Reproduce the failure as it is described
- If other failures occur, document them as described in the `story-writing` skill - don't work on them yet
- Reproduce the failure across multiple testruns
- Capture the exact steps and symptoms
- Don't proceed until the failure is reproduced

## Hypotheses
- Generate up to 5 falsifiable hypotheses
- Rank them based on probability
- Discuss the list with me

## Regression Testing
- We BDD our bugs, write the regression tests before working on a fix
- Make sure the new tests fail before moving on (red/ green)

## Fixing
- Try to reproduce why the failure occurred in the first place
- If similar patterns exist across the project, document them as described in the `story-writing` skill - don't work on them yet
- Find and apply the best fix for the described failure (follow the `bacon-workflow` skill)
- Make sure the tests succeed before moving on (red/ green)

## Cleanup
- Feature and main branch no longer reproduce the failure
- The hypothesis that turned out correct is stated in the commit/ PR message
- Document lessons learned (what would have prevented this bug?)
- Handover to `bacon-review` skill if applicable
