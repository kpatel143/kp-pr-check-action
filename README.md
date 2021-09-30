# pr-compliance-action

This action is meant to help in managing inbound PRs that may need adjustment other than code.

## Functionality

It looks for the following:
- [x] PR Title formatted according to [@commitlint/conventional-commit](https://www.conventionalcommits.org/en/v1.0.0/).
- [x] PR Body refers to an issue, as detected by a regular expression
- [x] PR originates from a protected branch e.g. "main", (based on head ref)
- [x] PR includes modifications to specific files that should be reviewed carefully (e.g. package.json)

## Behavior

This action drives the following outcomes:

Check | Outcome on Failure
--- | ---
PR Title Lint | Action shows as failed check. Action leaves comment.
PR Refers to Issue | Action closes issue. Action leaves comment.
PR Originates from Protected Branch | Action closes issue. Action leaves comment.
PR Avoids Watched Files | Action leaves comment.

## Inputs

All inputs are required and all have default values. The only input absolutely require to be specified in a workflow file is the `repo-token` input.

Name | Default | Description
--- | --- | ---
repo-token | (Blank) | Access token for which this action will run. This action uses `@actions/core` library.
ignore-authors | dependabot<br/>dependabot[bot] | If the action detects that the PR author is one of these logins, it will skip checks and set all outputs to `true`.
base-comment | (see [action.yml](./action.yml)) | Preamble to any comment the action leaves on the PR.
body-regex | (fix|resolv|close)(es)* #\d+ | Regular expression to identify whether the PR body refers to an issue.
body-auto-close | true | Whether or not to auto-close on failed check of PR Body
body-comment | (see [action.yml](./action.yml)) | Comment to leave on PR on failed check of PR Body
protected-branch | "main" | Branch that check should ensure that PR does not use as it's head.
protected-branch-auto-close | true | Whether or not to auto-close on failed check of PR head branch
protected-branch-comment | (see [action.yml](./action.yml)) | Comment to leave on PR on failed check of PR head branch.
title-check-enable | true | Whether or not to lint PR title per @commitlint/conventional-commit.
title-comment | (see [action.yml](./action.yml)) | Comment to leave on PR on failed check of PR title per @commitlint/conventional-commit
watch-files | (Blank) | Files to flag for modifications (e.g. package.json)
watch-files-comment | (see [action.yml](./action.yml)) | Comment to leave on PR when watched files have been changed.

## Outputs

Each check performed is also manifested in an output.

Name | Description
--- | ---
body-check | Result of match for PR Body against configured regex.
branch-check | Result of check to ensure PR head is not protected branch.
title-check | Result of check to ensure PR title is formatted per @commitlint/conventional-commit
watch-files-check | Result of check for watched files having been modified. True if no modifications found to watched files.
