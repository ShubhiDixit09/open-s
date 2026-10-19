# Hackathon [Feature]: Add Kiwi TCMS test-evidence connector

Saved issue text · 19 October 2026 · intended author: ShubhiDixit09.

**Publication status:** published as [#6037](https://github.com/topoteretes/cognee/issues/6037) by ShubhiDixit09. Verified open, unassigned and labelled enhancement on 19 October 2026. A later [assignment comment](https://github.com/topoteretes/cognee/issues/6037#issuecomment-6269614174) was posted at Shubhi's request. The saved original body below is historical.

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

Proposed for Mergetober scope feedback. I have an assignment request pending on #6027 and am not asking for a parallel assignment here. This is researched scope, with no connector implementation or live test completed. Any coding would wait for assignment.

References:

- [Official client](https://github.com/kiwitcms/tcms-api/)
- [RPC overview](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html)
- [Execution API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testexecution.html)
- [Local Docker installation](https://kiwitcms.readthedocs.io/en/stable/installing_docker.html)

## Pre-submission Checklist

- [x] Prior research found no matching issue, PR or named connector; repeat the availability check immediately before posting.
- [x] Provided a clear problem statement and proposed solution.
- [x] Described a specific use case and planned validation.

Editorial status notes above are local bookkeeping, not part of the intended GitHub issue body.
