# Changelog

All notable changes to AI Agent CLI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.3] - 2025-11-17

### Changed
- **Documentation Updates** - Updated all documentation files with latest information
  - Improved installation instructions
  - Enhanced troubleshooting guide
  - Updated README with clearer examples
  - Refined changelog formatting

### Installation

Install directly from PyPI:

```bash
pip install aiagent-2025
```

Or install via uvx from GitHub releases:

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.3/aiagent_2025-0.1.3-py3-none-any.whl
```

---

## [0.1.2] - 2025-11-16

### Added
- **PyPI Package Distribution** - AI Agent CLI is now available on PyPI at [https://pypi.org/project/aiagent-2025/](https://pypi.org/project/aiagent-2025/)
  - Simplified installation via `pip install aiagent-2025`
  - Easier package management and updates
  - Standard Python packaging for wider accessibility

### Installation

Install directly from PyPI:

```bash
pip install aiagent-2025
```

Or install via uvx from GitHub releases:

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.2/aiagent_cli-0.1.2-py3-none-any.whl
```

---

## [0.1.0] - 2025-11-16

### Initial Release

First public release of AI Agent CLI - AI-powered codebase analysis and documentation generation tool.

### Added

#### Core Commands
- `aiagent understand` - Analyze codebase and generate comprehensive knowledge document
- `aiagent docs` - Generate technical, architecture, and business documentation
- `aiagent project-brief` - Interactive project context gathering
- `aiagent --version` - Version information
- `aiagent --help` - Command help and usage

#### Intelligent Analysis Features
- **Smart file prioritization** - Focuses on entry points, configs, and critical source files
- **Semantic chunking** - AST-based code parsing for Python files
- **Parallel processing** - Concurrent file analysis with ThreadPoolExecutor (4 workers)
- **SQLite caching** - Incremental analysis with file hash-based cache invalidation
- **Project brief integration** - User-provided context to prevent AI hallucination

#### Documentation Generation
- **Technical documentation** - Comprehensive technical docs with Mermaid diagrams
  - System overview with architecture diagrams
  - Component descriptions with interaction flows
  - Technology stack analysis
  - Development setup and testing strategy
- **Architecture documentation** - Focused on design and architectural decisions
  - Architecture patterns and principles
  - Component responsibilities and boundaries
  - Integration points and dependencies
  - Design rationale and trade-offs
- **Business documentation** - Jargon-free documentation for stakeholders
  - Business capabilities and processes
  - Business value and workflows
  - Intelligent term clarification (max 7 questions)
  - Business-friendly term mapping

#### Anti-Hallucination Safeguards
- Strict grounding instructions in all AI prompts
- Project brief validation against actual code
- "Not visible in codebase" policy for uncertain details
- No assumption of orchestration tools or infrastructure not present in code

#### AI Response Handling
- **Continuation support** - Automatic handling of truncated responses
- **Response validation** - Detects incomplete Mermaid diagrams, code blocks, and sentences
- **Multi-turn conversations** - Context-aware follow-up for synthesis

#### User Experience
- Rich terminal UI with progress bars and spinners
- Color-coded output with status indicators
- Intelligent question flow (max 7 clarifications for business docs)
- Cache statistics and performance metrics
- Informative error messages

#### Databricks AI Integration
- Support for multiple Databricks models:
  - `META_LLAMA_3_1_405B` (default) - 128K token limit
  - `META_LLAMA_3_1_70B` - 128K token limit
  - `DBRX_INSTRUCT` - 32K token limit
- Configurable temperature and token limits
- Environment-based authentication (DATABRICKS_HOST, DATABRICKS_TOKEN)

#### File Management
- Organized `.aiagent/` directory structure
- Automatic directory creation
- Structured output files:
  - `knowledge.md` - Codebase analysis
  - `technical-documentation.md` - Technical docs
  - `architecture-documentation.md` - Architecture docs
  - `business-documentation.md` - Business docs
  - `project-brief.json` - Project context
  - `business-mappings.json` - Term mappings
  - `cache.db` - SQLite cache

#### Configuration Options
- `--skip-cache` - Force fresh analysis
- `--disable-parallel` - Sequential processing mode
- `--model <name>` - Custom Databricks model selection
- `--type <type>` - Documentation type selection (technical/architecture/business/all)

#### Smart Scanning
- Automatic `.gitignore` compliance
- Exclusion of binary files, media, and build artifacts
- Default exclusions: `node_modules/`, `venv/`, `.git/`, `__pycache__/`, `dist/`, `build/`
- Configurable file size limits (default: 500 lines per file for analysis)
- File tree generation for project structure visualization

### Technical Details

#### Dependencies
- `click` >= 8.1.0 - CLI framework
- `rich` >= 13.0.0 - Terminal formatting
- `requests` >= 2.31.0 - HTTP client for Databricks API
- `python-dotenv` >= 1.0.0 - Environment variable management

#### Requirements
- Python 3.11 or higher
- Databricks workspace with Foundation Model API access
- uv/uvx for installation and execution

#### Architecture
- Modular design with separate concerns:
  - `cli.py` - Command-line interface
  - `analyzer.py` - Codebase analysis engine
  - `generator.py` - Documentation generation
  - `databricks.py` - Databricks AI client
  - `scanner.py` - File scanning and prioritization
  - `cache.py` - SQLite caching layer
  - `chunking.py` - Semantic code chunking
  - `project_brief.py` - Project context management

### Installation

Install via uvx from GitHub releases:

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.0/aiagent_cli-0.1.0-py3-none-any.whl
```

### Known Limitations

- **Language support**: Semantic chunking currently optimized for Python (fallback to simple truncation for other languages)
- **Large codebases**: Very large projects (>1000 files) may require `--disable-parallel` to avoid memory issues
- **Rate limits**: Subject to Databricks API rate limits (retry logic not yet implemented)
- **Binary files**: No support for analyzing compiled or binary artifacts
- **Cache invalidation**: Cache is file hash-based; renaming files doesn't invalidate cache

### Performance Characteristics

- **Small projects (<50 files)**: ~30-60 seconds for full analysis
- **Medium projects (50-200 files)**: ~2-5 minutes for full analysis
- **Large projects (200+ files)**: ~5-15 minutes (with prioritization analyzing top 50-100 files)
- **Cached re-analysis**: ~10-30 seconds (only analyzes changed files)

### Future Roadmap (Not in v0.1.0)

Items planned for future releases:

- Multi-language semantic chunking (JavaScript, Java, Go, etc.)
- Custom exclusion patterns via config file
- GitHub Actions workflow for automated documentation updates
- Support for additional AI providers (OpenAI, Anthropic, Azure OpenAI)
- Interactive code exploration mode
- Diff-based analysis for change documentation
- Export to PDF, HTML, and other formats
- Plugin system for custom analyzers
- Team collaboration features (shared briefs, templates)

---

## Release Assets

### v0.1.3

**PyPI Package:** [aiagent-2025](https://pypi.org/project/aiagent-2025/)

**Installation:**
```bash
pip install aiagent-2025
```

**Wheel file:** `aiagent_2025-0.1.3-py3-none-any.whl`

**GitHub Installation:**
```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.3/aiagent_2025-0.1.3-py3-none-any.whl
```

---

### v0.1.2

**PyPI Package:** [aiagent-2025](https://pypi.org/project/aiagent-2025/)

**Installation:**
```bash
pip install aiagent-2025
```

**Wheel file:** `aiagent_cli-0.1.2-py3-none-any.whl`

**GitHub Installation:**
```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.2/aiagent_cli-0.1.2-py3-none-any.whl
```

---

### v0.1.0

**Wheel file:** `aiagent_cli-0.1.0-py3-none-any.whl`

**SHA256 checksum:** (Generated during build - verify with `shasum -a 256 aiagent_cli-0.1.0-py3-none-any.whl`)

**Installation:**
```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/v0.1.0/aiagent_cli-0.1.0-py3-none-any.whl
```

---

**Developed by Hossam Tabana**
Powered by Databricks AI
