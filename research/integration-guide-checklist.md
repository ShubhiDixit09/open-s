# Official integration guide: requirements for our proposals

Read on 19 October 2026. [Building a cognee Integration](https://app.notion.com/p/topoteretes/Building-a-cognee-Integration-3c937007fa8281b18e83fcc24f86004a).

The guide describes code checked against the dev branch on 27 August 2026. Recheck the installed release and current repository contracts after assignment rather than treating all implementation details as timeless.

## Architecture choice

All five current proposals fit **Path B**: a caller periodically pulls a selected corpus through a plain dlt source. These are standalone community packages consumed through Cognee's ingestion API. Their proposed scopes do not require a Cognee OAuth consent screen or webhook integration.

| Proposed integration | Issue | Intended source material |
| --- | --- | --- |
| changedetection.io | [#6027](https://github.com/topoteretes/cognee/issues/6027) | Selected captured text snapshots and observation metadata |
| Readeck | [#6035](https://github.com/topoteretes/cognee/issues/6035) | Saved articles and attributed reader annotations |
| InvenTree | [#6036](https://github.com/topoteretes/cognee/issues/6036) | Engineering notes with part and direct BOM context |
| Kiwi TCMS | [#6037](https://github.com/topoteretes/cognee/issues/6037) | Selected test case/run/execution evidence |
| EspoCRM | [#6038](https://github.com/topoteretes/cognee/issues/6038) | Eligible knowledge articles and category context |

These are architecture notes, not integration code. Maintainer assignment is still required before implementation.

## Path B checklist

- Use a dlt source/resource with stable primary keys for repeated synchronization.
- Give each connector/account scope its own dataset so state and deletion handling remain scoped.
- Explicitly mark prose sources for document ingestion using the supported document-source contract. The default relational route produces structured rows rather than normal prose entity extraction.
- Hand the actual dlt source to Cognee. Pre-flattening everything into strings can bypass source-aware reconciliation.
- Keep rendered content deterministic. Include genuine source timestamps where relevant; do not add the current sync time or other changing decoration to otherwise unchanged content.
- Disable self-improvement on repeated syncs as the guide recommends, and verify the precise API options against the chosen Cognee release.
- Set and test the intended write disposition explicitly. The guide flags an inconsistency between an API docstring and the actual default; do not rely on that default.
- Verify deletion behavior, including the last remaining source record. The guide says orphan cleanup is skipped for append mode and empty fresh row sets. Check current behavior before promising automatic cleanup; a complete but empty source and an incomplete failed read are different cases.
- Use a separately installable package named `cognee-community-connector-<source>`, with packaging metadata, setup/credential instructions, runnable example and meaningful tests. Add the connector to the repository's README tables as required.
- Branch community work from main. Follow current repository checks, and explain non-obvious choices in module documentation.

## Folder discrepancy verified against the repository

The guide writes `packages/connectors/<name>/` in two places. The actual [Notion reference directory](https://github.com/topoteretes/cognee-community/tree/main/packages/connector/notion) uses **`packages/connector/`**, singular. This matches the event resources and all five submitted proposals. Verify the live layout again when setting up the assigned work.

## Evidence before a PR

Run the selected connector against a real authorized account or local service instance. Verify initial retrieval, unchanged sync, independent metadata/child-record changes, deletion, final-record deletion and recovery from partial/failed reads. Check clean package installation and every documented command.

The event additionally requires signed-off commits, the DCO affirmation, original work the contributor understands and a separate public real-world-use blog per PR. These requirements complement the integration guide; the guide does not remove the assignment-first rule.

References: [integration guide](https://app.notion.com/p/topoteretes/Building-a-cognee-Integration-3c937007fa8281b18e83fcc24f86004a) · [community repository](https://github.com/topoteretes/cognee-community) · [event rules](https://www.wemakedevs.org/hackathons/mergetober/rules) · [event resources](https://www.wemakedevs.org/hackathons/mergetober/resources)
