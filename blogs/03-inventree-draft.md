# The reason a part changed belongs with the BOM

> **Working draft — status reviewed 20 October 2026.**
>
> [Published proposal #6036](https://github.com/topoteretes/cognee/issues/6036) · [Assignment request](https://github.com/topoteretes/cognee/issues/6036#issuecomment-6268156539) · [Saved proposal](../proposals/03-inventree.md)
>
> Maintainer acceptance and assignment are pending. No connector implementation, PR or live test result exists yet; the examples below are planned. This AI-assisted draft needs Shubhi's own review and actual usage evidence before it becomes the PR's final public blog.

A bill of materials can tell you which component is in an assembly. It may not tell you why the previous component was replaced.

That explanation might be in an engineering note attached to a part. Another record identifies the revision, and a parameter has a value whose unit matters. A useful answer needs those records together.

My InvenTree proposal focuses on that recorded reasoning. The goal is to retrieve the note behind a change and show which assembly it concerns.

## A small assembly with a reason to remember

For the proposed example, I would create a synthetic sensor assembly with two revisions. A note would record why a regulator was replaced, and the BOM would show where it belongs.

The question would be straightforward: “Why did revision B replace this regulator?” The expected evidence is the note actually entered in InvenTree, along with the relevant part and assembly references.

If the note does not contain a reason, the integration cannot supply one. Numeric fields should not become an invented explanation of electrical suitability. This example is about recalling a documented decision, not recommending a component.

I have not created this test instance or run the query yet.

## The relationships are part of the source

InvenTree's [REST documentation](https://docs.inventree.org/en/latest/api/) and schemas describe parts, notes, parameters and BOM resources. Those separate records give the connector more useful context than a flat export of part names.

The proposed first version would let a user select parts, then import their engineering notes, named parameters with units, revision information and direct BOM relationships. Stable source identities and references would let someone inspect the original record.

I would keep the expansion bounded. Selecting one part should not silently import every assembly, supplier and stock record connected to it. Prices, customer records and inventory changes are outside the initial proposal.

## The note may change while the part looks unchanged

A sync based only on the parent part's timestamp could miss an edited note or BOM relationship. After assignment, I would verify the actual behavior of those child resources on a released server before choosing a cursor strategy.

The planned checks would edit the note and BOM separately, repeat an unchanged import, and delete the note. Another check would interrupt a later page of the API response. The connector needs to distinguish an incomplete inventory from confirmed disappearance before removing anything from Cognee.

Those checks need to reach the imported records and retrieval results. Successfully sending a tombstone alone would not prove that obsolete evidence has disappeared.

## Start with a released server

InvenTree's latest documentation can differ from a stable release. I would pin a local Docker deployment and use the matching schema, then test the README from a clean environment.

The article that accompanies the eventual PR should show the assembly I actually used, the note I changed, and the results before and after synchronization. It should also explain any limits in how the connector represents revisions and related parts. At this stage, those remain planned experiments.

---

## Evidence to add before publication

- Assigned issue, approved scope and PR URLs.
- Tested server, schema and connector versions.
- Your synthetic assembly, revision details and source note.
- Actual retrieval evidence for the recorded decision.
- Separate note/BOM edit results, deletion and incomplete-pagination recovery.
- Clean-install commands, limitations and your final wording.
- Public blog URL and event submission record.

References: [API](https://docs.inventree.org/en/latest/api/) · [Notes/parameters](https://docs.inventree.org/en/latest/api/schema/general/) · [BOM](https://docs.inventree.org/en/latest/api/schema/bom/) · [Docker setup](https://docs.inventree.org/en/latest/start/docker_install/)
