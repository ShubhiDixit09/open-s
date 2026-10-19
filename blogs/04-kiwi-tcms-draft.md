# A red test result is only the start of the story

> Working draft for Shubhi's review · 19 October 2026. This is an AI-assisted proposal-stage article. [Issue #6037](https://github.com/topoteretes/cognee/issues/6037) is published; assignment is pending and no connector has been built or tested. Replace planned behavior with your own observed results before publication. [Issue draft](../proposals/04-kiwi-tcms.md)

A failed test tells you where to look. The case description, the environment and the tester's comment may explain what actually happened.

That context can matter several releases later. Was this the same failure? Did it happen on the same environment? Was the test's expected result changed after the original run?

Kiwi TCMS keeps records that can help answer those questions. My proposed connector would bring selected test evidence into Cognee while preserving which plan, case, run and execution it came from.

## Two runs, one regression

The example I want to try would use a disposable local instance and two release runs. A regression would fail in one environment. Its execution comment would contain a reproduction detail. A later run would pass.

I would ask for the failure before the release and the evidence recorded about it. A useful response should lead back to the test record and the comment. If nobody recorded why it failed, the answer should not manufacture a root cause.

I would then change the test's expected result and an execution comment separately. They describe different things, and they need to remain distinguishable in the imported material.

These are planned records and checks. I have not run this demonstration.

## Follow the actual API

The official [tcms-api client](https://github.com/kiwitcms/tcms-api/) documents an authenticated XML-RPC interface. The server's [RPC reference](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html) covers the plans, cases, runs and executions needed for this scope.

That matters for setup. I would use the documented client and authentication rather than assume a REST endpoint or personal-access-token flow exists.

The filter methods return lists, but I have not verified a server-side pagination contract. The first proposal therefore uses explicit plan selection and bounded runs. An oversized or incomplete extraction should produce a clear error instead of quietly returning a partial history.

## Repeated imports need more than a new-run check

An execution comment may be edited after the run is complete. A case's latest text may change later still. Looking only for newly created runs could leave stale evidence in memory.

After assignment, I would test those updates independently, followed by an unchanged sync and a removed execution. RPC faults and permission failures also need explicit handling. A failed request must not be mistaken for a successful list containing no records.

The final verification should inspect both the imported data and actual Cognee retrieval. That would establish whether identities and context survive the sync, rather than merely proving that an API call returned successfully.

## A useful limit

The first connector should serve selected release-test knowledge. Pulling every execution and every possible log would make the example harder to understand and the resulting memory noisier.

Once the integration is assigned and built, this post should include the real test records, exact versions, commands, outputs and at least one limitation. Until then, it describes the evidence I want to preserve and the checks I need to run.

---

## Evidence to add before publication

- Assigned issue, approved plan/run scope and PR URLs.
- Tested server and official-client versions; actual setup commands.
- Your controlled failing and passing runs and recorded reproduction detail.
- Actual retrieval output, with source identities.
- Case-text and comment edits, execution removal, RPC-fault and permission checks.
- Scope limits, one failed or limited result and your explanation.
- Your final wording, public blog URL and event submission record.

References: [Official client](https://github.com/kiwitcms/tcms-api/) · [RPC API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html) · [Executions](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testexecution.html) · [Local installation](https://kiwitcms.readthedocs.io/en/stable/installing_docker.html)
