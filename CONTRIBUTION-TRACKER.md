# Mergetober contribution tracker

Last checked: 19 October 2026 (Asia/Kolkata).

GitHub account: [ShubhiDixit09](https://github.com/ShubhiDixit09). The GitHub API verified this author for all five issues below.

## Published proposals

| Integration | Issue | Current workflow | Blog |
| --- | --- | --- | --- |
| changedetection.io watched-page history | [#6027](https://github.com/topoteretes/cognee/issues/6027) | [Approach and assignment comment posted](https://github.com/topoteretes/cognee/issues/6027#issuecomment-6265201081); awaiting acceptance and assignment | [Draft](blogs/01-changedetection-6027-draft.md) |
| Readeck | [#6035](https://github.com/topoteretes/cognee/issues/6035) | [Assignment requested](https://github.com/topoteretes/cognee/issues/6035#issuecomment-6268005185); awaiting acceptance and assignment | [Draft](blogs/02-readeck-draft.md) |
| InvenTree | [#6036](https://github.com/topoteretes/cognee/issues/6036) | [Assignment requested](https://github.com/topoteretes/cognee/issues/6036#issuecomment-6268156539); awaiting acceptance and assignment | [Draft](blogs/03-inventree-draft.md) |
| Kiwi TCMS | [#6037](https://github.com/topoteretes/cognee/issues/6037) | [Assignment requested](https://github.com/topoteretes/cognee/issues/6037#issuecomment-6269614174); awaiting acceptance and assignment | [Draft](blogs/04-kiwi-tcms-draft.md) |
| EspoCRM | [#6038](https://github.com/topoteretes/cognee/issues/6038) | [Assignment requested](https://github.com/topoteretes/cognee/issues/6038#issuecomment-6268015996); awaiting acceptance and assignment | [Draft](blogs/05-espocrm-draft.md) |

All five issues are currently open, unassigned and labelled enhancement. Maintainer acceptance and event labelling remain pending. No PR exists yet.

An issue is a proposal and discussion. A pull request is the later code submission. The event's “Your pull requests” panel is expected to remain empty until a PR exists.

## Working sequence

Daily monitoring is scheduled for **9:00 AM IST**, starting 20 October 2026, under app automation `check-mergetober-issue-updates`. It checks all five issues and alerts only for substantive replies, assignment, decisions or other actionable changes. It does not send assignment requests. Baseline: [issue monitor](research/issue-monitor-baseline.json).

1. Wait for actual maintainer assignment before integration code or prototypes. Assignment requests are pending on all five issues. Do not send repeat requests automatically.
2. Confirm the approved scope and run Cognee locally.
3. Build a complete useful connector and verify real ingestion/retrieval, unchanged sync, changes, deletions and failed-read recovery.
4. Check clean installation, README/example commands, repository contribution rules, signed-off commits and DCO affirmation.
5. Revise the integration's separate blog using Shubhi's actual experience, versions and evidence. Do not invent usage or results.
6. Open the eligible PR in October, record PR/public-blog URLs, and submit both through the event form when available.
7. Address feedback without pressuring maintainers. Request another assignment only when ready to begin that work.

## Limits

- New integration issues opened: **5 of 5** (#6027, #6035, #6036, #6037, #6038). No new issue slots remain.
- Open PRs created in this chat: **0**; the simultaneous maximum is five.
- Assignment requests: **five**, one per proposal. All were explicitly authorized by Shubhi and posted from ShubhiDixit09 on 19 October.
- Each request tags @NMZivkovic and explains the approach and validation plan. Do not duplicate these requests while awaiting a response. Original scope-proposal issue bodies are historical; the later comments record the change in intent. The [proposal index](proposals/README.md) links the saved comments.
- No integration code, prototypes or live integration tests have been completed.
- Blog drafts: **five working drafts**, saved here for review; no finished real-use event blog has been published.

## Files

- [Five published proposals and saved texts](proposals/README.md)
- [All blog drafts](blogs/README.md)
- [First blog draft](blogs/01-changedetection-6027-draft.md)
- [Research and duplicate-check evidence](research/backup-shortlist-2026-10-19.md)
- [Official integration guide checklist](research/integration-guide-checklist.md)

## Sources

- [Event rules](https://www.wemakedevs.org/hackathons/mergetober/rules)
- [Overview and FAQ](https://www.wemakedevs.org/hackathons/mergetober)
- [Contribution resources](https://www.wemakedevs.org/hackathons/mergetober/resources)
- [Community contribution guide](https://github.com/topoteretes/cognee-community/blob/main/CONTRIBUTING.md)
- [Building a cognee Integration](https://app.notion.com/p/topoteretes/Building-a-cognee-Integration-3c937007fa8281b18e83fcc24f86004a)
