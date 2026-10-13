# Crucible MCP Server

An integration server built on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) that connects AI assistants to Atlassian Fisheye & Crucible. It provides tools to retrieve recent project reviews, project commits, review details, unified file diffs, and create draft review comments.

## Supported Crucible Versions

- **Atlassian FishEye & Crucible**: 4.x (4.5.x, 4.6.x, 4.7.x, 4.8.x, 4.9.x+)
- **Crucible Deployment Types**: Self-hosted Server and Data Center instances
- **REST APIs Used**: Crucible REST API v1 (`/rest-service/reviews-v1`) and FishEye RSS feeds

---

## Features

- **Get Recent Reviews**: Fetch the most recent code reviews from a specific Crucible project.
- **Get Recent Commits**: Fetch the most recent commits/changesets from a repository linked to Crucible.
- **Fetch Review Details**: Retrieve detailed information about a specific code review, including changed files (review items), authors, and comments.
- **Get File Diff (Patch)**: Dynamically fetch and compute the unified diff (patch) between two file versions inside a code review using `fromContentUrl` and `toContentUrl`.
- **Add Comments in Draft Mode**: Add review-level comments or file inline comments to reviews with strictly enforced draft mode (`draft: true`).
- **Strict Schema Compliance**: All tools declare input and output schemas conforming to the MCP specifications.

---

## Installation

1. Clone or copy the repository files.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the server:
   ```bash
   npm start
   ```

---

## Configuration

The server is configured via environment variables or a configuration file.

| Variable | Description | Example |
| :--- | :--- | :--- |
| `CRUCIBLE_URL` | Base URL of your Crucible instance | `https://crucible.example.com` |
| `CRUCIBLE_USERNAME` | Username (required for `basic` authentication) | `your_username` |
| `CRUCIBLE_PASSWORD` | Password for basic authentication | `your_password` |
| `CRUCIBLE_API_TOKEN` | API Token (if supported/configured instead of Basic auth) | `your_token` |

### Configuration File (`.crucible-config.json`)

You can provide configuration by creating a `.crucible-config.json` file in your home directory (`~/.crucible-config.json`) or directly inside the project directory:

```json
{
  "baseUrl": "https://crucible.example.com",
  "username": "your_username",
  "password": "your_password"
}
```

---

## Available MCP Tools

### `crucible_get_recent_reviews`
Lists recent code reviews from a project (e.g. `MYPROJECT`).
- **Inputs:** `project` (string, required), `limit` (number, default: 30)

### `crucible_get_recent_commits`
Lists recent commits/changesets from a project repository (e.g. `MYPROJECT`).
- **Inputs:** `project` (string, required), `limit` (number, default: 30)

### `crucible_get_review_details`
Get detailed information about a specific review, including files and patches/diffs.
- **Inputs:** `reviewId` (string, required, e.g., `CR-123`)

### `crucible_get_file_diff`
Get the unified diff (patch) for a specific file change using its `fromContentUrl` and `toContentUrl`.
- **Inputs:** `fromContentUrl` (string, required), `toContentUrl` (string, required)

### `crucible_add_comment`
Add a review comment or in-line file comment to a Crucible review. All comments are strictly created in draft mode (`draft: true`) so they can be reviewed and published manually.
- **Inputs:**
  - `reviewId` (string, required): ID of the review (e.g., `CR-123`).
  - `message` (string, required): Comment text content.
  - `reviewItemId` (string, optional): Review item ID (e.g., `CFR-123`) for file-level or inline comments. If omitted, adds a general review comment.
  - `toLineRange` (string, optional): Line number or range in the new file revision (e.g., `42` or `40-45`).
  - `fromLineRange` (string, optional): Line number or range in the old file revision (e.g., `40`).

---

## Development & Contribution
- Check `.agents/AGENTS.md` for AI agent guidelines and onboarding.
- Adhere to the `vitest` unit testing standards found in the `tests/` directory (`npm test`).
- Code should be documented using JSDoc.
