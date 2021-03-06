Writing high quality, easily maintainable code: A Guide
=============

# Introduction

Hi there!

This is a guide for writing high quality code. It's a set of principles and practices designed to maximize quality and maintainability without compromising velocity, timeline, or budget.

# tldr;

Writing high quality, easily maintainable code can be achieved by implementing a process for collaboration, review and feedback. Collaborate with other team members by announcing proposed changes earlier, request a review when the change is finalized, and use automated code review tools to catch [easy to miss issues](#review-checklist--areas-of-consideration) and style inconsistencies. Set up additional infrastructure to facilitate usability testing and ensure a passing automated test suite, such as staging servers and continuous integration.

# Contents
- Introduction
- [tldr;](#tldr)
- [Project Setup](#project-setup)
  - [Configure pull request status tracking](#configure-pull-request-status-tracking)
  - [Set up Continuous Integration](#set-up-continuous-integration)
  - [Adopt a branching strategy](#adopt-a-branching-strategy)
  - [Stand up a staging environment](#stand-up-a-staging-server)
  - [Install code linters](#install-code-linters)
- [Pull Request Responsibilities](#pull-request-responsibilities)
  - [General Guidelines](#general-guidelines)
    - [Pull Request Size](#pull-request-size)
    - [Response Timeliness](#response-timeliness)
    - [Contributions from other developers](#contributions-from-other-developers)
    - [Cleaning up](#cleaning-up)
    - [Review context](#review-context)
    - [Attitude](#attitude)
  - [Author](#author)
  - [Pull Request Reviewer](#pull-request-reviewer)
    - [Advice and Best Practices](#advice-and-best-practices)
  - [Review Checklist & Areas of Consideration](#review-checklist--areas-of-consideration)
    - [Code Style and Formatting](#code-style-and-formatting)
    - [Testing](#testing)
    - [User Experience](#user-experience)
    - [Housekeeping](#housekeeping)
    - [Data Considerations](#data-considerations)
- [Additional References](#additional-references)

# Project Setup

Set up the project for success before it's underway! Complete the following checklist items when a project is getting started in order to put in place easy conventions and habits for facilitating the code review process without friction.

- [x] Configure Pull Request Labels
- [x] Set up Continuous Integration
- [x] Adopt a branching strategy
- [x] Stand up a staging environment
- [x] Install code linters

## Configure pull request status tracking

Set up GitHub labels in order to easily track the status of a pull request. Configure at least the following labels. Add additional labels as necessary.
-	**wip**: a pull request is in progress. There is no need to review it unless you are specifically *@mentioned*. Use the color orange so it easily stands out.
-	**blocked**: the request cannot be completed. Be sure to indicate why in the description. Use a more subdued color like blue so it doesn't distract from more important items.
-	**for review**: the pull request is ready for review. Use the color yellow so it easily stands out.

## Set up Continuous Integration

Using a Continuous Integration server allows for easy verification that the changes have successfully passed the test suite and linter checks. It allows ensures the checks pass successfully on another machine, other than the machine that the feature was developed on.

## Adopt a branching strategy

Define a branching strategy in your project's documentation, wiki, or Readme. This informs developers how a feature will go through the development process and eventually end up in Production. A good default is to assign `master` as *staging ready* and then use tags to assign a particular commit as *production ready*. This makes it frictionless to additionally maintain a changelog. Also consider using [Git Flow](https://datasift.github.io/gitflow/IntroducingGitFlow.html) as a branching model.

## Stand up a staging environment

Staging environments allow the feature to be reviewed and smoke tested before it goes live in Production. This can catch any issues that the test suite didn't find and also allow the change to be reviewed without necessarily having a local development environment setup. A few tips:
- Deploy `master` to it regularly or automatically
- Ensure it has realistic data to test with
- Keep its architecture similar to the production environment

## Install code linters

Automate style guide and other quality checks by using code linting tools. This allows the reviewer to focus on less nitpicky issues while ensuring these easily correctable issues get addressed before review by a human.

# Pull Request Responsibilities

The pull request process is composed of a series of participants: the author of the changes, the pull request reviewed, and (optionally) any other stakeholders. There are general guidelines that all participants should follow, as well as specific responsibilities for the core participants.

## General Guidelines

### Pull Request Size

To get valuable feedback, a reviewer must be able to easily read the requested change and understand the motivations with little background. Small pull requests greatly improve the ability for a reviewer to give valuable feedback. A small diff also makes a follow-up review upon the implementation of feedback much easier. Aim for pull requests to contain less than 400 lines of diff (GitHub provides diff metrics on the pull request files page).

### Response Timeliness

Pull request reviews and feedback requests should be responded to promptly, ideally by the end of the next business day.

### Contributions from other developers

The author of the pull request is responsible for the proposed changes and as a result, their changes take precedence over contributions from other developers. If another developer wishes to contribute additional changes to the pull request, the second developer should coordinate with the author to facilitate those changes and ensure that they don't conflict with the author's intentions.

### Cleaning up

Don't rewrite git history once you have asked for feedback. Other developers should pull your changes and comment on them, so changing historical commits during review is discouraged. If you must, be sure to coordinate with the team.

Before merging a reviewed and improved pull request, consider grouping related changes and removing fixups through a rebase.

### Review context

Provide context for the motivation of the change in the pull request description. If there isn't enough context, the author and reviewer should review the pull request together.

### Attitude

Be aware of how you ask questions. Suggest approaches or provoke exploration. See [Advice and Best Practices](#advice-and-best-practices).

## Author

The author of the pull request drives this process. This individual is responsible for keeping the process moving and coordinating communication and action as necessary.

### Open Pull Request

Open a pull request as early as possible. This allows other developers to be informed of the changes that you're proposing.

- Set the pull request's status to `wip`
- Document the requirements for the change (if appropriate) in the pull request description (e.g. a link to the feature story)
- Provide before and after screenshots of the changing interface (if relevant)

### Solicit Feedback

Solicit feedback from other team members while the pull request is in progress by *@mentioning* them.

### Prep for Review

Once you consider the change is finished, prepare the pull request for review:

1. Complete a review of the pull request yourself. Specific items to ensure:
  1. Adherence to the project's style guide
  1. Adequate test coverage has been added for the changes
1. Ensure the pull request passes Continuous Integration
1. Update the pull request's status to `for review`
1. Request a review on the pull request to alert your team members that the change is ready for review

### Review Pull Request

Review the pull request, referencing the [Review Checklist & Areas of Consideration](#review-checklist--areas-of-consideration) chart below.

### Address Review Feedback

If the review process yields actionable feedback, respond to the comments left on the pull request until a resolution is obtained. Change the pull request's status back to `wip` while significant feedback is being addressed, as necessary. Push any additional commits required to resolve the feedback to GitHub using [an auto-squashing strategy](https://robots.thoughtbot.com/autosquashing-git-commits). Repeat this loop until all stakeholders are satisfied.

### Re-review

If significant changes were introduced during the feedback review step, initiate another review of the pull request by @mentioning the designated pull request reviewers.

### Sign Off & Prep for Merge

After the pull request has been reviewed, received feedback has been adequately addressed, and all stakeholders have signed off on it, prep the pull request for merge:
-	Rebase the branch onto master and resolve any merge conflicts
-	Squash any fixup commits to clean up the version history, force pushing the branch to GitHub if necessary. Consider setting `--force-with-lease` in your Git config to [protect against accidental overwrites](https://developer.atlassian.com/blog/2015/04/force-with-lease/).
-	Ensure the pull request again passes Continuous Integration

### Merge

Once the pull request has achieved sign off and passed all requirements for merge, @mention the designated pull request merger to have the pull request merged.

## Pull Request Reviewer

When a pull request review has been initiated, complete a code review while referencing the [Review Checklist & Areas of Consideration](#review-checklist--areas-of-consideration) chart below. Upon satisfactory review, leave a comment on the pull request with a :thumbsup:.

**Adopt this mindset:**

> Trust no one. Question everything (kindly). Assume that the author has made mistakes that you need to catch. Save your users from those bugs.

### Advice and Best Practices

-	**Ask questions:** How does this method work? If this requirement changes, what else would have to change? How could we make this more maintainable?
-	**Complement / reinforce good practices:** One of the most important parts of the code review is to reward developers for growth and effort. Few things feel better than getting praise from a peer. Try to offer as many positive comments as possible.
-	**Discuss in person for more detailed points:** On occasion, a recommended architectural change might be large enough that it's easier to discuss it in person rather than in the comments. Similarly, if you're discussing a point and it goes back and forth, it's better to often pick it up in person and finish out the discussion. After the offline discussion, post a quick summary on the pull request so that other team members can follow along.
-	**Explain reasoning:** It's best both to ask if there's a better alternative and justify why you think it's worth fixing. Sometimes it can feel like the changes suggested can seem nit-picky without context or explanation.
-	**Make it about the code:** It's easy to take notes from code reviews personally, especially if we take pride in our work. It's best to make discussions about the code than about the developer. It lowers resistance and it's not about the developer anyway, it's about improving the quality of the code.
-	**Suggest importance of fixes:** Offer many suggestions, not all of which need to be acted upon. Clarifying if an item is important to fix before it can be considered finished is useful both for the reviewer and the reviewee. It makes the results of a review clear and actionable.

## Review Checklist & Areas of Consideration

Use the following areas of consideration for pull request review criteria.

### Code Style and Formatting

| Topic | Things to look for |
| ----- | ------------------ |
| Style Guide Violations  | Violations of the style guide that the linters don't catch yet. Does this change introduce any inconsistencies with the agreed upon conventions outlined in the project's style guide?  |
| Obvious Mistakes  | Typos, poor variable names, unclear method names, large method definitions, removal of commented out code. Logical omissions or errors.  |
| Linting Exceptions | Is there sufficient justification for the addition of the linting exception? Can you suggest a way to avoid the exception? If it takes longer than a short amount of time to come up with alternatives, than otherwise allow it. |
| Usage of frameworks/libraries | Are the included frameworks/libraries being used as designed? Is there a more idiomatic way of using the particular framework/library in question? |
| Maintainability | Are any aspects of the change difficult to understand or follow? Can you suggest an alternative implementation or refactor to increase the readability/understandability and/or maintainability?<br/><br/>Can you gather context for the diff? i.e., if you're totally unfamiliar with that part of the codebase, could you roughly tell what that part of the system does and how the diff affects it?<br/><br/>Can you actually understand what the code is doing? The diff should be understandable by any member of the team. If it is hard to reason about, let the author know. |
| Impact of codebase | Is there a wider impact on the codebase beyond what was changed that has not been considered? |
| Commit messages | The first line should be a capitalized, short (80 characters or less) summary of the commit. Leave a blank line and then write a paragraph about the details of the change. Keep line length less than 80 characters. Your commit message editor should be set up to do this for you. A bulleted list of changes and their motivations is encouraged. Read [this](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html). |

### Testing

| Topic | Things to look for |
| ----- | ------------------ |
| Test Coverage | Is there adequate test coverage for the changed lines or critical code paths based on previously established testing conventions within the codebase? |
| Testing level | Is the code tested at the right level? Can any of the long-running tests be changed to faster executing tests? Faster executing types of tests (e.g. unit tests) are preferred over slower tests (e.g. integration or acceptance tests). |
| Test value | Do the tests (non-trivially) pass, assert a valuable behavior, and do not (if you can help it) test implementation details? Can you suggest any ways of strengthening the tests?<br/><br/>Are the tests thoughtful? Do they cover the failure conditions? Are they easy to read? How fragile are they? How big are the tests? Are they slow? |
| Branching | Do branching paths have adequate test coverage? |
| Boundaries | Are unit tests isolated? Do integration tests assess meaningful behavior? Are integration tests adding too much coverage that should be tested at the unit level? |

### User Experience

| Topic | Things to look for |
| ----- | ------------------ |
| Meets requirements | Does this change meet _**all**_ of the requirements (as specified in the Pivotal story and other sources)? When previewing the change, does it meet both the letter and spirit of the requirements? |
| Edge cases | Can you identify any unhandled edge cases based on the assumptions being made and/or the requirements? |
| User Experience | Does the change result in an intuitive and well-conceived user experience? |
| Adherence to design patterns | Are any interface changes consistent with existing visual design patterns? |
| Smoke testing | Did smoke testing reveal any less than ideal user interactions? |
| Compatibility | Is this change likely to run into any compatibility issues on different devices or in different browsers? If so, were there discussions with the client about a degraded experience on those devices. |

### Housekeeping

| Topic | Things to look for |
| ----- | ------------------ |
| Documentation | Is developer setup and other documentation up-to-date? Will the development team be able to set up/update their development environment based on the documentation? |
| Deployment considerations | If there are infrastructure or dependency changes, are deployment considerations taken into account? Is there a plan established to update non-development/test environments? |
| Stale code | If code was removed as part of this change, is there now unused code (CSS, javascript, ruby methods, metrics tracking, etc.) or files that should also be removed? |
| Redundancy | Does the change introduce new code that effectively does the same thing as existing methods, test steps, etc.? |

### Data Considerations

| Topic | Things to look for |
| ----- | ------------------ |
| Future-proof migrations | If migrations were added, will the migration break in the future?<br/><br/>Migrations that run today can break in the future. The most common cause is migrations that reference application code. Classes and methods can be removed or renamed. See this [awesome blog post](http://blog.testdouble.com/posts/2014-11-04-healthy-migration-habits.html) for examples and solutions. |
| Maintenance window / Downtime | Will this require a maintenance window?<br/><br/>Code gets deployed first, then migrations run. If the code depends on the migration, you should use a maintenance window when deploying the change. Some migrations might take a significant amount of time to run. In general, prefer more frequent, smaller deploys that don't require a maintenance window. |
| Sample data | Will this change affect sample data?<br/><br/>Does the migration add new fields, new models, or remove/change existing fields? If the application uses database seeding or has a sample dataset in the form of a database dump or script, update these items to reflect the new state of the database. Also ensure that running the script, importing the database dump, or running the database seeds from an empty database can be executed successfully after this change. |
| Search indexes | If changes to any search indexes were made, have you verified that the indexes have been properly set up in order to be appropriately updated? |
| Integrity | Are there appropriate database constraints? Do migrations work in both directions? Are there any field type changes that would lose precision?<br/>Are there transactions used in the code? Are there places where a rollback of a transaction is missing and should be included? |

# Additional References

- [10 facts about code reviews and quality](https://blog.codacy.com/10-facts-about-code-reviews-and-quality-c5adf2e869fe#.ae5q5w5hw) *Codacy*
- [Code Review Best Practices](https://www.kevinlondon.com/2015/05/05/code-review-best-practices.html) *Kevin London*
- [Moving Fast With High Code Quality](https://engineering.quora.com/Moving-Fast-With-High-Code-Quality) *Engineering at Quora*
- [Stop More Bugs with our Code Review Checklist](http://blog.fogcreek.com/increase-defect-detection-with-our-code-review-checklist-example/) *Fog Creek*
- [thoughtbot's Code Review Guide](https://github.com/thoughtbot/guides/tree/master/code-review) *thoughtbot*
- [ThinkUp's Pull Request Checklist](https://github.com/ThinkUpLLC/ThinkUp/wiki/Developer-Guide:-Pull-Request-Checklist) *ThinkUp*
- [Effective Code Review](https://alexgaynor.net/2013/sep/26/effective-code-review/) *Alex Gaynor*
- [Giving Better Code Reviews](https://medium.com/@mrjoelkemp/giving-better-code-reviews-16109e0fdd36#.qyki8oniz) *Joel Kemp*
- [What we learned from Google: code reviews aren’t just for catching bugs](https://blog.fullstory.com/what-we-learned-from-google-code-reviews-arent-just-for-catching-bugs-b125a13aa292#.ou9mdrqz5) *Full Story*
- [How terrible code gets written by perfectly sane people](https://chrismm.com/blog/how-terrible-code-gets-written-by-perfectly-sane-people/) *Christian Maioli*
- [Healthy ActiveRecord migrations habits](http://blog.testdouble.com/posts/2014-11-04-healthy-migration-habits.html) *Test Double*

# Contributing

The guide is still a work in progress&mdash;we encourage you to contribute! Please check out the [contribution guidelines](CONTRIBUTING.md) for guidelines on how to proceed. Feel free to open issues or send pull requests with improvements. Thanks in
advance for your help!

# Credits

An initial version of this guide was originally developed at [smartlogic](http://smartlogic.io), a web and mobile development consultancy in Baltimore, MD.
