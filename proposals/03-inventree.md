# Hackathon [Feature]: Add InvenTree engineering-knowledge connector

Published issue: https://github.com/topoteretes/cognee/issues/6036

Author: ShubhiDixit09. Saved from the verified GitHub description after its 20 October 2026 update. The description links the existing assignment request; maintainer acceptance and assignment remain pending. The technical proposal is unchanged.

[Blog draft](../blogs/03-inventree-draft.md) · [Posted assignment request](https://github.com/topoteretes/cognee/issues/6036#issuecomment-6268156539) · [Contribution tracker](../CONTRIBUTION-TRACKER.md)

## Problem Statement

A BOM says which parts belong in an assembly. The note beside a part may explain why it was chosen or why it was replaced in the next revision. That reasoning is useful project knowledge, but it stays separate from an agent's memory of the rest of the project.

I searched Cognee's issues, PRs and connector packages for InvenTree and found no matching integration. I'd like to make selected engineering records searchable without turning a parts database into an unbounded data dump.

## Proposed Solution

A dlt document source in `packages/connector/inventree/` in cognee-community, connected to a user's InvenTree instance with a token from an account that has read permissions.

Start with explicitly selected parts: descriptions, engineering notes, named parameters with units, revision information and direct BOM relationships. Each document would retain its source identity and link. Notes and BOM changes need to be checked independently of the parent part's timestamp.

Use bounded, paginated reconciliation and content fingerprints for repeat imports. Remove disappeared records only after successfully reading the complete selected scope. Keep prices, customer/supplier records, attachments and stock mutations outside this initial version.

## Alternatives Considered

A CSV export can preserve part rows but loses the note-to-part and part-to-assembly context. Traversing every related assembly would expand the scope unpredictably, so the first connector should stop at the selected parts and their direct BOM context.

## Use Case

A small sensor assembly has two revisions and a note explaining why a regulator was replaced. The example should retrieve that recorded explanation and the affected assembly. It should not infer electrical suitability from a few parameters. Editing a note or a BOM should update the evidence on the next sync.

## Implementation Ideas

Pin a released InvenTree server and use its matching schema. The documented REST API includes part, note, parameter and BOM resources, token authentication, and limit/offset pagination. Exact child-resource behavior and deletion semantics need verification against that version.

Use a local Docker instance with synthetic engineering records. Cover initial import, unchanged sync, note-only edits, BOM changes, deletion and a failed later page that must not erase previously imported records. Include a README and runnable Cognee retrieval example.

## Additional Context

Proposed for Mergetober. I have requested assignment in [this comment](https://github.com/topoteretes/cognee/issues/6036#issuecomment-6268156539), which also summarizes my approach and validation plan. The detailed scope, use case and testing plan are above.

Maintainer acceptance and assignment are still pending. No connector code or live integration test has been completed; I will wait for assignment before coding.

References:

- [REST API](https://docs.inventree.org/en/latest/api/)
- [Notes and parameter schema](https://docs.inventree.org/en/latest/api/schema/general/)
- [BOM schema](https://docs.inventree.org/en/latest/api/schema/bom/)
- [Docker setup](https://docs.inventree.org/en/latest/start/docker_install/)

## Pre-submission Checklist

- [x] I have searched existing issues to ensure this feature hasn't been requested already
- [x] I have provided a clear problem statement and proposed solution
- [x] I have described my specific use case
