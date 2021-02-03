---
title: 'Assert Statements in Python'
date: '2020-11-07'
spoiler: Implement light checks within your code.
---

- `assert {condition}` checks for the condition within the code and raises an assertion error if the condition is violated
- assertion errors are not intended to signal expected error conditions like file not found error where user takes corrective actions
- assertions are meant to be internal self checks for the program.They declare certain conditions as impossible for the code and crashing the code when one such conditions occur
- mostly the conditions declared in assertions should not hit , if it does that means there is a bug in the code
- assertions can be turned off globally by passing some flags during runtime and thus assertions should not be used as a part of critical business logic