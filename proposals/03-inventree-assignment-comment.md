# InvenTree assignment comment — posted

Target: https://github.com/topoteretes/cognee/issues/6036

Posted from ShubhiDixit09 on 19 October 2026: https://github.com/topoteretes/cognee/issues/6036#issuecomment-6268156539

Hi @NMZivkovic, I'd now like to request assignment for this InvenTree connector if the scope works for Mergetober.

I'd use token-authenticated REST reads for selected parts, engineering notes, parameters and direct BOM relationships. I'd check child records independently, since a note or BOM edit may not update the parent part's timestamp.

I'd validate it on a pinned local Docker release with a small synthetic assembly: unchanged sync, note-only edits, BOM changes and deletion. A failed later page must leave existing imports intact. The retrieval example would trace a component replacement back to its recorded note.

For transparency, my requests on #6027, #6035 and #6038 are also pending. No implementation has started; I'll wait for assignment before coding.
