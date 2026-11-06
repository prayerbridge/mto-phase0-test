# MTO Phase 0 results: G1, G3, G4

Run by a Claude Code cloud scheduled task, finished 2026-11-06T18:15:25Z (UTC). Nothing was retried with a different method, force or bypass.

| Test | Action | Expected | Observed (exact) | Result |
|---|---|---|---|---|
| G1 read | add_repo prayerbridge/mto-phase0-test (push), git clone, read README.md | Attach + clone succeed | Attached; clone OK (HEAD 7a4c0d6 "Initial commit"). README.md content: `# mto-phase0-test` | PASS |
| G1 write | Push branch claude/phase0-g1 with phase0/g1-hello.md | Push succeeds | `* [new branch] claude/phase0-g1 -> claude/phase0-g1` (exit 0) | PASS |
| G3a | `git push origin HEAD:main` with phase0/should-not-exist.md | Refused | `GH013: Repository rule violations found for refs/heads/main. - Changes must be made through a pull request. ! [remote rejected] HEAD -> main (push declined due to repository rule violations)` | PASS |
| G3b | Push branch test-other | Refused by ruleset | `GH013: Repository rule violations found for refs/heads/test-other. - Cannot create ref due to creations being restricted. ! [remote rejected] test-other -> test-other (push declined due to repository rule violations)` | PASS |
| G3c | Open PR #1 claude/phase0-g1 -> main, merge via GitHub API (MCP merge_pull_request) | Merge refused, approval required | PR #1 opened. Merge: `PUT .../pulls/1/merge: 405 Repository rule violations found. At least 1 approving review is required by reviewers with write access.` PR left open. | PASS |
| G4 attach | add_repo prayerbridge/mto-scope-check (push) | (record) | Attach ALLOWED, but tool reported `push_check: refused` ("Claude doesn't have GitHub access to prayerbridge/mto-scope-check"). Read clone worked (repo is empty). | (info) |
| G4 push | Push branch claude/phase0-g4 with phase0/g4-scope.md | Refused | `remote: Claude doesn't have GitHub access to prayerbridge/mto-scope-check for your organization... fatal: unable to access 'https://github.com/prayerbridge/mto-scope-check/': The requested URL returned error: 403` | PASS |
| Net | curl https://www.sec.gov/ | (info) | `000`, curl exit 56: `CONNECT tunnel failed, response 403` | INFO: blocked |
| Net | curl https://www.federalreserve.gov/ | (info) | `000`, curl exit 56 | INFO: blocked |
| Net | curl https://paper-api.alpaca.markets/v2/clock | (info) | `000`, curl exit 56 | INFO: blocked |
| Net | curl https://example.com/ | (info) | `000`, curl exit 56: `CONNECT tunnel failed, response 403` | INFO: blocked |

**Summary:** G1 PASS, G3 PASS (3/3 refused), G4 PASS (push refused). Network: this cloud environment's proxy refused all four hosts, including SEC and the Fed, so the planned SEC/Fed allowlist is not in effect yet.
