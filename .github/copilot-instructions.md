<!-- .github/copilot-instructions.md: guidance for AI coding agents on the E-Bikes repo -->

# E-Bikes — AI agent instructions

Purpose: give a compact, actionable summary so an AI coding agent can be immediately productive in this Salesforce + LWC sample app.

- Big picture: This repository is a Salesforce Experience Cloud sample app built with Lightning Web Components (LWC) and Apex. Major surface areas live under `force-app/main/default`:
    - `lwc/` — Lightning Web Components and their metadata (.js/.html/.xml).
    - `classes/` — Apex controllers and test classes (look for `Test*` classes next to controllers).
    - `sites/`, `experiences/`, `networkBranding/` — Experience Cloud site and branding metadata.
    - `contentassets/`, `staticresources/` — front-end assets.

- Quick developer workflows (use these exact commands when automating or suggesting steps):
    - Install node deps: `npm install` (Node LTS required).
    - Create scratch org: `sf org create scratch -d -f config/project-scratch-def.json -a <alias>`
    - Deploy source to org: `sf project deploy start` or `sf project deploy start -d force-app` (use `--metadata-dir` for guest-profile metadata).
    - Assign permission set: `sf org assign permset -n ebikes` and `sf org assign permset -n Walkthroughs`.
    - Import sample data: `sf data tree import -p ./data/sample-data-plan.json`.
    - Publish Experience site: `sf community publish -n E-Bikes`.

- Tests & CI:
    - LWC unit tests: `npm test` (uses `sfdx-lwc-jest` wrapper — see `jest.config.js` and `test/jest-mocks`).
    - UI tests (UTAM + WebdriverIO): compile page objects `npm run test:ui:compile`, generate login `npm run test:ui:generate:login`, run `npm run test:ui`. These require an active org session (`sf org open`) and Chrome driver compatibility.
    - Linters/formatters: `npm run lint` and `npm run prettier` (Prettier Apex plugin requires Java 11+).
    - Pre-commit hooks: Husky + lint-staged configured in `package.json`.

- Project-specific patterns and conventions to preserve:
    - Always update component metadata `.xml` when adding/modifying LWC components under `force-app/main/default/lwc/<name>`.
    - Apex controllers live in `force-app/main/default/classes` alongside their test classes. Tests use standard Salesforce patterns; do not change test class names or remove `@isTest` annotations.
    - Experience site config is stored in `force-app/main/default/sites/E_Bikes.site-meta.xml` — automated/site guest settings often require editing this file (e.g., `<siteAdmin>` and `<siteGuestRecordDefaultOwner>` tags).
    - Guest site/permission metadata is packaged under `guest-profile-metadata/` and deployed separately with `--metadata-dir`.

- Integration and external dependencies:
    - Salesforce Platform (requires `sf` CLI and an org — scratch org or Dev/Playground).
    - Node.js (LTS) and npm packages found in `package.json` (LWC test tools, WebdriverIO, UTAM).
    - Optional demo: `ebikes-manufacturing` shows Pub Sub API, Platform Events, and Change Data Capture — see README for link.

- When editing code, follow these rules (explicit, repository-specific):
    - For UI changes: update LWC files in `lwc/` and their `.xml` metadata; run `npm test` for unit tests and `npm run test:ui` for UTAM flows when possible.
    - For backend changes: modify Apex in `classes/` and ensure associated test classes cover new behavior; Apex tests run in an org/CI (cannot execute locally).
    - Metadata-only changes (profiles, sites): prefer `sf project deploy start --metadata-dir=guest-profile-metadata -w 10` for guest-profile updates as used in README.

- Files to read first when understanding a pull request or making changes:
    - `README.md` — setup, deploy, and test commands (canonical).
    - `package.json`, `jest.config.js`, `wdio.conf.js`, `utam.config.js` — local test and CI configurations.
    - `force-app/main/default/classes/*` — Apex controllers & tests.
    - `force-app/main/default/lwc/*` — LWC components and their metadata.

- Helpful examples (copy exact snippets when suggesting commands):
    - Deploy: `sf project deploy start`
    - Import data: `sf data tree import -p ./data/sample-data-plan.json`
    - Run UI tests: `npm run test:ui`

If anything above is ambiguous or you need additional details (CI, exact Apex test command, or org-specific secrets), ask — I can run targeted searches or open files to fill gaps.

---

Please review this file for missing project-specific items or commands you rely on; I will iterate on your feedback.
