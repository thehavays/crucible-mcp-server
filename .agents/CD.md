# Continuous Deployment (CD) Guidelines for Crucible MCP Server

---

## ⚙️ Environment & CD Context

* **HOSTING_PLATFORM**: Local CLI / MCP Client Stdio Execution
* **GITHUB_USERNAME**: `thehavays`
* **REPOSITORY**: `crucible-mcp-server`
* **DEPLOYMENT_TYPE**: Package distribution (npm / npx) & local repository execution
* **RUNTIME**: Node.js 20+

---

## 🌐 Delivery & Runtime Architecture

`crucible-mcp-server` operates as a Model Context Protocol (MCP) server communicating over Standard Input/Output (`stdio`). 
It does not run as a long-lived web service or cloud container; instead, it is invoked on-demand by MCP hosts (such as Claude Desktop, Cursor, Gemini Code Assist, or Antigravity).

### Distribution Methods:
1. **Direct GitHub Execution via `npx`**:
   ```json
   {
     "mcpServers": {
       "crucible": {
         "command": "npx",
         "args": ["-y", "github:thehavays/crucible-mcp-server"],
         "env": {
           "CRUCIBLE_URL": "https://crucible.example.com",
           "CRUCIBLE_USERNAME": "your_username",
           "CRUCIBLE_PASSWORD": "your_password"
         }
       }
     }
   }
   ```
2. **Local Repository Execution**:
   ```json
   {
     "mcpServers": {
       "crucible": {
         "command": "node",
         "args": ["/path/to/crucible-mcp-server/index.js"],
         "env": {
           "CRUCIBLE_URL": "https://crucible.example.com",
           "CRUCIBLE_USERNAME": "your_username",
           "CRUCIBLE_PASSWORD": "your_password"
         }
       }
     }
   }
   ```

---

## 🌿 Environment & Branch Mappings

| Branch | Stage | Purpose |
| :--- | :--- | :--- |
| **`main`** | **Production Release** | Stable release code; consumers installing via `github:thehavays/crucible-mcp-server` pull from `main`. |
| **`dev`** | **Integration / Staging** | Active development and integration branch for verified feature PRs. |
| **`feat/*`** | **Feature Branches** | Work in progress branches targeting `dev`. |

---

## 🚀 Production Release Workflow

1. **Verify Integration**: Ensure all features and tests pass on `dev`.
2. **Open Release PR**: Ask user confirmation, then create Release PR from `dev` to `main` via GitHub MCP `create_pull_request`.
3. **Merge Release PR**: Ask user confirmation, then merge via GitHub MCP `merge_pull_request` (auto-closes resolved issues).
4. **Tag Release**: Create semantic version tag (e.g., `git tag -a v1.1.0 -m "Release v1.1.0"`).
