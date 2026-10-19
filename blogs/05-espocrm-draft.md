# An article can become outdated without a text edit

> Working draft for Shubhi's review · 19 October 2026. This is an AI-assisted proposal-stage article. [Issue #6038](https://github.com/topoteretes/cognee/issues/6038) is published; assignment is pending and no connector has been built or tested. Add your real usage and results, and revise the wording yourself before publication. [Issue draft](../proposals/05-espocrm.md)

A troubleshooting article can contain exactly the same text today as yesterday and still stop being an approved answer.

It may have expired. Someone may have unpublished it. It may have moved out of the category a team uses. A connector that looks only for text edits could miss all three changes.

That is the problem behind my EspoCRM proposal: keep a selected set of support knowledge current enough that an agent does not keep retrieving advice the source has retired.

## The first check would change no words

My proposed example would contain two synthetic troubleshooting articles for different product versions. I would first ask for the applicable guide, then expire one article without editing its body.

After a successful sync, I would check that the expired article's imported evidence is gone. I would also edit the other article and move it between categories to see whether the connector handles those changes independently.

This is a planned test. There is no result to report yet, and I have not run a local EspoCRM instance for it.

## Choose the knowledge base deliberately

EspoCRM contains many kinds of records. The first integration does not need all of them. Its [knowledge-base documentation](https://docs.espocrm.com/user-guide/knowledge-base/) provides a focused starting point around articles, categories and publication rules.

I proposed published, unexpired articles from explicitly selected categories. Each imported document would retain its source identity and category context. General CRM records and support cases would stay outside this initial scope.

The [API documentation](https://docs.espocrm.com/development/api/) supports a dedicated API user with roles and API-key authentication. The connector should use the permissions of that account rather than require broad administrative access for a small article import.

## A modification cursor is not enough

Expiry is the important counterexample. The clock can change an article's eligibility without a new body edit. That means the selected inventory needs to be reevaluated, even when a modification cursor reports nothing new.

The proposed approach is complete scoped reconciliation, with content fingerprints to avoid reimporting unchanged documents. Removing ineligible records would happen only after that inventory has completed successfully.

Pagination needs its own checks. EspoCRM's [list API](https://docs.espocrm.com/development/api/crud/) supports offsets and limits, but the reported total can be a special value rather than a normal count. An implementation that assumes every total is a record count could stop early and wrongly treat unseen articles as missing.

## Access failures need an explanation

A lost permission is not the same event as an article being deleted. A failed later page is not an empty knowledge base either. Both need an explicit failure state before any cleanup decision.

After assignment, I would verify the exact article schema and role behavior on a pinned local release. The planned evidence includes unchanged imports, text edits, expiry, category changes, deletion and interrupted listings. I would inspect actual Cognee retrieval as well as the imported records.

The eventual post should show the article I expired, the result before and after synchronization, and any remaining limits. For now, the proposal is built around a small question worth testing: does yesterday's approved answer remain in memory after the source has retired it?

---

## Evidence to add before publication

- Assigned issue, approved category scope and PR URLs.
- Tested EspoCRM and connector versions and working installation commands.
- Your controlled articles and actual publication/expiry configuration.
- Real retrieval before and after expiry without a body edit.
- Results for edits, category moves, deletion, role changes and failed listings.
- Pagination behavior observed on that release and a documented limitation.
- Your final wording, public blog URL and event submission record.

References: [API/authentication](https://docs.espocrm.com/development/api/) · [List behavior](https://docs.espocrm.com/development/api/crud/) · [Knowledge base](https://docs.espocrm.com/user-guide/knowledge-base/) · [Docker installation](https://docs.espocrm.com/administration/docker/installation/)
