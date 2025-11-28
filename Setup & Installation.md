<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 3rem 2rem; border-radius: 8px; margin-bottom: 2rem;">
  <img src="https://media.licdn.com/dms/image/v2/D560BAQEX2svTIH2VZw/company-logo_200_200/company-logo_200_200/0/1737660676208/thepowerofalignment_logo?e=2147483647&v=beta&t=nT_uaJEnBfqvq2SpxMioHCL0WkcqVRwyZQggj1hhV10" alt="Linean Logo" style="height: 80px; margin-bottom: 1.5rem;">
  <h1 style="color: white; margin: 0; font-size: 2.5rem; font-weight: 700;">Claude Code Setup</h1>
  <p style="color: rgba(255,255,255,0.9); margin: 0.5rem 0 0 0; font-size: 1.2rem;">Get up and running with Claude Code at Linean</p>
</div>
---
## Table of Contents:
* [Home](./Linean%20AI%20Development%20Platform%20Proposal.md)
* [Installation](#installation) 
* [Authentication](#authentication) 
* [Project Configuration](#project-configuration) 
* [Optional: Semantic Search](#optional-but-recommended-semantic-search-with-claude-context) 
* [Verify Setup](#verify-setup)
## Installation

**System Requirements:**
- macOS 10.15+, Ubuntu 20.04+, or Windows 10+ (WSL)
- 4GB+ RAM
- Node.js 18+ (for NPM installation only)

**Install Claude Code:**
```bash
# macOS/Linux (recommended)
curl -fsSL https://claude.ai/install.sh | bash

# Or via Homebrew
brew install --cask claude-code

# Or via NPM if you prefer
npm install -g @anthropic-ai/claude-code
```

For detailed installation instructions (Windows, troubleshooting, etc.), see [official setup docs](https://code.claude.com/docs/en/setup).

After installation:
```bash
cd your-project
claude
``````

---
## Authentication

At Linean, we recommend using the **Personal Max plan** ($100/mo) for Claude Code.

When you first run `claude`, choose:
- **Option: Claude App (with Max plan)**
- Log in with your Claude account

---

## Project Configuration

Every project at Linean should have a **CLAUDE.MD** file in the repository root. This will be committed and shared, and gives Claude instructions to operate. Sometimes, you will have to refer Claude to the file. [Claude's recommended structure ⬇️ ](https://www.claude.com/blog/using-claude-md-files)

⚠️ **Notice**: Claude loads this file into context with every prompt, so keep it concise to save on tokens!

```markdown
# [Project Name]
Project x is ... your role is ...

## File Structure
app/
├── models/      # SQLAlchemy models
├── schemas/     # Pydantic schemas
├── api/         # Route handlers
└── services/    # Business logic
tests/           # Pytest suite

## Commands
poetry run uvicorn app.main:app --reload              # Dev server
poetry run pytest                                     # Run tests
poetry run black . && poetry run mypy .               # Format & type check
poetry run alembic revision --autogenerate -m "msg"   # Migration
poetry run alembic upgrade head                       # Apply migration

## Standards
- Type hints required on all functions
- Database models use singular names 
.....

## Workflows
### Adding a Database Model
1. Create model in `app/models/` and schema in `app/schemas/`
2. Generate migration: `alembic revision --autogenerate`
.....

## Avoid
- Bypassing authentication
- Raw SQL queries
...
```

Customize this template for each project. The more context you give Claude, the better it performs (and the less you have to type!).

---

## Optional (but recommended!): Semantic Search with claude-context

For large codebases, you can add semantic search capabilities via the [claude-context MCP](https://github.com/zilliztech/claude-context).

**What it does:**
- Searches your entire codebase semantically (not just grep - which is a keyword search)
- Finds relevant code across millions of lines
- Reduces token usage by only loading relevant files into context

**Setup:**
1. Get a free vector database from [Zilliz Cloud](https://cloud.zilliz.com/)
2. Get an OpenAI API key for embeddings
3. Add the MCP:
```bash
claude mcp add claude-context \
  -e OPENAI_API_KEY=sk-your-openai-api-key \
  -e MILVUS_TOKEN=your-zilliz-cloud-api-key \
  -- npx @zilliz/claude-context-mcp@latest
```

4. Index your codebase:
```bash
cd your-project
claude
# Then in Claude: "Index this codebase"
```

5. Use it:
```bash
# "Find functions that handle user authentication"
# "Show me where we validate grant budgets"
```

This is optional but highly recommended for projects like GMS with complex codebases. It does require an API Key (for embedding), however, you can run an embedding model locally as well using Ollama. 

---
## Verify Setup

Run this to check everything is configured correctly:

```bash
claude doctor # or claude mcp list
```

---
<div style="background: #f8f9fa; padding: 2rem; border-radius: 8px; text-align: center; margin-top: 3rem;">
  <img src="https://media.licdn.com/dms/image/v2/D560BAQEX2svTIH2VZw/company-logo_200_200/company-logo_200_200/0/1737660676208/thepowerofalignment_logo?e=2147483647&v=beta&t=nT_uaJEnBfqvq2SpxMioHCL0WkcqVRwyZQggj1hhV10" alt="Linean Logo" style="height: 60px; margin-bottom: 1rem;">
  <p style="color: #666; margin: 0;">Prepared by Noah Gallego for Linean</p>
  <p style="color: #999; margin: 0.5rem 0 0 0; font-size: 0.9rem;">November 2025</p>
</div>