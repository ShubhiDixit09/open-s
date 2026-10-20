# Hackathon [Feature]: Add changedetection.io watched-page history connector

Published issue: https://github.com/topoteretes/cognee/issues/6027

Author: ShubhiDixit09. Saved from the verified GitHub description after its 20 October 2026 update. The description links the existing assignment request; maintainer acceptance and assignment remain pending. The technical proposal is unchanged.

[Blog draft](../blogs/01-changedetection-6027-draft.md) · [Posted assignment request](https://github.com/topoteretes/cognee/issues/6027#issuecomment-6265201081) · [Contribution tracker](../CONTRIBUTION-TRACKER.md)

## Problem Statement

Teams use changedetection.io to watch dependency documentation, migration guides and deprecation notices. Its retained text snapshots contain useful project knowledge: what the page said, when it was observed, and what changed. A Cognee connector could make that evidence searchable alongside a team's own documentation.

I searched issues and PRs across the Cognee organization and found no changedetection.io integration. The completed web-scraping issue #1351 covers fetching pages; this proposal reads snapshots already captured by changedetection.io.

## Proposed Solution

- A dlt document source in `cognee-community/packages/connector/changedetection/`, using the instance URL and `x-api-key` authentication.
- Explicit watch selection and a configurable, bounded number of retained text snapshots per watch. Each document includes a stable identity, safe source reference and observation time, with historical versions clearly identified.
- Read the watch inventory, history index and text snapshots through API v1. Repeated syncs skip unchanged documents; metadata changes are reconciled too.
- Remove imported records for deleted watches or pruned snapshots only after a complete successful reconciliation. Failed reads or snapshots disappearing mid-run must not trigger deletion.
- Export only approved metadata fields; exclude watch authentication headers, browser steps and notification credentials. The connector never changes watches or triggers rechecks.

## Alternatives Considered

_No response_

## Use Case

The example would follow changes to documentation pages, so a developer can retrieve captured versions of upgrade instructions with their source and observation time alongside the team's own documentation.

## Implementation Ideas

Validation: a local changedetection.io instance watching two controlled documentation pages; import, unchanged sync, page edit, retained-history removal and watch deletion. Add offline tests for failure recovery, stable IDs and secret exclusion, plus setup instructions and a runnable example.

## Additional Context

Proposed for Mergetober. I have requested assignment in [this comment](https://github.com/topoteretes/cognee/issues/6027#issuecomment-6265201081), which also summarizes my approach and validation plan. The detailed scope, use case and testing plan are above.

Maintainer acceptance and assignment are still pending. No connector code or live integration test has been completed; I will wait for assignment before coding.

References:
- [changedetection.io API v1](https://changedetection.io/docs/api_v1/index.html)
- [Local installation](https://github.com/dgtlmoon/changedetection.io/blob/master/README.md#installation)

## Pre-submission Checklist

- [x] I have searched existing issues to ensure this feature hasn't been requested already
- [x] I have provided a clear problem statement and proposed solution
- [x] I have described my specific use case
