Writing high quality, easily maintainable code: A Guide
=============

# Introduction

Hi there!

This is a guide for writing high quality code. It's a set of principles and practices designed to maximize quality and maintainability without compromising velocity, timeline, or budget.

# Contents
- Introduction
- [tldr;](#tldr)
- [Project Setup](#project-setup)
- [Pull Request Responsibilities](#pull-request-responsibilities)
- [Conclusion](#conclusion)

# tldr;

# Project Setup

Set up the project for success before its underway! Complete the following checklist items when a project is getting started in order to put in place easy conventions and habits for facilitating the code review process without friction.

- [x] Configure Pull Request Labels
- [x] Set up Continuous Integration
- [x] Adopt a branching strategy
- [x] Stand up a staging environment
- [x] Install code linters

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

# Pull Request Responsibilities

The pull request process is composed of a series of participants: the author of the changes, the pull request reviewed, and (optionally) any other stakeholders. There are general guidelines that all participants should follow, as well as specific responsibilities for the core participants.

## General Guidelines

### Pull Request Size

To get valuable feedback, a reviewer must be able to easily read the requested change and understand the motivations with little background. Small pull requests greatly improve the ability for a reviewer to give valuable feedback. A small diff also makes a follow-up review upon the implementation of feedback much easier. Aim for pull requests to contain less than 400 lines of diff<sup>[1](#footnote1)</sup>.

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

Review the pull request, referencing the [Areas of Consideration](#areas-of-consideration) chart below.

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

When a pull request review has been initiated, complete a code review while referencing the [Areas of Consideration](#areas-of-consideration) chart below. Upon satisfactory review, leave a comment on the pull request with a :thumbsup:.

**Adopt this mindset:**

> Trust no one. Question everything (kindly). Assume that the author has made mistakes that you need to catch. Save your users from those bugs.

### Advice and Best Practices

-	**Ask questions:** How does this method work? If this requirement changes, what else would have to change? How could we make this more maintainable?
-	**Compliment / reinforce good practices:** One of the most important parts of the code review is to reward developers for growth and effort. Few things feel better than getting praise from a peer. Try to offer as many positive comments as possible.
-	**Discuss in person for more detailed points:** On occasion, a recommended architectural change might be large enough that it's easier to discuss it in person rather than in the comments. Similarly, if you're discussing a point and it goes back and forth, it's better to often pick it up in person and finish out the discussion. After the offline discussion, post a quick summary on the pull request so that other team members can follow along.
-	**Explain reasoning:** It's best both to ask if there's a better alternative and justify why you think it's worth fixing. Sometimes it can feel like the changes suggested can seem nit-picky without context or explanation.
-	**Make it about the code:** It's easy to take notes from code reviews personally, especially if we take pride in our work. It's best to make discussions about the code than about the developer. It lowers resistance and it's not about the developer anyway, it's about improving the quality of the code.
-	**Suggest importance of fixes:** Offer many suggestions, not all of which need to be acted upon. Clarifying if an item is important to fix before it can be considered finished is useful both for the reviewer and the reviewee. It makes the results of a review clear and actionable.

## Areas of Consideration

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

### User Experience

### Housekeeping

### Data Considerations

# Conclusion


## Credits

An initial version of this guide was originally developed at [smartlogic](http://smartlogic.io), a web and mobile development consultancy in Baltimore, MD.

---

### Footnotes

<a name="footnote1">1</a>: GitHub provides diff metrics on the pull request files page
