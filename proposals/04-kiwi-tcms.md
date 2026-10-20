# Hackathon [Feature]: Add Kiwi TCMS test-evidence connector

Published issue: https://github.com/topoteretes/cognee/issues/6037

Author: ShubhiDixit09. Saved from the verified GitHub description after its 20 October 2026 update. The description links the existing assignment request; maintainer acceptance and assignment remain pending. The technical proposal is unchanged.

[Blog draft](../blogs/04-kiwi-tcms-draft.md) · [Posted assignment request](https://github.com/topoteretes/cognee/issues/6037#issuecomment-6269614174) · [Contribution tracker](../CONTRIBUTION-TRACKER.md)

## Problem Statement

Knowing that a test failed is less useful than knowing what was tested, on which environment, and what the tester recorded about the failure. Kiwi TCMS keeps that context across plans, cases, runs and execution comments. An agent should be able to retrieve the recorded evidence behind a release decision.

I found no Kiwi TCMS connector in the Cognee organization searches or existing connector packages.

## Proposed Solution

Add a read-only dlt document source under `packages/connector/kiwi-tcms/` in cognee-community. Use the official `tcms-api` client and its documented authenticated XML-RPC interface.

The user would select test-plan IDs and a bounded set of runs. Import case text, run/environment context, execution results and execution comments while preserving their source identities and relationships. Keep case text and execution comments distinct because they can change separately.

The first version would use bounded reconciliation rather than promise an unverified server-side cursor. If extraction fails or the selected scope exceeds a configured limit, report an incomplete run without treating missing rows as deletions. Credentials and general account data would stay out of documents.

## Alternatives Considered

A report of pass/fail totals leaves out the explanation. Ingesting every execution or raw log would add a lot of noise. Selected plans and runs provide a smaller, useful starting point.

## Use Case

Two release runs contain the same regression case. It fails in one environment, with the reproduction detail recorded in an execution comment, and passes in a later run. The example should retrieve the failure and its recorded context, without inventing an explanation that is absent from the source.

## Implementation Ideas

Verify the RPC contract on a pinned community Docker release. The official API covers plans, cases with their latest text, runs, executions and comments. Pagination and deletion behavior still need checking; no live API validation has happened yet.

Test unchanged imports, case-text edits, execution-comment edits, removed executions, permission failures and RPC faults. Changes to a child record must be detected even if its parent appears unchanged. Include a small example, source references and actual Cognee retrieval checks.

## Additional Context

Proposed for Mergetober. I have requested assignment in [this comment](https://github.com/topoteretes/cognee/issues/6037#issuecomment-6269614174), which also summarizes my approach and validation plan. The detailed scope, use case and testing plan are above.

Maintainer acceptance and assignment are still pending. No connector code or live integration test has been completed; I will wait for assignment before coding.

References:

- [Official client](https://github.com/kiwitcms/tcms-api/)
- [RPC overview](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html)
- [Execution API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testexecution.html)
- [Local Docker installation](https://kiwitcms.readthedocs.io/en/stable/installing_docker.html)

## Pre-submission Checklist

- [x] I have searched existing issues to ensure this feature hasn't been requested already
- [x] I have provided a clear problem statement and proposed solution
- [x] I have described my specific use case
