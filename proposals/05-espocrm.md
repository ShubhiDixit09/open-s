# Hackathon [Feature]: Add EspoCRM knowledge-base connector

Saved issue text · 19 October 2026 · intended author: ShubhiDixit09.

**Publication status:** published as [#6038](https://github.com/topoteretes/cognee/issues/6038) by ShubhiDixit09. Verified open, unassigned and labelled enhancement on 19 October 2026. Originally a scope proposal; a later [assignment comment](https://github.com/topoteretes/cognee/issues/6038#issuecomment-6268015996) was posted at Shubhi's request. The saved original issue body below is historical.

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

This is a proposed Mergetober integration, not an implementation report. I am awaiting assignment on #6027 and am not requesting another assignment here. No connector code or live test has been completed; coding would wait for assignment.

References:

- [API and authentication](https://docs.espocrm.com/development/api/)
- [List behavior](https://docs.espocrm.com/development/api/crud/)
- [Knowledge-base semantics](https://docs.espocrm.com/user-guide/knowledge-base/)
- [Docker installation](https://docs.espocrm.com/administration/docker/installation/)

## Pre-submission Checklist

- [x] Prior research found no matching issue, PR or named connector; repeat the availability check immediately before posting.
- [x] Provided a clear problem statement and proposed solution.
- [x] Described a specific use case and planned validation.

Editorial status notes above are local bookkeeping, not part of the intended GitHub issue body.
