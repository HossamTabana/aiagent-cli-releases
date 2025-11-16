# AI Agent CLI

AI-powered codebase analysis and documentation generation using Databricks AI.

**Developed by Hossam Tabana**

## Overview

AI Agent CLI is a powerful command-line tool that leverages Databricks AI to:

- **Analyze codebases** - Intelligent scanning and understanding of project structure
- **Generate documentation** - Create technical, architecture, and business documentation automatically
- **Extract knowledge** - Build comprehensive knowledge bases from source code
- **Smart analysis** - Context-aware analysis with anti-hallucination safeguards

## Installation

### Prerequisites

1. **Python 3.11+** (check with `python --version` or `python3 --version`)

2. **uv and uvx** (fastest Python package manager):

   **macOS/Linux:**
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

   **Windows:**
   ```powershell
   powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

   Restart your terminal after installation.

3. **Databricks Account** - You'll need:
   - Databricks workspace URL
   - Personal access token
   - Model serving endpoint

### Install Latest Version

**Option 1: Install from PyPI (Recommended)**

```bash
pip install aiagent-2025
```

**Option 2: Install from GitHub Releases**

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.2/aiagent_cli-0.1.2-py3-none-any.whl
```

The tool will be globally available as `aiagent` from any directory.

### Verify Installation

```bash
aiagent --version
```

You should see: `aiagent, version 0.1.2`

## Configuration

### Required: Databricks Credentials

Set these environment variables (or create a `.env` file):

```bash
export DATABRICKS_HOST="https://your-workspace.cloud.databricks.com"
export DATABRICKS_TOKEN="dapi..."
export DATABRICKS_MODEL="databricks-meta-llama-3-1-405b-instruct"
```

**macOS/Linux (.bashrc or .zshrc):**
```bash
echo 'export DATABRICKS_HOST="https://your-workspace.cloud.databricks.com"' >> ~/.bashrc
echo 'export DATABRICKS_TOKEN="dapi..."' >> ~/.bashrc
echo 'export DATABRICKS_MODEL="databricks-meta-llama-3-1-405b-instruct"' >> ~/.bashrc
source ~/.bashrc
```

**Windows (PowerShell):**
```powershell
[Environment]::SetEnvironmentVariable("DATABRICKS_HOST", "https://your-workspace.cloud.databricks.com", "User")
[Environment]::SetEnvironmentVariable("DATABRICKS_TOKEN", "dapi...", "User")
[Environment]::SetEnvironmentVariable("DATABRICKS_MODEL", "databricks-meta-llama-3-1-405b-instruct", "User")
```

## Quick Start

### 1. Create Project Brief (Optional but Recommended)

Provide context about your project to improve analysis accuracy:

```bash
cd /path/to/your/project
aiagent project-brief
```

Answer 3 quick questions about your project. This prevents AI hallucination.

### 2. Analyze Your Codebase

Generate a comprehensive knowledge document:

```bash
aiagent understand
```

This creates `.aiagent/knowledge.md` with:
- Project overview and architecture
- Technology stack analysis
- Component descriptions
- File inventory

**Options:**
- `--skip-cache` - Force fresh analysis (ignore cached results)
- `--disable-parallel` - Sequential processing (slower but uses less memory)
- `--model META_LLAMA_3_1_70B` - Use different Databricks model

### 3. Generate Documentation

Create professional documentation from your codebase:

```bash
# All documentation types
aiagent docs

# Individual types
aiagent docs --type technical      # Technical documentation with Mermaid diagrams
aiagent docs --type architecture   # Architecture-focused documentation
aiagent docs --type business       # Business-friendly documentation (no jargon)
```

**Generated files:**
- `.aiagent/technical-documentation.md`
- `.aiagent/architecture-documentation.md`
- `.aiagent/business-documentation.md`

**Options:**
- `--model META_LLAMA_3_1_405B` - Use larger model for better quality

## Command Reference

### `aiagent understand`
Analyze codebase and generate knowledge document.

**Options:**
- `--skip-cache` - Force fresh analysis
- `--disable-parallel` - Disable parallel processing
- `--model <name>` - Specify Databricks model

**Output:** `.aiagent/knowledge.md`

### `aiagent docs`
Generate documentation from codebase knowledge.

**Options:**
- `--type <type>` - Documentation type: `technical`, `architecture`, `business`, or `all` (default)
- `--model <name>` - Specify Databricks model

**Output:** `.aiagent/<type>-documentation.md`

### `aiagent project-brief`
Create or update project brief for better analysis.

**Interactive:** Asks 3 questions about your project.

**Output:** `.aiagent/project-brief.json`

### `aiagent --version`
Show version information.

### `aiagent --help`
Show help message with all commands.

## Available Models

The tool supports various Databricks models:

| Model | Best For | Token Limit |
|-------|----------|-------------|
| `META_LLAMA_3_1_405B` (default) | Comprehensive analysis | 128K |
| `META_LLAMA_3_1_70B` | Faster, cost-effective | 128K |
| `DBRX_INSTRUCT` | Databricks-optimized | 32K |

Specify with `--model` flag:
```bash
aiagent understand --model META_LLAMA_3_1_70B
```

## Features

### Intelligent Codebase Analysis
- **Smart file prioritization** - Focuses on entry points and critical files
- **Semantic chunking** - AST-based code analysis for better understanding
- **Parallel processing** - Fast analysis with concurrent file processing
- **Caching** - SQLite-based caching for incremental analysis

### Anti-Hallucination Safeguards
- **Grounding instructions** - Strict adherence to visible code
- **Project brief integration** - User-provided context validation
- **No assumption policy** - Only mentions technologies actually present in code

### Professional Documentation
- **Technical documentation** - With Mermaid diagrams, architecture details
- **Architecture documentation** - Design decisions, component interactions
- **Business documentation** - Jargon-free, stakeholder-friendly
- **Smart term mapping** - Converts technical terms to business language

### Smart Optimizations
- **Continuation handling** - Automatic handling of truncated AI responses
- **Response validation** - Detects incomplete outputs (unclosed code blocks, etc.)
- **Progress tracking** - Rich terminal UI with real-time progress

## Example Workflow

```bash
# Navigate to your project
cd /path/to/your/project

# Step 1: Provide project context (optional, first time only)
aiagent project-brief
# Answer: "A Python CLI tool for AI-powered code analysis"
# Answer: "Helps developers understand and document codebases automatically"
# Answer: "Software developers and technical writers"

# Step 2: Analyze codebase
aiagent understand
# Creates: .aiagent/knowledge.md

# Step 3: Generate all documentation
aiagent docs
# Creates:
#   .aiagent/technical-documentation.md
#   .aiagent/architecture-documentation.md
#   .aiagent/business-documentation.md

# Step 4: Review generated documentation
cat .aiagent/technical-documentation.md
```

## Output Structure

All generated files are stored in `.aiagent/` directory:

```
your-project/
├── .aiagent/
│   ├── knowledge.md                    # Codebase analysis
│   ├── technical-documentation.md      # Technical docs
│   ├── architecture-documentation.md   # Architecture docs
│   ├── business-documentation.md       # Business docs
│   ├── project-brief.json             # Project context
│   ├── business-mappings.json         # Term mappings
│   └── cache.db                        # Analysis cache (SQLite)
├── .gitignore                          # (Add .aiagent/ to this)
└── ... (your source code)
```

**Recommendation:** Add `.aiagent/` to your `.gitignore` file.

## Troubleshooting

### "Command not found: aiagent"

**Solution:** Ensure installation completed successfully.

```bash
# If installed via pip
pip install --upgrade aiagent-2025

# If installed via uvx, verify uvx is available
uvx --version

# Reinstall via uvx
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.2/aiagent_cli-0.1.2-py3-none-any.whl
```

### "Databricks authentication failed"

**Solution:** Verify environment variables are set correctly.

```bash
# Check variables
echo $DATABRICKS_HOST
echo $DATABRICKS_TOKEN
echo $DATABRICKS_MODEL

# Test authentication
aiagent --version  # Should work without auth
aiagent understand  # Will fail if auth is wrong
```

### "Rate limit exceeded"

**Solution:** Databricks API rate limits apply. Wait a few minutes or reduce parallel workers:

```bash
aiagent understand --disable-parallel
```

### "Out of memory during parallel processing"

**Solution:** Disable parallel processing for large codebases:

```bash
aiagent understand --disable-parallel
```

### "Analysis seems incomplete or inaccurate"

**Solution:**
1. Create a project brief to provide context:
   ```bash
   aiagent project-brief
   ```

2. Force fresh analysis (skip cache):
   ```bash
   aiagent understand --skip-cache
   ```

3. Try a larger model:
   ```bash
   aiagent docs --model META_LLAMA_3_1_405B
   ```

## Upgrading

To upgrade to a new version:

**If installed via pip:**
```bash
pip install --upgrade aiagent-2025
```

**If installed via uvx:**
```bash
# Install new version (replace version number with latest)
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.3/aiagent_cli-0.1.3-py3-none-any.whl
```

**Verify new version:**
```bash
aiagent --version
```

## Uninstallation

**If installed via pip:**
```bash
pip uninstall aiagent-2025
```

**If installed via uvx:**
```bash
uvx uninstall aiagent-cli
```

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.

## Support

For issues, questions, or feature requests, please contact:

**Hossam Tabana**
📧 [hossam.tabana@gmail.com](mailto:hossam.tabana@gmail.com)

## License

Proprietary - All rights reserved.

This software is distributed for authorized use only. Redistribution, modification, or reverse engineering is prohibited without explicit permission.

---

**Developed by Hossam Tabana**
Powered by Databricks AI
