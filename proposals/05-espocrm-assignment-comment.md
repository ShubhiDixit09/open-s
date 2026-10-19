Posted from ShubhiDixit09 on 19 October 2026: https://github.com/topoteretes/cognee/issues/6038#issuecomment-6268015996

Hi @NMZivkovic, could you assign this EspoCRM connector to me if the proposed scope is suitable for Mergetober?

I'd keep the first version limited to published, unexpired knowledge articles in selected categories, using a dedicated API user with read permissions. I'd check eligibility on every complete sync so an article can be removed when it expires, even if its body hasn't changed.

I'd test against a pinned local Docker instance with synthetic articles: edits, expiry, category moves, deletion, lost permissions and a failed later page. Failed reads must preserve existing imports. The example would show retrieval before and after an article expires.

I also have requests pending on #6027 and #6035. No implementation has started; I'll wait for assignment and any scope feedback before coding.
