---
description: Analyze and improve test coverage for .NET projects
argument-hint: Explain the context, what is the code whose test coverage should be improved.
allowed-tools: Task, AskUserQuestion, Read, Bash, Edit, Write, Bash(git status:*), Bash(git log:*), Skill
---

## User instructions

User has given the following input: $ARGUMENTS

This should be the description  of the scope in which the test coverage should be improved.


## Your task

### 1. Understand the scope

Understand the scope in which to improve test coverage.  If the scope is unclear, use the AskUserQuestion tool to confirm the scope.

The scope should not be test files; it should be the production code that is tested. All tests should always be run to get the exact coverage of any piece of code.

### 2. Get recommendations about which tests to add

Launch a `test-coverage-improvement-planner` agent. As input, explain the scope of the analysis.

Expect a report with clear recommendations on which test cases to add.

### 3. Implement the recommended test cases

Implement the new test cases based on the recommendations from the test coverage improvement agent.

Make sure you activate any suitable skills or custom sub agents to implement the test cases.

### 4. Run all tests

Make sure all tests pass.
