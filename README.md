Writing high quality, easily maintainable code: A Guide
=============

# Introduction

Hey there!

This is a guide for writing high quality code. It's a set of principles and practices designed to maximize quality and maintainability without compromising velocity, timeline, or budget.

# Contents
- Introduction
- [tldr;](#tldr)
- [Project Setup](#project-setup)
- [Conclusion](#conclusion)

# tldr;

# Project Setup

Set up the project for success before its underway! Complete the following checklist items when a project is getting started in order to put in place easy conventions and habits for facilitating the code review process without friction.

- [ ] Configure Pull Request Labels
- [ ] Set up Continuous Integration
- [ ] Adopt a branching strategy
- [ ] Stand up a staging environment
- [ ] Install code linters

## Configure Pull Request Labels

Set up GitHub labels in order to easily track the status of a pull request. Configure at least the following labels. Add additional labels as necessary.
-	**wip**: a pull request is in progress. There is no need to review it unless you are specifically *@mentioned*. Use the color orange so it easily stands out.
-	**blocked**: the request cannot be completed. Be sure to indicate why in the description. Use a more subdued color like blue so it doesn't distract from more important items.
-	**for review**: the pull request is ready for review. Use the color yellow so it easily stands out.

## Set up Continuous Integration

Using a Continuous Integration server allows for easy verification that the changes have successfully passed the test suite and linter checks. It allows ensures the checks pass successfully on another machine, other than the machine that the feature was developed on.

## Adopt a branching strategy

Define a branching strategy in your project's documentation, wiki, or Readme. This informs developers how a feature will go through the development process and eventually end up in Production. A good default is to assign `master` as *staging ready* and then use tags to assign a particular commit as *production ready*. This makes it frictionless to additionally maintain a changelog.

## Stand up a staging environment

Staging environments allow the feature to be reviewed and smoke tested before it goes live in Production. This can catch any issues that the test suite didn't find and also allow the change to be reviewed without necessarily having a local development environment set up. A few tips:
- Deploy `master` to it regularly or automatically
- Ensure it has realistic data to test with
- Keep its architecture similar to the production environment

## Install code linters

Automate style guide and other quality checks by using code linting tools. This allows the reviewer to focus on less nitpicky issues while ensuring these easily correctable issues get addressed before review by a human.
