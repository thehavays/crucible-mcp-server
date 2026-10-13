# Continuous Integration (CI) Guidelines for Crucible MCP Server

---

## ⚙️ Environment & CI Context

* **GITHUB_USERNAME**: `thehavays`
* **REPOSITORY**: `crucible-mcp-server`
* **STACK**: Node.js (ESM), `@modelcontextprotocol/sdk`, `vitest`
* **CI_STRATEGY**: Mandatory local verification gates and automated GitHub Actions PR checks

---

## 🎯 CI Philosophy & Purpose

Continuous Integration guarantees that all code integrated into `dev` and `main`:
1. **Passes Static & Syntax Analysis**: Clean JavaScript execution without unhandled exceptions or module resolution issues.
2. **Maintains Schema Compliance**: All MCP tools declare valid input and output JSON schemas matching the MCP protocol.
3. **Passes Automated Tests**: Vitest test suites execute successfully across supported Node.js versions (20.x, 22.x).
4. **Ensures Zero Leaks**: Strict zero-secret policy via `.env.example` and `.gitignore`.

---

## 🚦 Mandatory Local Verification Gates

Before opening a Pull Request or pushing changes to remote, run these gates in order:

### 1. Test Suite Verification
```bash
npm run test
```
* **Acceptance Criteria**: Exit code 0, all vitest test files passing.

### 2. Startup & Dependency Check
```bash
node -c index.js
```
* **Acceptance Criteria**: Syntax check passes with exit code 0.

### 3. Diff Hygiene & Cleanliness
```bash
git status
git diff
```
* **Acceptance Criteria**:
  - No leftover debug statements (`console.log`, temporary dump files).
  - No untracked cache or secret files (`.env`, `.crucible-config.json`).
  - Working tree is clean and scoped.

---

## 🤖 Automated CI Strategy: GitHub Actions

Configured workflow: [`.github/workflows/ci.yml`](../.github/workflows/ci.yml)
- **Triggers**: On `push` and `pull_request` targeting `dev` and `main`.
- **Matrix**: Node.js versions `20.x` and `22.x` on `ubuntu-latest`.
- **Steps**: Checkout code, Setup Node.js with npm caching, `npm ci`, and `npm run test`.
