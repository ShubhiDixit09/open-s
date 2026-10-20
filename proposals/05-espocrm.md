# Hackathon [Feature]: Add EspoCRM knowledge-base connector

Published issue: https://github.com/topoteretes/cognee/issues/6038

Author: ShubhiDixit09. Saved from the verified GitHub description after its 20 October 2026 update. The description links the existing assignment request; maintainer acceptance and assignment remain pending. The technical proposal is unchanged.

[Blog draft](../blogs/05-espocrm-draft.md) · [Posted assignment request](https://github.com/topoteretes/cognee/issues/6038#issuecomment-6268015996) · [Contribution tracker](../CONTRIBUTION-TRACKER.md)

## Problem Statement

A support article can stop being valid without its text changing: it may expire, become unpublished, or move outside the category a team uses. Importing the article once leaves an agent with advice the source no longer considers current.

EspoCRM's knowledge base exposes a useful, focused integration surface. I found no matching EspoCRM issue, PR or existing connector in the Cognee organization searches.

## Proposed Solution

A dlt document source under `packages/connector/espocrm/` in cognee-community, using a dedicated role-scoped API user and `X-Api-Key` authentication.

The first version would import published, unexpired knowledge articles from explicitly selected categories, keeping article identity, category context and source references. Evaluate article eligibility on each complete reconciliation; a modification-time cursor alone cannot catch expiry.

Use paginated listings and content fingerprints to avoid repeating unchanged documents. Remove imported articles that are confirmed deleted or no longer eligible after a successful scoped inventory. Authentication errors, changed permissions or incomplete listings must be reported instead of being treated as an empty source. General CRM records and support-case ingestion would be outside the first version.

## Alternatives Considered

Exporting all CRM records makes the scope and privacy boundaries harder to explain. A one-time file export can ingest articles but cannot keep their publication and expiry state current. A focused knowledge-base connector is easier to test and maintain.

## Use Case

Two troubleshooting guides apply to different product versions. The example would retrieve the appropriate guide, edit one article, then expire the other without editing its body. After the next successful sync, the expired article's imported evidence should be removed.

## Implementation Ideas

Pin a local EspoCRM release and verify its article schema, roles, filtering and pagination. List endpoints support `offset`, `maxSize`, ordering and selected fields; the reported total is not always a count, so termination needs explicit tests.

Use the official Docker setup with synthetic support articles. Check unchanged syncs, edits, expiry, category moves, deletion, role changes and a failed later page. Include an example demonstrating actual Cognee retrieval and a clean-install README.

## Additional Context

Proposed for Mergetober. I have requested assignment in [this comment](https://github.com/topoteretes/cognee/issues/6038#issuecomment-6268015996), which also summarizes my approach and validation plan. The detailed scope, use case and testing plan are above.

Maintainer acceptance and assignment are still pending. No connector code or live integration test has been completed; I will wait for assignment before coding.

References:

- [API and authentication](https://docs.espocrm.com/development/api/)
- [List behavior](https://docs.espocrm.com/development/api/crud/)
- [Knowledge-base semantics](https://docs.espocrm.com/user-guide/knowledge-base/)
- [Docker installation](https://docs.espocrm.com/administration/docker/installation/)

## Pre-submission Checklist

- [x] I have searched existing issues to ensure this feature hasn't been requested already
- [x] I have provided a clear problem statement and proposed solution
- [x] I have described my specific use case
