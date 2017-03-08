Writing high quality, easily maintainable code: A Guide
=============

# Introduction

Hey there!

This is a guide for writing high quality code. It's a set of principles and practices designed to maximize quality and maintainability without compromising velocity, timeline, or budget.

# tldr;

# Contents
- Introduction
- [tldr;](#tldr)
- [Project Setup](#project-setup)
- [Conclusion](#conclusion)

# Project Setup

Set up the project for success before its underway! Complete the following checklist items when a project is getting started in order to put in place easy conventions and habits for facilitating the code review process without friction.

- [ ] Configure Pull Request Labels
- [ ] Set up Continuous Integration
- [ ] Adopt a branching strategy
- [ ] Stand up a staging environment
- [ ] Install code linters

## Configure Pull Request Labels

Set up GitHub labels in order to easily track the status of a pull request. Configure at least the following labels. Add additional labels as necessary.
-	**wip**: a pull request is in progress. There is no need to review it unless you are specifically *@mentioned*
-	**blocked**: the request cannot be completed. Be sure to indicate why in the description.
-	**for review**: the request request is ready for review
