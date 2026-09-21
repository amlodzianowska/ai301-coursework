## Checks

| Check                | Evidence                                                | Pass condition                                                                                                                                                                        | Weight    |
| :------------------- | :------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------- |
| `commits-alive`      | `repo-facts` block (last 5 default-branch commit dates) | The most recent human commit to the default branch occurred within the last 90 days.                                                                                                  | required  |
| `responds-to-issues` | Issue comment thread                                    | Maintainers have not ignored questions or PRs from contributors. If there are no comments from the community, or if the issue was recently opened by a maintainer, this check passes. | required  |
| `unclaimed`          | Issue comment thread and linked PRs                     | The issue has 0 open linked PRs, and no community member has stated they are working on it in the comments.                                                                           | required  |
| `newcomer-scope`     | Issue body and labels                                   | The issue has a beginner-friendly label (e.g., "good first issue", "help wanted") OR the issue body clearly outlines reproduction steps or specific files to edit.                    | preferred |

## Verdict Rule

To determine the final status of the issue:

- **Accept:** The issue is accepted only if all `required` checks evaluate to `pass`.
- **Reject:** The issue is rejected if any `required` check evaluates to `fail` or `unclear`.

The `preferred` check (`newcomer-scope`) is used to rank accepted issues but does not trigger a rejection if it fails or is unclear.
