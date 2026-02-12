# Test-Driven Development (TDD)
- [Test-Driven Development (TDD)](#test-driven-development-tdd)
  - [Overview](#overview)
  - [Code Coverage](#code-coverage)
  - [Software](#software)

## Overview

![TDD Steps: 1) write a test that fails, 2) write enough code to compile, and 3) complete code to meet requirements of test. Repeat as needed.](images/learning_test-driven-development_overview.png)

- Testers have no bias toward code and often find defects that developers miss. Test driven reduces the bias that developers have by approaching the code from the perspective of they should implement to meet the test scenarios instead of building test scenarios that meet the code.
- Can expose poorly designed monolithic code

## Code Coverage

- Code coverage is the percentage of total code being executed by tests
- Achieving 100% code coverage is challeng$ing
- Code coverage goals should be included in the team’s definition of done
- Recommend starting with low percentage and ramp up to 100% at team’s pace
- Automated unit testing should be part of continuous build process

## Software

- Mockito
- PowerMock
- MOQ
