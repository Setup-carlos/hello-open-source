# Branch Protection

This repository uses a branch ruleset to protect the default branch (`main`).

## Rules

- Direct pushes to `main` are blocked
- All changes must go through a pull request
- Force pushes and branch deletions are blocked
- Commits must be signed with GPG
- The `check` status check must pass before merging

## How to apply

This ruleset must be created manually in the GitHub UI:

1. Go to Settings → Rules → Rulesets
2. Click "New branch ruleset"
3. Name it `protect-main`
4. Import or copy the contents of `.github/ruleset-protect-main.json`
5. Set target branches to include the default branch
6. Save

For single-maintainer projects, `required_approving_review_count` is set to `0`.
Increase this value when multiple contributors are active.
