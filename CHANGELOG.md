## v0.1.0 (2026-10-19)

### Features

* add generic replacement updater
* **sync:** continuously reconcile plugin standards (#19)
* migrate template to subprocess plugin model
* create plugin template repository

### Bug Fixes

* **ci:** run semrel from the runner temp directory
* support legacy semrel release asset names (#22)
* handle concurrent template sync branches
* **sync:** avoid workflow-file pushes with least-privileged token (#21)
* remove commit_changelog:false with no replacement plugin — CHANGELOG.md was never updated
* **ci:** extract semrel release tarball into a dedicated subdir
* **dockerfile:** remove hardcoded repo-specific OCI labels
* **sync-template:** gate workflow file sync behind include_workflows input
* **sync-template:** set remote URL with token for git push + drop --label flag
* **sync-template:** fix YAML block scalar break from blank line in commit msg
* repair template workflows

### Other Changes

* test against stable Go 1.27 instead of the release candidate\n\nGo 1.27.0 has been generally available since August 2026 (now at\n1.27.1). Replace the temporary 1.27rc1 matrix entry with the stable\n1.27 line so CI resolves and tests against the latest 1.27.x patch\nrelease, matching the same treatment already applied in the main\nsemrel repository CI matrix.\n\nNo Go 1.28 pre-release exists yet to add (dev cycle started July\n2026, RC1 not expected before around January 2027).\n\nCo-authored-by: Copilot <223556219+Copilot@users.noreply.github.com> (#25)
* **deps:** Bump github.com/stretchr/testify from 1.11.1 to 1.12.1 (#23)
* fix(ci): extract semrel release tarball into a dedicated subdir
* fix(dockerfile): remove hardcoded repo-specific OCI labels
* feat: migrate template to subprocess plugin model
* build(deps): Bump actions/checkout from 6 to 7
* **deps:** Bump actions/checkout from 6 to 7
* add Go 1.27rc1 to test matrix (#13)
* add dynamic latest-release version badge placeholder to README template (#12)
* upgrade template to Go 1.25 + best-practice ci.yml + sync workflow
* remove gosimple linter (merged into staticcheck in golangci-lint v2)
* **deps:** Bump softprops/action-gh-release from 2 to 3 (#10)
* **deps:** Bump actions/download-artifact from 4 to 8 (#9)
* **deps:** Bump actions/dependency-review-action from 4 to 5 (#8)
* **deps:** Bump actions/checkout from 4 to 6 (#7)
* upgrade golangci-lint to v2.12.2 (supports Go 1.25)
* upgrade golangci-lint to v2, bump to Go 1.25
* **deps:** Bump actions/upload-artifact from 4 to 7 (#1)
* **deps:** Bump actions/setup-go from 5 to 6 (#3)
* **deps:** Bump github.com/stretchr/testify from 1.10.0 to 1.11.1 (#4)
* **deps:** Bump golangci/golangci-lint-action from 6 to 9 (#5)
* **deps:** Bump fsfe/reuse-action from 5 to 6 (#6)

