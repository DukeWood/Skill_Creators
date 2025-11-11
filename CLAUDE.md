# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 🎯 Current Status (November 6, 2025)

**Version:** v2.0.0 (Production Ready - Major Feature Release)
**Active Development:** Flexible, incremental task-based approach

### Recent Updates (This Week):

**🚀 Major Release: Unified Multi-Source Scraping (v2.0.0)**
- **NEW**: Combine documentation + GitHub + PDF in one skill
- **NEW**: Automatic conflict detection between docs and code
- **NEW**: Rule-based and AI-powered merging
- **NEW**: Transparent conflict reporting with side-by-side comparison
- **NEW**: 5 example unified configs (React, Django, FastAPI, Godot, FastAPI-test)
- **NEW**: Complete documentation in docs/UNIFIED_SCRAPING.md
- **NEW**: Integration tests added (378/390 tests passing, 12 unified tests need fixes)
- **Status**: ⚠️ Core functionality stable, unified tests need attention

**✅ Community Response (H1 Group):**
- **Issue #8 Fixed** - Added BULLETPROOF_QUICKSTART.md and TROUBLESHOOTING.md for beginners
- **Issue #7 Fixed** - Fixed all 11 configs (Django, Laravel, Astro, Tailwind) - 100% working
- **Issue #4 Linked** - Connected to roadmap Tasks A2/A3 (knowledge sharing + website)
- **PR #5 Reviewed** - Approved anchor stripping feature (security verified, 32/32 tests pass)
- **MCP Setup Fixed** - Path expansion bug resolved in setup_mcp.sh

**📦 Configs Status:**
- ✅ **24 total configs available** (including unified configs)
- ✅ 5 unified configs added (React, Django, FastAPI, Godot, FastAPI-test)
- ✅ Core selectors tested and validated
- 📝 Single-source configs: ansible-core, astro, claude-code, django, fastapi, godot, godot-large-example, hono, kubernetes, laravel, react, steam-economy-complete, tailwind, vue
- 📝 Multi-source configs: django_unified, fastapi_unified, fastapi_unified_test, godot_unified, react_unified
- 📝 Test/Example configs: godot_github, react_github, python-tutorial-test, example_pdf, test-manual

**📋 Next Up:**
- **Priority**: Fix 12 failing unified tests in tests/test_unified.py
  - ConfigValidator expecting dict instead of file path
  - ConflictDetector expecting dict pages, not list
- Task H1.3 - Create example project folder
- Task A3.1 - GitHub Pages site (skillseekersweb.com)
- Task J1.1 - Install MCP package for testing

**📊 Roadmap Progress:**
- 134 tasks organized into 22 feature groups
- Project board: https://github.com/users/yusufkaraaslan/projects/2
- See [FLEXIBLE_ROADMAP.md](FLEXIBLE_ROADMAP.md) for complete task list

---

## 🔌 MCP Integration Available

**This repository includes a fully tested MCP server with 9 tools:**
- `mcp__skill-seeker__list_configs` - List all available preset configurations
- `mcp__skill-seeker__generate_config` - Generate a new config file for any docs site
- `mcp__skill-seeker__validate_config` - Validate a config file structure
- `mcp__skill-seeker__estimate_pages` - Estimate page count before scraping
- `mcp__skill-seeker__scrape_docs` - Scrape and build a skill
- `mcp__skill-seeker__package_skill` - Package skill into .zip file (with auto-upload)
- `mcp__skill-seeker__upload_skill` - Upload .zip to Claude (NEW)
- `mcp__skill-seeker__split_config` - Split large documentation configs
- `mcp__skill-seeker__generate_router` - Generate router/hub skills

**Setup:** See [docs/MCP_SETUP.md](docs/MCP_SETUP.md) or run `./setup_mcp.sh`

**Status:** ✅ Tested and working in production with Claude Code

## Overview

Skill Seeker automatically converts any documentation website into a Claude AI skill. It scrapes documentation, organizes content, extracts code patterns, and packages everything into an uploadable `.zip` file for Claude.

## Prerequisites

**Python Version:** Python 3.10 or higher (required for MCP integration)

**Setup with Virtual Environment (Recommended):**
```bash
# One-time setup
python3 -m venv venv
source venv/bin/activate  # macOS/Linux (Windows: venv\Scripts\activate)
pip install requests beautifulsoup4 pytest
pip freeze > requirements.txt

# Every time you use Skill Seeker in a new terminal session
source venv/bin/activate  # Activate before using any commands
```

**Why use a virtual environment?**
- Keeps dependencies isolated from system Python
- Prevents package version conflicts
- Standard Python development practice
- Required for running tests with pytest

**If someone else clones this repo:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Optional (for API-based enhancement):**
```bash
source venv/bin/activate
pip install anthropic
export ANTHROPIC_API_KEY=sk-ant-...
```

## Core Commands

### Quick Start - Use a Preset

```bash
# Single-source scraping (documentation only)
python3 cli/doc_scraper.py --config configs/godot.json
python3 cli/doc_scraper.py --config configs/react.json
python3 cli/doc_scraper.py --config configs/vue.json
python3 cli/doc_scraper.py --config configs/django.json
python3 cli/doc_scraper.py --config configs/laravel.json
python3 cli/doc_scraper.py --config configs/fastapi.json
```

### Unified Multi-Source Scraping (**NEW - v2.0.0**)

```bash
# Combine documentation + GitHub + PDF in one skill
python3 cli/unified_scraper.py --config configs/react_unified.json
python3 cli/unified_scraper.py --config configs/django_unified.json
python3 cli/unified_scraper.py --config configs/fastapi_unified.json
python3 cli/unified_scraper.py --config configs/godot_unified.json

# Override merge mode
python3 cli/unified_scraper.py --config configs/react_unified.json --merge-mode claude-enhanced

# Result: One comprehensive skill with conflict detection
```

**What makes it special:**
- ✅ Detects discrepancies between documentation and code
- ✅ Shows both versions side-by-side with ⚠️ warnings
- ✅ Identifies outdated docs and undocumented features
- ✅ Single source of truth showing intent (docs) AND reality (code)

**See full guide:** [docs/UNIFIED_SCRAPING.md](docs/UNIFIED_SCRAPING.md)

### First-Time User Workflow (Recommended)

```bash
# 1. Install dependencies (one-time)
pip3 install requests beautifulsoup4

# 2. Estimate page count BEFORE scraping (fast, no data download)
python3 cli/estimate_pages.py configs/godot.json
# Time: ~1-2 minutes, shows estimated total pages and recommended max_pages

# 3. Scrape with local enhancement (uses Claude Code Max, no API key)
python3 cli/doc_scraper.py --config configs/godot.json --enhance-local
# Time: 20-40 minutes scraping + 60 seconds enhancement

# 4. Package the skill
python3 cli/package_skill.py output/godot/

# Result: godot.zip ready to upload to Claude
```

### Interactive Mode

```bash
# Step-by-step configuration wizard
python3 cli/doc_scraper.py --interactive
```

### Quick Mode (Minimal Config)

```bash
# Create skill from any documentation URL
python3 cli/doc_scraper.py --name react --url https://react.dev/ --description "React framework for UIs"
```

### Skip Scraping (Use Cached Data)

```bash
# Fast rebuild using previously scraped data
python3 cli/doc_scraper.py --config configs/godot.json --skip-scrape
# Time: 1-3 minutes (instant rebuild)
```

### Async Mode (2-3x Faster Scraping)

```bash
# Enable async mode with 8 workers for best performance
python3 cli/doc_scraper.py --config configs/react.json --async --workers 8

# Quick mode with async
python3 cli/doc_scraper.py --name react --url https://react.dev/ --async --workers 8

# Dry run with async to test
python3 cli/doc_scraper.py --config configs/godot.json --async --workers 4 --dry-run
```

**Recommended Settings:**
- Small docs (~100-500 pages): `--async --workers 4`
- Medium docs (~500-2000 pages): `--async --workers 8`
- Large docs (2000+ pages): `--async --workers 8 --no-rate-limit`

**Performance:**
- Sync: ~18 pages/sec, 120 MB memory
- Async: ~55 pages/sec, 40 MB memory (3x faster!)

**See full guide:** [ASYNC_SUPPORT.md](ASYNC_SUPPORT.md)

### Enhancement Options

**LOCAL Enhancement (Recommended - No API Key Required):**
```bash
# During scraping
python3 cli/doc_scraper.py --config configs/react.json --enhance-local

# Standalone after scraping
python3 cli/enhance_skill_local.py output/react/
```

**API Enhancement (Alternative - Requires API Key):**
```bash
# During scraping
python3 cli/doc_scraper.py --config configs/react.json --enhance

# Standalone after scraping
python3 cli/enhance_skill.py output/react/
python3 cli/enhance_skill.py output/react/ --api-key sk-ant-...
```

### Package and Upload the Skill

```bash
# Package skill (opens folder, shows upload instructions)
python3 cli/package_skill.py output/godot/
# Result: output/godot.zip

# Package and auto-upload (requires ANTHROPIC_API_KEY)
export ANTHROPIC_API_KEY=sk-ant-...
python3 cli/package_skill.py output/godot/ --upload

# Upload existing .zip
python3 cli/upload_skill.py output/godot.zip

# Package without opening folder
python3 cli/package_skill.py output/godot/ --no-open
```

### Force Re-scrape

```bash
# Delete cached data and re-scrape from scratch
rm -rf output/godot_data/
python3 cli/doc_scraper.py --config configs/godot.json
```

### Estimate Page Count (Before Scraping)

```bash
# Quick estimation - discover up to 100 pages
python3 cli/estimate_pages.py configs/react.json --max-discovery 100
# Time: ~30-60 seconds

# Full estimation - discover up to 1000 pages (default)
python3 cli/estimate_pages.py configs/godot.json
# Time: ~1-2 minutes

# Deep estimation - discover up to 2000 pages
python3 cli/estimate_pages.py configs/vue.json --max-discovery 2000
# Time: ~3-5 minutes

# What it shows:
# - Estimated total pages
# - Recommended max_pages value
# - Estimated scraping time
# - Discovery rate (pages/sec)
```

**Why use estimation:**
- Validates config URL patterns before full scrape
- Helps set optimal `max_pages` value
- Estimates total scraping time
- Fast (only HEAD requests + minimal parsing)
- No data downloaded or stored

## Repository Architecture

### Modular Architecture

The codebase has evolved from a single-file design into a modular architecture:

**Core Scrapers** (Independent scraping modules):
- `doc_scraper.py` (1,789 lines) - HTML documentation scraping with async support
- `github_scraper.py` (797 lines) - GitHub repo scraping with AST analysis
- `pdf_scraper.py` (401 lines) - PDF extraction with OCR support
- `pdf_extractor_poc.py` (1,222 lines) - Advanced PDF features

**Unified Multi-Source** (v2.0.0 architecture):
- `unified_scraper.py` (449 lines) - Orchestrates multi-source scraping
- `config_validator.py` (376 lines) - Validates unified configs
- `conflict_detector.py` (513 lines) - Detects docs vs code conflicts
- `merge_sources.py` (513 lines) - Rule-based & AI-powered merging
- `unified_skill_builder.py` (444 lines) - Builds unified skills
- `code_analyzer.py` (491 lines) - AST parsing for 6+ languages

**Utilities & Tools**:
- `estimate_pages.py` (288 lines) - Page count estimation
- `enhance_skill.py` / `enhance_skill_local.py` (303 lines) - AI enhancement
- `package_skill.py` - Skill packaging
- `upload_skill.py` - Direct upload to Claude
- `split_config.py` (320 lines) - Split large configs
- `generate_router.py` (274 lines) - Router/hub skill generation

**llms.txt Support**:
- `llms_txt_detector.py` - Detects llms.txt files
- `llms_txt_downloader.py` - Downloads llms.txt variants
- `llms_txt_parser.py` - Parses llms.txt format

### File Structure

```
Skill_Seekers/
├── cli/                        # All CLI tools (modular)
│   ├── doc_scraper.py          # HTML documentation scraper
│   ├── github_scraper.py       # GitHub repository scraper
│   ├── pdf_scraper.py          # PDF extraction tool
│   ├── unified_scraper.py      # Multi-source orchestrator
│   ├── config_validator.py     # Config validation
│   ├── conflict_detector.py    # Conflict detection
│   ├── merge_sources.py        # Intelligent merging
│   ├── code_analyzer.py        # AST code analysis
│   ├── unified_skill_builder.py # Unified skill builder
│   ├── estimate_pages.py       # Page count estimator
│   ├── enhance_skill.py        # API-based enhancement
│   ├── enhance_skill_local.py  # Local enhancement
│   ├── package_skill.py        # Skill packager
│   ├── upload_skill.py         # Upload to Claude
│   ├── split_config.py         # Config splitting
│   ├── generate_router.py      # Router generation
│   ├── llms_txt_detector.py    # llms.txt detection
│   ├── llms_txt_downloader.py  # llms.txt downloading
│   ├── llms_txt_parser.py      # llms.txt parsing
│   ├── constants.py            # Shared constants
│   └── run_tests.py            # Test runner
├── skill_seeker_mcp/           # MCP server (9 tools)
│   ├── server.py               # Main MCP server
│   └── tools/                  # MCP tool implementations
├── configs/                    # 24 preset configurations
│   ├── Single-source (14): godot.json, react.json, vue.json, etc.
│   ├── Unified (5): react_unified.json, django_unified.json, etc.
│   └── Test configs (5): godot_github.json, example_pdf.json, etc.
├── tests/                      # Test suite (390 tests, 378 passing)
│   ├── test_unified.py         # 12 failing unified tests
│   ├── test_integration.py     # Integration tests
│   └── test_*.py               # Feature tests
├── docs/                       # Documentation
│   ├── UNIFIED_SCRAPING.md     # Multi-source guide
│   ├── MCP_SETUP.md            # MCP setup guide
│   ├── ENHANCEMENT.md          # Enhancement guide
│   └── UPLOAD_GUIDE.md         # Upload guide
└── output/                     # Generated output (git-ignored)
    ├── {name}_data/            # Scraped raw data (cached)
    │   ├── pages/*.json        # Individual page data
    │   └── summary.json        # Scraping summary
    └── {name}/                 # Built skill directory
        ├── SKILL.md            # Main skill file
        ├── SKILL.md.backup     # Backup (if enhanced)
        ├── references/         # Categorized documentation
        │   ├── index.md
        │   ├── getting_started.md
        │   ├── api.md
        │   └── ...
        ├── scripts/            # Empty (user scripts)
        └── assets/             # Empty (user assets)
```

### Data Flow

**Single-Source Workflow** (Documentation, GitHub, or PDF only):

1. **Scrape Phase** (`scrape_all()` in respective scraper):
   - Input: Config JSON (name, base_url, selectors, url_patterns, categories)
   - Process: BFS traversal (HTML), API calls (GitHub), or PDF extraction
   - Output: `output/{name}_data/pages/*.json` + `summary.json`

2. **Build Phase** (`build_skill()` in respective scraper):
   - Input: Scraped JSON data from `output/{name}_data/`
   - Process: Load pages → Smart categorize → Extract patterns → Generate references
   - Output: `output/{name}/SKILL.md` + `output/{name}/references/*.md`

3. **Enhancement Phase** (optional):
   - Input: Built skill directory with references
   - Process: Claude analyzes references and rewrites SKILL.md
   - Output: Enhanced SKILL.md with real examples and guidance

4. **Package Phase** (`package_skill.py`):
   - Input: Skill directory
   - Process: Zip all files (excluding .backup)
   - Output: `{name}.zip`

**Unified Multi-Source Workflow** (v2.0.0):

1. **Config Validation** (`config_validator.py`):
   - Validates unified config structure
   - Checks source types (docs, github, pdf)
   - Ensures merge mode is valid

2. **Multi-Source Scraping** (`unified_scraper.py`):
   - Scrapes each source independently
   - Stores in `output/{name}_data_{source_type}/`

3. **Conflict Detection** (`conflict_detector.py`):
   - Compares docs vs code implementations
   - Detects outdated docs, undocumented features
   - Generates conflict report JSON

4. **Intelligent Merging** (`merge_sources.py`):
   - Rule-based: Priority-based merging
   - Claude-enhanced: AI-powered resolution
   - Produces unified pages JSON

5. **Unified Skill Build** (`unified_skill_builder.py`):
   - Builds comprehensive SKILL.md
   - Includes conflict warnings (⚠️)
   - Side-by-side comparisons

6. **Package & Upload**:
   - Same as single-source workflow

### Configuration File Structure

Config files (`configs/*.json`) define scraping behavior:

```json
{
  "name": "godot",
  "description": "When to use this skill",
  "base_url": "https://docs.godotengine.org/en/stable/",
  "selectors": {
    "main_content": "div[role='main']",
    "title": "title",
    "code_blocks": "pre"
  },
  "url_patterns": {
    "include": [],
    "exclude": ["/search.html", "/_static/"]
  },
  "categories": {
    "getting_started": ["introduction", "getting_started"],
    "scripting": ["scripting", "gdscript"],
    "api": ["api", "reference", "class"]
  },
  "rate_limit": 0.5,
  "max_pages": 500
}
```

**Config Parameters:**
- `name`: Skill identifier (output directory name)
- `description`: When Claude should use this skill
- `base_url`: Starting URL for scraping
- `selectors.main_content`: CSS selector for main content (common: `article`, `main`, `div[role="main"]`)
- `selectors.title`: CSS selector for page title
- `selectors.code_blocks`: CSS selector for code samples
- `url_patterns.include`: Only scrape URLs containing these patterns
- `url_patterns.exclude`: Skip URLs containing these patterns
- `categories`: Keyword mapping for categorization
- `rate_limit`: Delay between requests (seconds)
- `max_pages`: Maximum pages to scrape

## Key Features & Implementation

### Auto-Detect Existing Data
Tool checks for `output/{name}_data/` and prompts to reuse, avoiding re-scraping (check_existing_data() in doc_scraper.py:653-660).

### Language Detection
Detects code languages from:
1. CSS class attributes (`language-*`, `lang-*`)
2. Heuristics (keywords like `def`, `const`, `func`, etc.)

See: `detect_language()` in doc_scraper.py:135-165

### Pattern Extraction
Looks for "Example:", "Pattern:", "Usage:" markers in content and extracts following code blocks (up to 5 per page).

See: `extract_patterns()` in doc_scraper.py:167-183

### Smart Categorization
- Scores pages against category keywords (3 points for URL match, 2 for title, 1 for content)
- Threshold of 2+ for categorization
- Auto-infers categories from URL segments if none provided
- Falls back to "other" category

See: `smart_categorize()` and `infer_categories()` in doc_scraper.py:282-351

### Enhanced SKILL.md Generation
Generated with:
- Real code examples from documentation (language-annotated)
- Quick reference patterns extracted from docs
- Common pattern section
- Category file listings

See: `create_enhanced_skill_md()` in doc_scraper.py:426-542

## Common Workflows

### First Time (With Scraping + Enhancement)

```bash
# 1. Scrape + Build + AI Enhancement (LOCAL, no API key)
python3 cli/doc_scraper.py --config configs/godot.json --enhance-local

# 2. Wait for enhancement terminal to close (~60 seconds)

# 3. Verify quality
cat output/godot/SKILL.md

# 4. Package
python3 cli/package_skill.py output/godot/

# Result: godot.zip ready for Claude
# Time: 20-40 minutes (scraping) + 60 seconds (enhancement)
```

### Using Cached Data (Fast Iteration)

```bash
# 1. Use existing data + Local Enhancement
python3 cli/doc_scraper.py --config configs/godot.json --skip-scrape
python3 cli/enhance_skill_local.py output/godot/

# 2. Package
python3 cli/package_skill.py output/godot/

# Time: 1-3 minutes (build) + 60 seconds (enhancement)
```

### Without Enhancement (Basic)

```bash
# 1. Scrape + Build (no enhancement)
python3 cli/doc_scraper.py --config configs/godot.json

# 2. Package
python3 cli/package_skill.py output/godot/

# Note: SKILL.md will be basic template - enhancement recommended
# Time: 20-40 minutes
```

### Creating a New Framework Config

**Option 1: Interactive**
```bash
python3 cli/doc_scraper.py --interactive
# Follow prompts, it creates the config for you
```

**Option 2: Copy and Modify**
```bash
# Copy a preset
cp configs/react.json configs/myframework.json

# Edit it
nano configs/myframework.json

# Test with limited pages first
# Set "max_pages": 20 in config

# Use it
python3 cli/doc_scraper.py --config configs/myframework.json
```

## Testing & Verification

### Finding the Right CSS Selectors

Before creating a config, test selectors with BeautifulSoup:

```python
from bs4 import BeautifulSoup
import requests

url = "https://docs.example.com/page"
soup = BeautifulSoup(requests.get(url).content, 'html.parser')

# Try different selectors
print(soup.select_one('article'))
print(soup.select_one('main'))
print(soup.select_one('div[role="main"]'))
print(soup.select_one('div.content'))

# Test code block selector
print(soup.select('pre code'))
print(soup.select('pre'))
```

### Verify Output Quality

After building, verify the skill quality:

```bash
# Check SKILL.md has real examples
cat output/godot/SKILL.md

# Check category structure
cat output/godot/references/index.md

# List all reference files
ls output/godot/references/

# Check specific category content
cat output/godot/references/getting_started.md

# Verify code samples have language detection
grep -A 3 "```" output/godot/references/*.md | head -20
```

### Test with Limited Pages

For faster testing, edit config to limit pages:

```json
{
  "max_pages": 20  // Test with just 20 pages
}
```

## Troubleshooting

### No Content Extracted
**Problem:** Pages scraped but content is empty

**Solution:** Check `main_content` selector in config. Try:
- `article`
- `main`
- `div[role="main"]`
- `div.content`

Use the BeautifulSoup testing approach above to find the right selector.

### Poor Categorization
**Problem:** Pages not categorized well

**Solution:** Edit `categories` section in config with better keywords specific to the documentation structure. Check URL patterns in scraped data:

```bash
# See what URLs were scraped
cat output/godot_data/summary.json | grep url | head -20
```

### Data Exists But Won't Use It
**Problem:** Tool won't reuse existing data

**Solution:** Force re-scrape:
```bash
rm -rf output/myframework_data/
python3 cli/doc_scraper.py --config configs/myframework.json
```

### Rate Limiting Issues
**Problem:** Getting rate limited or blocked by documentation server

**Solution:** Increase `rate_limit` value in config:
```json
{
  "rate_limit": 1.0  // Change from 0.5 to 1.0 seconds
}
```

### Package Path Error
**Problem:** doc_scraper.py shows wrong cli/package_skill.py path

**Expected output:**
```bash
python3 cli/package_skill.py output/godot/
```

**Not:**
```bash
python3 /mnt/skills/examples/skill-creator/scripts/cli/package_skill.py output/godot/
```

The correct command uses the local `cli/package_skill.py` in the repository root.

## Key Modules & Functions

### doc_scraper.py (HTML Documentation Scraping)
- `DocToSkillConverter` class - Main scraper with async support
- `scrape_all()` - BFS traversal of documentation
- `extract_content()` - HTML content extraction with selectors
- `detect_language()` - Code language detection (Python, JS, GDScript, etc.)
- `smart_categorize()` - AI categorization by keywords
- `build_skill()` - Generates SKILL.md and references/

### github_scraper.py (Repository Analysis)
- `GitHubScraper` class - GitHub API + code analysis
- `scrape_repo()` - Fetches README, file tree, issues, PRs
- `analyze_code_files()` - AST parsing for 6+ languages
- `extract_api_info()` - Extracts functions, classes, methods
- Supported languages: Python, JavaScript, TypeScript, Java, C++, Go

### unified_scraper.py (Multi-Source Orchestration)
- `UnifiedScraper` class - Orchestrates multi-source workflow
- `scrape_all_sources()` - Scrapes docs + GitHub + PDF
- `detect_conflicts()` - Calls conflict_detector.py
- `merge_sources()` - Intelligent merging with rule-based or AI

### conflict_detector.py (Documentation vs Code Analysis)
- `ConflictDetector` class - Finds discrepancies
- `detect_conflicts()` - Compares docs vs implementation
- `find_outdated_docs()` - Detects version mismatches
- `find_undocumented_features()` - Missing documentation

### code_analyzer.py (AST Code Analysis)
- `CodeAnalyzer` class - Multi-language AST parser
- `analyze_python()` - Python AST analysis
- `analyze_javascript()` - JS/TS analysis (esprima)
- `extract_functions()` / `extract_classes()` - API extraction

### config_validator.py (Config Validation)
- `ConfigValidator` class - Validates all config types
- `validate_unified_config()` - Unified config validation
- `validate_single_source()` - Single-source validation
- Validates URLs, selectors, source types, merge modes

## Common Development Patterns

### Adding Support for a New Documentation Site

1. **Test selectors** using BeautifulSoup (see Testing & Verification section)
2. **Create config** in `configs/yourframework.json`
3. **Test with limited pages** first (`"max_pages": 20`)
4. **Run estimation** with `python3 cli/estimate_pages.py configs/yourframework.json`
5. **Full scrape** with `python3 cli/doc_scraper.py --config configs/yourframework.json`

### Creating a Unified Multi-Source Skill

1. **Create individual configs** for each source (docs, GitHub, PDF)
2. **Create unified config** combining all sources (see `configs/*_unified.json`)
3. **Run unified scraper** with `python3 cli/unified_scraper.py --config configs/your_unified.json`
4. **Review conflicts** in the generated SKILL.md (look for ⚠️ warnings)
5. **Choose merge mode**: `rule-based` (fast) or `claude-enhanced` (better quality)

### Extending the Code Analyzer

To add support for a new programming language:

1. **Edit `code_analyzer.py`**
2. **Add language parser** (e.g., tree-sitter for C#, Ruby, etc.)
3. **Implement `analyze_<language>()` method**
4. **Extract API patterns** (functions, classes, methods)
5. **Add tests** in `tests/test_code_analyzer.py`

### Module Import Pattern

All CLI tools use this pattern for imports:

```python
# Add parent directory to path for imports
sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from cli.config_validator import ConfigValidator
from cli.conflict_detector import ConflictDetector
# etc.
```

This allows CLI tools to be run directly (e.g., `python3 cli/doc_scraper.py`) without installing as a package.

## Enhancement Details

### LOCAL Enhancement (Recommended)
- Uses your Claude Code Max plan (no API costs)
- Opens new terminal with Claude Code
- Analyzes reference files automatically
- Takes 30-60 seconds
- Quality: 9/10 (comparable to API version)
- Backs up original SKILL.md to SKILL.md.backup

### API Enhancement (Alternative)
- Uses Anthropic API (~$0.15-$0.30 per skill)
- Requires ANTHROPIC_API_KEY
- Same quality as LOCAL
- Faster (no terminal launch)
- Better for automation/CI

**What Enhancement Does:**
1. Reads reference documentation files
2. Analyzes content with Claude
3. Extracts 5-10 best code examples
4. Creates comprehensive quick reference
5. Adds domain-specific key concepts
6. Provides navigation guidance for different skill levels
7. Transforms 75-line templates into 500+ line comprehensive guides

## Performance

| Task | Time | Notes |
|------|------|-------|
| Scraping | 15-45 min | First time only |
| Building | 1-3 min | Fast! |
| Re-building | <1 min | With --skip-scrape |
| Enhancement (LOCAL) | 30-60 sec | Uses Claude Code Max |
| Enhancement (API) | 20-40 sec | Requires API key |
| Packaging | 5-10 sec | Final zip |

## Available Configs (24 Total)

### Single-Source Documentation Configs (14 configs)

**Web Frameworks:**
- ✅ `react.json` - React (article selector, 7,102 chars)
- ✅ `vue.json` - Vue.js (main selector, 1,029 chars)
- ✅ `astro.json` - Astro (article selector, 145 chars)
- ✅ `django.json` - Django (article selector, 6,468 chars)
- ✅ `laravel.json` - Laravel 9.x (#main-content selector, 16,131 chars)
- ✅ `fastapi.json` - FastAPI (article selector, 11,906 chars)
- ✅ `hono.json` - Hono web framework **NEW!**

**DevOps & Automation:**
- ✅ `ansible-core.json` - Ansible Core 2.19 (div[role='main'] selector, ~32K chars)
- ✅ `kubernetes.json` - Kubernetes (main selector, 2,100 chars)

**Game Engines:**
- ✅ `godot.json` - Godot (div[role='main'] selector, 1,688 chars)
- ✅ `godot-large-example.json` - Godot large docs example

**CSS & Utilities:**
- ✅ `tailwind.json` - Tailwind CSS (div.prose selector, 195 chars)

**Gaming:**
- ✅ `steam-economy-complete.json` - Steam Economy (div.documentation_bbcode, 588 chars)

**Development Tools:**
- ✅ `claude-code.json` - Claude Code documentation **NEW!**

### Unified Multi-Source Configs (5 configs - **NEW v2.0!**)
- ⚠️ `react_unified.json` - React (docs + GitHub + code analysis)
- ⚠️ `django_unified.json` - Django (docs + GitHub + code analysis)
- ⚠️ `fastapi_unified.json` - FastAPI (docs + GitHub + code analysis)
- ⚠️ `fastapi_unified_test.json` - FastAPI test config
- ⚠️ `godot_unified.json` - Godot (docs + GitHub + code analysis)

### Test/Example Configs (5 configs)
- 📝 `godot_github.json` - GitHub-only scraping example
- 📝 `react_github.json` - GitHub-only scraping example
- 📝 `python-tutorial-test.json` - Python tutorial test
- 📝 `example_pdf.json` - PDF extraction example
- 📝 `test-manual.json` - Manual testing config

**Note:** ⚠️ = Unified configs have 12 failing tests that need fixing
**Last verified:** November 6, 2025

## Additional Documentation

- **[README.md](README.md)** - Complete user documentation
- **[BULLETPROOF_QUICKSTART.md](BULLETPROOF_QUICKSTART.md)** - Complete beginner guide **NEW!**
- **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** - Comprehensive troubleshooting **NEW!**
- **[QUICKSTART.md](QUICKSTART.md)** - Get started in 3 steps
- **[docs/CLAUDE.md](docs/CLAUDE.md)** - Detailed technical architecture
- **[docs/ENHANCEMENT.md](docs/ENHANCEMENT.md)** - AI enhancement guide
- **[docs/UPLOAD_GUIDE.md](docs/UPLOAD_GUIDE.md)** - How to upload skills to Claude
- **[FLEXIBLE_ROADMAP.md](FLEXIBLE_ROADMAP.md)** - Complete task catalog (134 tasks)
- **[NEXT_TASKS.md](NEXT_TASKS.md)** - What to work on next
- **[TODO.md](TODO.md)** - Current focus
- **[STRUCTURE.md](STRUCTURE.md)** - Repository structure

## Testing

**Test Suite:** 390 tests (378 passing, 12 failing in unified tests)

### Running Tests

```bash
# Run all tests with colored output
python3 cli/run_tests.py

# Run specific test file
python3 -m pytest tests/test_unified.py -v

# Run with pytest directly
python3 -m pytest tests/ -v

# Run single test
python3 -m pytest tests/test_integration.py::TestIntegration::test_basic_scraping -v
```

### Test Organization

- `test_integration.py` - End-to-end workflow tests
- `test_unified.py` - Unified multi-source tests (12 failing - needs fixing)
- `test_config_validation.py` - Config validation tests
- `test_scraper_features.py` - Core scraping features
- `test_github_scraper.py` - GitHub scraping tests
- `test_pdf_scraper.py` - PDF extraction tests
- `test_async_scraping.py` - Async mode tests
- `test_mcp_server.py` - MCP integration tests

### Known Issues

**Priority**: Fix 12 failing tests in `tests/test_unified.py`
- ConfigValidator expecting dict instead of file path
- ConflictDetector expecting dict pages, not list

## Notes for Claude Code

- This is a Python-based documentation scraper with modular architecture
- Evolved from single-file (~790 lines) to modular design (9,874 lines across cli/)
- Core modules: doc_scraper, github_scraper, pdf_scraper, unified_scraper
- Unified multi-source scraping (v2.0.0) with conflict detection
- MCP server integration with 9 tools
- 390 tests (378 passing, focus on fixing 12 unified tests)
- Output is cached and reusable
- Enhancement is optional but highly recommended
- All scraped data stored in `output/` (git-ignored)
