# The article's claim and my note are different evidence

> **Working draft — status reviewed 20 October 2026.**
>
> [Published proposal #6035](https://github.com/topoteretes/cognee/issues/6035) · [Assignment request](https://github.com/topoteretes/cognee/issues/6035#issuecomment-6268005185) · [Saved proposal](../proposals/02-readeck.md)
>
> Maintainer acceptance and assignment are pending. No connector implementation, PR or live test result exists yet; the examples below are planned. This AI-assisted draft needs Shubhi's own review and actual usage evidence before it becomes the PR's final public blog.

Saving an article is easy. Remembering why it was useful is harder.

A bookmark might contain a careful technical explanation, a highlighted paragraph and a note saying, “Check this before we change our retry behavior.” The article's author did not write that note. It belongs to the reader and refers to a particular project. If both end up in an agent's memory as one undifferentiated block, a useful distinction disappears.

That is the reason behind my Readeck connector proposal. I want to preserve the saved material and the reader's context, with enough attribution to tell them apart.

## The example I want to try

My planned example uses two technical articles that I control. One discusses retry timing; the other describes how duplicate operations can occur. I would save both in Readeck, highlight one claim and add a personal note about its relevance to a project.

Then I would ask Cognee to retrieve the evidence relevant to that project decision. A useful result would lead back to the source article and keep the annotation identifiable as a reader's note. It should not present the note as something the article's author concluded.

This is a planned demonstration. I have not run it, and I do not yet know how well retrieval will preserve that distinction.

## Readeck already has a sync interface

The [Readeck synchronization API announcement](https://readeck.org/en/blog/202508-readeck-20/) describes a way to obtain changed and deleted bookmark IDs, then export requested resources including Markdown. That is a better starting point than repeatedly downloading every saved page.

Authentication needs care too. Readeck still supports manual API tokens, while the old username/password endpoint was removed in [version 0.22](https://readeck.org/en/blog/202602-readeck-22/). Copying an older example would give new users a broken setup.

The detailed API documentation is available inside a running instance. After assignment, I would pin a release and check its actual export format, token permissions, selection options and response boundaries before building the connector around them.

## An annotation edit is a real update

The first update I want to test would change only the personal note. The article itself would stay untouched. If the connector looks only for article-body changes, it could miss the most useful new context.

I would also repeat an unchanged import and look for duplicates, then remove a bookmark and verify that its imported material disappears. A separate failure test would interrupt an export before it finishes. That incomplete read must not move the checkpoint forward or erase earlier documents.

This is where I expect the implementation work to be: defining what a complete import means and preserving that boundary when the network fails.

## Keep the first version small

I proposed selected bookmarks, article Markdown, labels and available annotations, with stable identities and source references. Images and video processing can wait. The first example needs to show that saved research and the reader's reasoning survive a round trip into searchable memory.

Once there is an assigned issue and working code, this article will include the exact setup, actual retrieval output and whatever limitations the experiment exposes. Until then, it is the question I want the integration to answer, rather than a report that it already does.

---

## Evidence to add before publication

- Assigned issue and PR URLs; tested Readeck and connector versions.
- Your actual reason for using the workflow and the two authorized articles.
- Commands and clean-install results you obtained yourself.
- Source versus annotation attribution in the real retrieval output.
- Results for unchanged sync, annotation-only edit, deletion and partial export.
- One limitation or failed query, with your explanation.
- Your final wording, public blog URL and event-form submission record.

References: [API sync](https://readeck.org/en/blog/202508-readeck-20/) · [Authentication](https://readeck.org/en/blog/202602-readeck-22/) · [Local Compose setup](https://readeck.org/en/docs/compose)
