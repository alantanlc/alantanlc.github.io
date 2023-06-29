---
layout: post
categories: blog
---

# Thoughts on Regression And Unit Testing

Saw this LinkedIn [post](https://www.linkedin.com/posts/mansoorshaikh_testing-automation-java-activity-7054983226951409665-T9p4?utm_source=share&utm_medium=member_ios) sharing 21 hygiene items on test automation. Some of the points resonated with me and I would love to elaborate on them based on my personal experience.

## Use VCS and CICD Pipelines (Jenkins)

__Summary__

Use Version Control (git) and CI/CD pipelines (Jenkins).

__Thoughts__

VCS is handy to show a history of the test cases written, the author of each commit along with some description about why the test case was added (what is being tested). This gives someone who is taking over the codebase some background as well as hints on who to look for in case of any questions.

Having the tests (including unit tests such as JUnit and feature tests such as Cucumber) run in an automated way as part of your CICD pipeline provides early discovery of failing tests to be fixed, be it the test case itself or a real issue in the main codebase.

## Delete Obsolete Tests

__Summary__

Delete obsolete tests. Yes delete it and not comment it. Remember every line of code has a maintainance cost associated with it.

__Thoughts__

Every line of code has a maintenance cost associated with it indeed. 