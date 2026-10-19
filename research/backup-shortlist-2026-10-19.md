# Four backup integrations worth investigating

Checked: 19 October 2026, Asia/Kolkata. Account: ShubhiDixit09.

These four candidates have now been published: Readeck #6035, InvenTree #6036, Kiwi TCMS #6037 and EspoCRM #6038. They remain unassigned and unimplemented. All four and changedetection.io #6027 now have assignment requests; the [current tracker](../CONTRIBUTION-TRACKER.md) records their status.

Publication update: all four were posted from ShubhiDixit09 after explicit confirmation. The [proposal index](../proposals/README.md) links each issue and its separate blog draft. The known new-issue total is now five of five; no new issue slots remain.

## How these were selected

First exclude an existing proposal or implementation. Then consider useful source material, documented API behavior, free realistic testing, a bounded first version and the effort needed for a maintainable contribution. Low visible competition helps avoid duplicate work; it is not a calculated probability of winning.

The four choices below span different workflows. Readeck is the recommended next candidate because its synchronization API addresses both updates and deletions and it has a comparatively simple local deployment. InvenTree is the most distinctive engineering use case. Kiwi TCMS has valuable test reasoning but more complex extraction. EspoCRM offers a focused support-knowledge workflow with a clear content-expiry problem.

| Priority | Candidate | Matching public issues found | Matching public PRs found | Useful first scope |
| --- | --- | ---: | ---: | --- |
| 1 | Readeck | 0 | 0 | Saved articles, highlights and personal annotations with their attribution preserved |
| 2 | InvenTree | 0 | 0 | Selected part notes, parameters with units and direct BOM relationships |
| 3 | Kiwi TCMS | 0 | 0 | Selected test plans, case text, bounded runs and execution evidence |
| 4 | EspoCRM | 0 | 0 | Published, unexpired knowledge articles and their category context |

The counts are empty returned results from all-state public GitHub searches in the topoteretes organization, not a claim about private plans or future submissions. The final exact-name issue/PR checks were repeated independently with a 100-result limit. No labels or open-state filters were applied. Search indexing can lag. Recheck immediately before proposing any candidate.

## 1. Readeck: remember the research someone deliberately saved

A useful connector would preserve both the saved article and the reader's reason for keeping it. Highlights and personal annotations should remain distinguishable from the article author's claims. A generic bookmark-title importer would miss most of that value.

**Evidence:** Readeck documents a synchronization API: GET `/api/bookmarks/sync` reports updated/deleted bookmark IDs, with a `since` filter; POST to the same route can export requested bookmarks and resources including Markdown. Manual API tokens remain supported. Legacy username/password `/api/auth` was removed in 0.22.

**Bounded first version:** explicitly selected saved articles, Markdown text, labels and annotations, stable source IDs and original URLs. Leave images/video processing outside the first proposal. Advance the sync checkpoint only after complete successful ingestion. A personal annotation or label edit must be reflected even if the article text has not changed.

**Demonstration to run after assignment:** save two controlled articles, highlight a claim and add a note explaining why it matters. Retrieve evidence spanning the two articles. Edit only the personal note and sync again; then delete one bookmark and verify its imported records disappear. Check a partial export stream does not advance state or trigger destructive cleanup.

**Unresolved:** the detailed API reference lives inside a running instance. Pin a released version and verify export framing, token read permissions, request options and pagination there. This research has not exercised the API.

**Primary references:** [sync API](https://readeck.org/en/blog/202508-readeck-20/) · [tokens and OAuth](https://readeck.org/en/blog/202511-readeck-21/) · [0.22 authentication changes](https://readeck.org/en/blog/202602-readeck-22/) · [local Compose setup](https://readeck.org/en/docs/compose)

## 2. InvenTree: preserve the reasoning behind an assembly change

Part tables contain more than quantities. Engineering notes can explain a replacement, parameters include units, and BOM relationships identify affected assemblies. The value would come from connecting that recorded reasoning to the right part and revision.

**Evidence:** official schemas expose part, note, parameter and BOM resources. Notes can be scoped by `model_type` and `model_id`; BOMs by their parent part. The REST API supports token authentication and limit/offset pagination with `count`, `next` and `results`.

**Bounded first version:** selected parts and their notes, named parameters with units, and direct BOM relations. Keep instance/type/primary-key identity, revision and source reference. Avoid recursively expanding every related assembly. Leave prices, supplier/customer records, attachments and stock changes outside the initial scope.

**Demonstration to run after assignment:** create a small sensor assembly with two revisions and a recorded component-replacement note. Retrieve the actual note answering why the component was replaced, and identify the affected assembly. Edit the note and BOM independently; delete the note; interrupt pagination. Check the final imported content and failure behavior.

The connector should retrieve recorded decisions. It should not infer that a component is electrically suitable merely because some numeric fields look similar.

**Unresolved:** `latest` documentation may differ from a released server. Pin the server and matching schema. Verify child-resource update behavior, per-resource permissions and deletion semantics; a parent-part timestamp may not cover note or BOM changes. No live API validation has happened.

**Primary references:** [REST API](https://docs.inventree.org/en/latest/api/) · [parts](https://docs.inventree.org/en/latest/api/schema/part/) · [notes and parameters](https://docs.inventree.org/en/latest/api/schema/general/) · [BOM schema](https://docs.inventree.org/en/latest/api/schema/bom/) · [local Docker installation](https://docs.inventree.org/en/latest/start/docker_install/)

## 3. Kiwi TCMS: retain the evidence behind a release test result

A pass/fail count rarely explains a release decision. Test-case text, the environment of a run, an execution comment and a linked bug are more useful together. This connector would preserve that recorded evidence and its source identities.

**Evidence:** the official `tcms-api` client recommends XML-RPC with username/password authentication. Documented methods cover test plans, cases with their latest text, runs, executions and execution comments. Do not assume an API-token or REST interface that the official client does not document.

**Bounded first version:** explicit test-plan IDs and selected or bounded recent runs. Preserve plan, case, run and execution identities. Keep case text and execution comments separate; they can change independently. Exclude unbounded log ingestion and account details.

**Demonstration to run after assignment:** create two release runs. One regression fails in a particular environment with a reproduction detail recorded in an execution comment; a later run passes. Retrieve what failed and the recorded explanation. Edit the expected result and execution comment separately, remove an execution, and test RPC faults and permission failures.

**Unresolved:** the documented filter methods return lists; server-side pagination was not verified. Start with explicit scope limits and clear errors, not an invented cursor protocol. Distinguish review comments from execution comments. Verify how deleted or inaccessible objects appear before implementing cleanup. A local community Docker deployment is documented; it has not been started here.

**Primary references:** [RPC overview](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.html) · [official client](https://github.com/kiwitcms/tcms-api/) · [case API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testcase.html) · [run API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testrun.html) · [execution API](https://kiwitcms.readthedocs.io/en/latest/modules/tcms.rpc.api.testexecution.html) · [local Docker setup](https://kiwitcms.readthedocs.io/en/stable/installing_docker.html)

## 4. EspoCRM: stop retrieving an expired support answer

The useful initial scope is the knowledge base, rather than a dump of every CRM record. Published state, article expiry and category context determine whether an answer should still be available.

**Evidence:** EspoCRM documents a dedicated API user with roles and `X-Api-Key` authentication. Its REST list interface supports `offset`, `maxSize`, ordering, selected fields and filters. Related records also need pagination. The returned total can be `-1` or `-2`, so it cannot always be treated as the record count.

**Bounded first version:** published, unexpired knowledge articles from an explicitly selected category scope, with their category context and source identity. Related support cases should be separately opt-in and agreed with maintainers; they are not necessary to make the first version useful.

**Demonstration to run after assignment:** create two troubleshooting articles that apply to different versions. Retrieve the relevant answer, edit one article, expire the other and sync again. Verify the expired advice is removed from the imported dataset. Test category changes and an interrupted inventory as well.

**Unresolved:** expiry can change eligibility without a content edit, so a modification timestamp alone is insufficient. Verify the released instance's schema, pagination termination and role visibility. A complete scoped reconciliation with content fingerprints is a reasonable proposal, not a tested implementation. Do not interpret permission loss as an ordinary empty dataset. Official free Docker deployment is documented.

**Primary references:** [API and authentication](https://docs.espocrm.com/development/api/) · [list behavior](https://docs.espocrm.com/development/api/crud/) · [search filters](https://docs.espocrm.com/development/api-search-params/) · [relationship API](https://docs.espocrm.com/development/api/relationships/) · [knowledge-base semantics](https://docs.espocrm.com/user-guide/knowledge-base/) · [Docker installation](https://docs.espocrm.com/administration/docker/installation/)

## Reproducible competition checks

| Candidate | Final issue query | Final PR query | Further checks |
| --- | --- | --- | --- |
| Readeck | [org:topoteretes is:issue Readeck](https://github.com/search?q=org%3Atopoteretes%20is%3Aissue%20Readeck&type=issues) | [org:topoteretes is:pr Readeck](https://github.com/search?q=org%3Atopoteretes%20is%3Apr%20Readeck&type=pullrequests) | Alias "read deck" returned unrelated Anki/history material; no Readeck implementation identified |
| InvenTree | [org:topoteretes is:issue InvenTree](https://github.com/search?q=org%3Atopoteretes%20is%3Aissue%20InvenTree&type=issues) | [org:topoteretes is:pr InvenTree](https://github.com/search?q=org%3Atopoteretes%20is%3Apr%20InvenTree&type=pullrequests) | "inventree-python" and "Inven Tree" returned no issues or PRs |
| Kiwi TCMS | [org:topoteretes is:issue "Kiwi TCMS"](https://github.com/search?q=org%3Atopoteretes%20is%3Aissue%20%22Kiwi%20TCMS%22&type=issues) | [org:topoteretes is:pr "Kiwi TCMS"](https://github.com/search?q=org%3Atopoteretes%20is%3Apr%20%22Kiwi%20TCMS%22&type=pullrequests) | Kiwi, TCMS and kiwitcms returned no issues or PRs; broader test-management results were unrelated |
| EspoCRM | [org:topoteretes is:issue EspoCRM](https://github.com/search?q=org%3Atopoteretes%20is%3Aissue%20EspoCRM&type=issues) | [org:topoteretes is:pr EspoCRM](https://github.com/search?q=org%3Atopoteretes%20is%3Apr%20EspoCRM&type=pullrequests) | "Espo CRM" and Espo returned no issues or PRs; Espo code substring matches were unrelated |

Organization code searches and named-path scans of `cognee/dev`, `cognee-community/main` and `cognee-integrations/main` did not identify these connectors. This is a targeted check, not a proof that every line in every branch lacks related functionality.

## Candidates excluded or held in reserve

- **Grist:** existing [proposal #6023](https://github.com/topoteretes/cognee/issues/6023).
- **Kanboard:** existing [proposal #5625](https://github.com/topoteretes/cognee/issues/5625).
- **Docmost:** existing [discussion #5679](https://github.com/topoteretes/cognee/issues/5679), including an API-access constraint.
- **Leantime:** documented wiki discovery can create a wiki if none exists. A read operation with that side effect needs more investigation; it is a weaker choice for a limited proposal slot. [Wiki API](https://docs.leantime.io/api/classes/Leantime/Domain/Wiki/Services/Wiki)
- **Linkwarden:** no matching exact-name issue/PR surfaced and its API is viable. Keep as a reserve rather than pair it with Readeck, since both cover saved reference material. [API](https://docs.linkwarden.app/api/api-introduction)
- **SilverBullet:** no matching exact-name issue/PR surfaced; viable remote Markdown access, but the first proposal needs enough application-specific value beyond reading files. [HTTP API](https://docs.silverbullet.md/HTTP%20API)
- **GLPI:** no matching issue/PR surfaced; useful KB content, but legacy versus OAuth API setup and entity/profile visibility add complexity. [API](https://help.glpi-project.org/documentation/modules/configuration/general/api/api)
- **Woodpecker CI:** no matching proposal surfaced; log noise and the extra forge setup make it a weaker first choice than the four above.

## Next action and contribution discipline

These are now published scope proposals, without parallel assignment requests. Wait for assignment before code and request further assignments only when ready to begin. Do not open any more new integration issues for this event. The [event rules](https://www.wemakedevs.org/hackathons/mergetober/rules) allow at most five new integration issues but also prohibit issue hoarding.

For each eventual PR, record real usage evidence and create a separate public blog post. The [first blog draft](../blogs/01-changedetection-6027-draft.md) is currently a proposal-stage article, not a completed real-world-use report.
