## Phase 5 — Mark Story Complete

When the current story is fully implemented and validated, you MUST signal completion so the orchestrator can update `prd.json` safely.

Replace STORY_ID below with the actual story ID from the "## Current Story" section above (e.g. QP-002).

1. Tell the orchestrator you are done by creating a marker file:
   `touch .story-updates/STORY_ID.passed`
2. Verify the marker:
   `ls .story-updates/STORY_ID.passed`
3. Commit your work:
   `git add . && git commit -m "chore: mark STORY_ID as passed"`
4. Push commit to remote origin if connected:
   `git push origin HEAD 2>/dev/null || true`

## Test Strategy Gate

Before writing implementation code for the current story, classify it and write the
required tests FIRST (TDD). Do not mark `passes: true` until every required test
exists, runs green, and covers the story's acceptance criteria.

### Step 1 — Classify the story

| Story touches | Required tests |
|---------------|----------------|
| Service, utility, middleware, or business logic (no UI) | **Unit tests** for every public function and error branch |
| HTTP endpoint or repository + DB | **Unit tests** for service layer + **integration test** for the endpoint |
| UI route, page, form, modal, or user-visible flow | **Unit tests** for logic/hooks + **Playwright E2E spec** for the user flow |
| Full-stack feature (API + UI) | **Unit** + **integration** + **Playwright E2E** |
| Pure config, scaffold, or env-only (no runtime logic) | Sample passing unit test only; E2E not required |

Playwright is REQUIRED when the story adds or changes anything a user sees or
interacts with (pages, forms, buttons, navigation, toasts, modals). Browser-only
manual checks are NOT sufficient.

### Step 2 — Write tests before implementation

1. Read the project's existing test layout (co-located `*.test.ts`, `e2e/tests/`, page objects).
2. Write failing unit tests that map 1:1 to acceptance criteria for logic/API stories.
3. For UI stories, add a Playwright spec under `e2e/tests/` using the page-object pattern
   (`e2e/pages/*.page.ts`, `e2e/fixtures/`). Cover: happy path, primary error/validation path,
   and navigation/redirect if applicable.
4. Implement code until all new tests pass.

### Step 3 — Verify before completion

- Run the project's unit test command (`npm test`, `pytest`, etc.) — all pass.
- If Playwright applies: run `npm run test:e2e` (or project equivalent) — all pass.
- Run typecheck and lint if the project defines them.
- Every acceptance criterion must be exercised by at least one automated test.

If the project has no E2E setup yet and this story is UI-facing, create the minimal
Playwright scaffold (`playwright.config.ts`, `e2e/tests/`, `test:e2e` script) as part
of this story before marking it complete.
