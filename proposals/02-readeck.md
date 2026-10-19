# Hackathon [Feature]: Add Readeck saved-research connector

Saved issue text · 19 October 2026 · intended author: ShubhiDixit09.

**Publication status:** published as [#6035](https://github.com/topoteretes/cognee/issues/6035) by ShubhiDixit09. Verified open, unassigned and labelled enhancement on 19 October 2026. Originally a scope proposal; a later [assignment comment](https://github.com/topoteretes/cognee/issues/6035#issuecomment-6268005185) was posted at Shubhi's request. The saved original issue body below is historical.

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

Proposed for Mergetober. I am currently awaiting assignment on #6027; this is a separate scope proposal, not a request for another assignment. No connector code or live test has been completed. Any implementation would wait for assignment.

References:

- [Synchronization API](https://readeck.org/en/blog/202508-readeck-20/)
- [Authentication changes](https://readeck.org/en/blog/202602-readeck-22/)
- [Local setup](https://readeck.org/en/docs/compose)

## Pre-submission Checklist

- [x] Searched existing issues and PRs for this integration; repeat immediately before posting if delayed.
- [x] Provided a clear problem statement and proposed solution.
- [x] Described a specific use case and planned validation.

Editorial status notes above are local bookkeeping, not part of the intended GitHub issue body.
