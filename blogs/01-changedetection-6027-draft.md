# Remembering what changed in a migration guide

> **Working draft — status reviewed 20 October 2026.**
>
> [Published proposal #6027](https://github.com/topoteretes/cognee/issues/6027) · [Assignment request](https://github.com/topoteretes/cognee/issues/6027#issuecomment-6265201081) · [Saved proposal](../proposals/01-changedetection.md)
>
> Maintainer acceptance and assignment are pending. No connector implementation, PR or live test result exists yet; the examples below are planned. This AI-assisted draft needs Shubhi's own review and actual usage evidence before it becomes the PR's final public blog.

A migration guide can change while the link stays the same. That is a small detail with an awkward consequence: a note that was correct when you wrote it may now disagree with the page it points to.

For my first Mergetober proposal, I chose changedetection.io. It already watches pages and keeps text snapshots. I want to bring selected snapshots into Cognee so that a developer can retrieve the wording behind a decision, along with the source and the time it was observed.

The question I want to explore is: **what did this guide say when we made that decision, and what does the latest captured version say?**

## A small example

Suppose a dependency's upgrade guide initially says that an old configuration option is supported. A later revision removes it. A team's internal note still recommends the old option.

Searching only the latest page misses the context behind the note. Keeping both versions without identifying their dates creates a different problem: an agent might retrieve the old instruction as if it were current.

My proposed example will use two documentation pages that I control. I will change one instruction, let changedetection.io capture the edit, then check whether the imported documents preserve the old wording, the new wording and their observation times. These are planned checks, not results I have obtained.

## Read the snapshots that already exist

The connector would read from a user's changedetection.io instance. It would not run a second crawler or create watches for them.

The [API reference](https://changedetection.io/docs/api_v1/index.html) documents a watch inventory, a history index for each watch and access to individual text snapshots. Authentication uses an API key in the `x-api-key` header.

I proposed explicit watch selection and a bounded number of retained snapshots. Each imported document would carry its watch identity, a safe source reference and an observation timestamp. Older captures need to be clearly identified as historical. The observation time tells us when the monitor captured something; it does not prove when the publisher changed it.

That distinction is part of the usefulness of the integration. The evidence should say exactly what it can support.

## The difficult part is the next sync

Getting text into a system once is only the first check. I also want to know what happens when nothing changes, when someone edits a watch title, and when a previously imported snapshot disappears.

changedetection.io allows history to be cleared, and its watch settings support retention limits. A connector should respect those changes. It also needs to distinguish a complete inventory from an interrupted request. A timeout must not be interpreted as evidence that every missing document was deleted.

My proposed approach is to reconcile removals only after a complete successful read of the configured scope. If a snapshot disappears while a sync is running, the connector should preserve the previous imported state and report the incomplete read rather than guess.

There is a separate boundary around configuration. A full watch record can contain request headers, browser steps and notification URLs. Those settings are not the documentation I want to search. The export should use a small allowlist of metadata fields instead of serializing the entire watch object.

## What I need to demonstrate

After assignment, I plan to run a local instance and record these checks:

- Import the selected pages and retrieve the expected text.
- Repeat the sync without creating duplicate documents.
- Capture an edit and retrieve both versions with the correct observation times.
- Remove retained history and a watch, then verify the corresponding imported records are removed.
- Interrupt an API read and verify that previously imported knowledge survives.
- Inspect exported documents and errors for credentials or private configuration.

I also need to check the documented installation from a clean environment. A working development directory is not enough evidence that another contributor can run the example.

## Where this stands

I opened [issue #6027](https://github.com/topoteretes/cognee/issues/6027) to ask whether this is a useful Mergetober integration and to request assignment. I am waiting before writing connector code, as required by the [event rules](https://www.wemakedevs.org/hackathons/mergetober/rules).

Once I have built and tested it, I will replace the planned example with the pages I actually used, the changes I made, the results I observed and the limitations I found.

---

## Evidence to add before this becomes the PR's blog post

This section is an editorial checklist, not article text. Complete it from the actual work; remove it only when the final post contains the evidence.

- Maintainer assignment and approved scope.
- PR URL, exact package version and tested changedetection.io release.
- Your own reason for using this workflow and the authorized pages you used.
- Exact installation and example commands that you ran successfully.
- Screenshots or redacted output showing initial import, unchanged sync and an edit.
- Actual retrieval output, including an unsatisfactory result if one occurred.
- Evidence for deletion propagation and preservation after a failed read.
- A limitation or design decision you can explain from the implementation.
- Your final wording and review of all technical claims.
- Public blog URL, then PR and blog links submitted through the event form when available.

References: [API](https://changedetection.io/docs/api_v1/index.html) · [Local installation](https://github.com/dgtlmoon/changedetection.io/blob/master/README.md#installation) · [Event overview](https://www.wemakedevs.org/hackathons/mergetober)
