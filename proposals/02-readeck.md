# Hackathon [Feature]: Add Readeck saved-research connector

Published issue: https://github.com/topoteretes/cognee/issues/6035

Author: ShubhiDixit09. Saved from the verified GitHub description after its 20 October 2026 update. The description links the existing assignment request; maintainer acceptance and assignment remain pending. The technical proposal is unchanged.

[Blog draft](../blogs/02-readeck-draft.md) · [Posted assignment request](https://github.com/topoteretes/cognee/issues/6035#issuecomment-6268005185) · [Contribution tracker](../CONTRIBUTION-TRACKER.md)

## Problem Statement

A saved article and a reader's notes answer different questions. The article explains the subject; a highlight or annotation explains why the reader kept it. Readeck holds both, but I couldn't find a Readeck integration in Cognee's issues, PRs or existing connector packages.

A connector would let someone search their chosen reading material alongside project notes without losing which words came from the article and which came from them.

## Proposed Solution

Add a dlt document source under `packages/connector/readeck/` in cognee-community. Users would connect to their own instance with an API token and select the bookmarks to include.

The first version would import article Markdown, original URL, labels and available highlights/annotations, with explicit attribution and stable bookmark identities. It would use Readeck's synchronization API for updates and deleted IDs, and commit its checkpoint only after a successful import. Changes to annotations or labels need to count even when the article body is unchanged.

Images, video processing and changes to the source library are outside this first scope.

## Alternatives Considered

Exporting articles as files can get the text into Cognee once, but later annotation edits and bookmark deletions are easy to miss. Importing only URLs and titles loses the saved content and reading context.

## Use Case

The example would save two controlled technical articles, highlight a claim, and attach a personal note about its relevance to a project. It should then be possible to retrieve the original evidence and the separate note. Editing only the note must update the imported material; deleting a bookmark must remove it after a successful sync.

## Implementation Ideas

Before implementation, pin a released Readeck version and verify its in-instance API documentation: export framing, selection options, token permissions and paging. Current docs describe GET `/api/bookmarks/sync` for changed/deleted IDs and POST on the same route for requested resources, including Markdown. Manual API tokens are supported; the old username/password `/api/auth` route was removed in 0.22.

Validation would use the local Compose deployment. Tests would cover unchanged imports, annotation-only edits, deletes and a truncated export that leaves the previous checkpoint and documents intact. Include a clean-install example and README.

## Additional Context

Proposed for Mergetober. I have requested assignment in [this comment](https://github.com/topoteretes/cognee/issues/6035#issuecomment-6268005185), which also summarizes my approach and validation plan. The detailed scope, use case and testing plan are above.

Maintainer acceptance and assignment are still pending. No connector code or live integration test has been completed; I will wait for assignment before coding.

References:
- [Synchronization API](https://readeck.org/en/blog/202508-readeck-20/)
- [Authentication changes](https://readeck.org/en/blog/202602-readeck-22/)
- [Local setup](https://readeck.org/en/docs/compose)

## Pre-submission Checklist

- [x] I have searched existing issues to ensure this feature hasn't been requested already
- [x] I have provided a clear problem statement and proposed solution
- [x] I have described my specific use case
