CHANGELOG
###############

v0.8.4 (2026-10-08)
===================


- build: October 2026 dependency, security and tooling updates (#43)
- Same pass as esg-netsuite, esg-currentrms and gcp (#77).
- - Security: urllib3 2.7.0 -> 2.8.0 (Dependabot alerts on uv.lock and
  requirements.txt).
- uv lock --upgrade, with the ruff cap raised to 0.16.x to match the
  ruff-pre-commit hook; regenerate requirements.txt.
- commitizen version_provider pep621 -> uv, so cz bump also updates the
  version in uv.lock (CI's uv-lock hook failed lint after every
  release). commitizen.md updated; checked with `cz bump --files-only`.
- prek: the 13 pre-commit-hooks prek implements natively move to
  `repo = "builtin"`; check-ast, debug-statements and name-tests-test
  stay on pre-commit-hooks. autoupdate: ruff-pre-commit v0.16.10,
  codespell v2.4.3, uv-pre-commit 0.12.23, commitizen v4.19.2.
- Actions: setup-python v7, wait-on-check-action v1.9.1, setup-uv
  v10.2.0 and prek-action v3.0.1 (exact tags; immutable releases).
- Supersedes Dependabot #29, #33, #37-#41.
- Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>
- docs: bring dev docs up to date with uv, prek and commitizen (#42)
- * docs: bring dev docs up to date with uv, prek and commitizen
- Same docs layout as esg-netsuite, esg-currentrms and gcp:
- - development/commitizen.md (new): release flow, bump mapping, version
  source, and the current uv.lock phantom diff after each release.
- development/windows venv.md (new): uv setup, running src/app.py with
  its environment variables, VS Code. Takes over docs/devops/uv.md's
  content (including `uv lock --upgrade-package`).
- devops/requirements uv.md (new): `uv export` for the Docker image.
- Move pre-commit.md to development/ (prek); archive poetry.md under
  development/z_archive/. Drop docs/devops/uv.md, whose second copy of
  the Poetry notes duplicated poetry.md.
- Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
- * build: sync uv.lock with the 0.8.3 release
- The bump to 0.8.3 updated pyproject.toml but not uv.lock (commitizen
pep621 provider), so CI's uv-lock hook re-locks and fails lint on every
PR. The October updates switch commitizen to the uv provider.
- Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
- ---------
- Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>

v0.8.3 (2026-06-28)
===================


- build: migrate from pre-commit to prek (#32)
- Replace .pre-commit-config.yaml with prek.toml. All hooks are
equivalent; no-commit-to-branch replaces the custom bash local hook,
and pre-commit is removed from dev dependencies. Aligned hook set with
standard by adding check-executables-have-shebangs, mixed-line-ending,
and name-tests-test.
- Since ruff check is clean, lint.yaml is replaced with j178/prek-action
and ruff.yaml is removed entirely.
- Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>

v0.8.2 (2026-06-25)
===================


- build: bump ruff to 0.15.x, requests to >=2.33.0, actions/checkout to v7 (#28)
- Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>

v0.8.1 (2026-06-25)
===================


- Merge pull request #26 from dxclabs-io/build/migrate-poetry-to-uv
- build: migrate from poetry to uv
- ci: fix python-version format for setup-uv
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
- build: migrate from poetry to uv
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
- build: add deptry, CI improvements, commitizen version_provider (#25)
- * build: add deptry, CI improvements, commitizen version_provider
- - Add deptry >=0.25.1,<0.26.0 to linters group with package_module_name_map
- Add deptry pre-commit hook (francescorubbo/deptry-pre-commit v0.25.1)
- Add commitizen pre-commit hook (was missing)
- Add deptry step to lint.yaml
- Unchained lint and tests workflows from ruff (now run independently on PR)
- Remove Gr1N/setup-poetry from bump.yaml; use pip install commitizen directly
- commitizen: add version_scheme=semver2, version_provider=pep621
- actions/checkout v6 → v7 across all workflows
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
- * build: regenerate requirements files after poetry lock
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
- * build: bump pytest >=9.0.3 to cover dependabot alert
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
- * chore: remove RELEASE.md
- * ci: add explicit permissions to deploy.yaml
- ---------
- Co-authored-by: Claude Sonnet 4.6 <noreply@anthropic.com>
- Merge pull request #23 from dxclabs-io/ci-developer-connect
- ci: fix bump/deploy workflows and commitizen config for Developer Connect
- ci: fix bump/deploy workflows and commitizen config for Developer Connect
- bump.yaml: add bump: guard, workflow_dispatch, fix exit code trap to
handle both 3 and 21, push commit and tags separately.
- deploy.yaml: replace deploy_branch with deploy_tag triggered on v* tag
push; remove push-to-master trigger; preserve CONSUL_ROOT/DEPLOY_PATH
logic in deploy_tag.
- pyproject.toml: switch commitizen to cz_customize; move bump_pattern
and bump_map to customize section; add schema_pattern and bump_message.
- Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>

v0.8.0 (2026-06-11)
===================


- bump: version 0.6.0 → 0.7.0
- Package updates (#22)
- * ci: Pre-commit autoupdate. Updates to ruff and poetry
- * test: add pytest-cov to pyproject.toml tests group
- * build: update poetry requirements and requirements files

v0.7.0 (2026-06-11)
===================

Feat
----

- per-job-type termination reporting rules (#6)
- key event accumulator by allocation ID (#5)

v0.6.0 (2026-03-17)
===================

Feat
----

- key event accumulator by allocation ID instead of job ID

v0.5.0 (2026-03-16)
===================

Feat
----

- key TERMINAL_EVENT_TYPES by job type
- show deployment health on Started events in Slack
- treat deployment_healthy=False as a terminal state
- capture job_type, deployment health and task states per job
- replace polling with Nomad event stream, group events by job (#3)

Fix
---

- don't treat Started as terminal while health check is pending

v0.4.0 (2026-03-13)
===================

Feat
----

- save Consul index on clean shutdown
- per-job event accumulation via python-nomad event stream
- replace polling with Nomad event stream, group events by job
- src layout, pyupgrade, Block Kit formatting (#2)

Fix
---

- debounce Terminated and treat Started as terminal
- suppress duplicate reports after terminal event
- handle urllib3-wrapped ReadTimeout in stream window
- handle stale/unreadable Consul index value gracefully

0.3.0 - 2026-03-12
==================

chore: move app.py => src/app.py
chore: fix linting, typing and other ruff errors in app.py
refactor: change slack post to Block Kit messages
ci: tighten ruff tests in workflow
chore: ruff reformat app.py
chore: adjust .gitignore and .gitattributes
