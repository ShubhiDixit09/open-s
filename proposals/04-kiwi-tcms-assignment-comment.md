# Kiwi TCMS assignment comment — posted

Target: https://github.com/topoteretes/cognee/issues/6037

Posted from ShubhiDixit09 on 19 October 2026: https://github.com/topoteretes/cognee/issues/6037#issuecomment-6269614174

Hi @NMZivkovic, could you assign this Kiwi TCMS connector to me if the scope is suitable for Mergetober?

I'd use the official tcms-api client and authenticated XML-RPC reads for selected plans and a bounded set of runs. I'd preserve case text, environment, results and execution comments as distinct evidence, and verify the API behavior on a pinned local release before relying on any pagination assumptions.

I'd test two release runs with a controlled failure and later pass, then edit case text and execution comments separately. RPC faults, permission failures or an incomplete extraction must not remove existing imports. The example would retrieve the recorded failure context with its source reference.

My other assignment requests are still pending. No implementation has started; I'll wait for assignment and scope feedback before coding.
