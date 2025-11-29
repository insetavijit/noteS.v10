> [!quote] महाभारतम् (Epic Literature, India, ~400 BCE)
> **“विद्या विवादाय धनं मदाय शक्तिः परेषां परिपीडनाय।”**
> *“Knowledge used for arguments, wealth used for pride, and power used to crush others are all misused.”*

## 3. QUICK REFERENCE (PYTHON – XX Sheets)

High-speed access to essential knowledge — ready for real-world use without deep-dive reading.  
Copy, paste, lookup, fix, ship — instantly.

Python Roadmap#3.1 Cheatsheets – condensed one-page summaries | Python Roadmap#3.2 Snippets – copy-paste ready solutions | Python Roadmap#3.3 Templates – reusable structures for faster work | Python Roadmap#3.4 Condensed Architecture Diagrams | Python Roadmap#3.5 Error & Issue Playbook | Python Roadmap#3.6 Best Practices – recommended techniques  
Back to Top

### 3.1 Cheatsheets – condensed one-page summaries
Fast-lookup documents summarizing commands, syntax, patterns, workflow sequences.

- Python Syntax & Language Essentials
- Data Structures & Methods (list, dict, tuple, set)
- File I/O + OS + pathlib operations
- OOP Quick Syntax (class, dataclass, inheritance, @property)
- Async / Await core patterns + asyncio cheat sheet
- Virtual Environments & Packaging (venv, pip, pyproject.toml, poetry)
- FastAPI/Flask routing & dependency injection cheatsheet
- SQLAlchemy ORM mapping + query sheet

### 3.2 Snippets – copy-paste ready solutions
Pre-written small blocks solving high-frequency repetitive needs.

- Read/write file + context manager + JSON/CSV load & save
- Dictionary/list transformations (comprehensions, merge, defaultdict)
- requests session + timeout + error handling + JSON response
- FastAPI endpoint with Pydantic validation + response model
- SQLAlchemy model + session + CRUD operations sample
- Structured logging configuration (JSON + rotating files)
- .env loading with python-dotenv + type casting

### 3.3 Templates – reusable structures for faster work

#### 3.3.1 Prompt Templates – structured instruction patterns
Reusable AI/tool prompts for consistent results.

- Debug this code → step-by-step reasoning prompt
- Refactor & improve readability → detailed request scaffold
- Write docstrings + type hints → documentation prompt
- Code review checklist prompt

#### 3.3.2 Code Templates – common reusable outlines
Blueprints you drop into any project.

- Python CLI with argparse + click template
- FastAPI router → service → repository layered template
- SQLAlchemy Base model + repository pattern
- Celery / RQ task worker + result backend template
- Settings + logging + dependency config module

#### 3.3.3 Boilerplates – full starter projects
Ready-to-git clone scaffolding.

- Minimal Python package skeleton (src layout + pyproject.toml)
- FastAPI + SQLAlchemy + Alembic + pytest boilerplate
- CLI automation toolkit (typer + rich)
- Dockerized FastAPI service + multi-stage Dockerfile

### 3.4 Condensed Architecture Diagrams – minimal visual summaries
Mermaid-ready mini-maps (copy-paste into Obsidian).

- Request → Router → Service → Repository → DB flow
- Async task lifecycle (producer → queue → worker → callback)
- JWT authentication & role-based authorization flow
- CI/CD pipeline (GitHub Actions → build → test → deploy)

### 3.5 Error & Issue Playbook – common problems and fixes

#### 3.5.1 Common Errors
- ModuleNotFoundError / ImportError
- Circular import hell
- TypeError: missing positional argument / unsupported operand
- AttributeError / KeyError
- JSONDecodeError, SQLite connection issues, Alembic migration conflicts

#### 3.5.2 Typical Causes
- Wrong cwd / venv not activated
- Bad project layout or import style
- Mismatched function signatures / defaults
- Version conflicts (poetry/pip)
- Silent exception swallowing

#### 3.5.3 One-line Fixes & Commands
- python -m venv venv && source venv/bin/activate && pip install -r requirements.txt
- Use absolute imports or add __init__.py
- pip install -U package or poetry lock --no-update
- Add logging + pdb.set_trace() or debugger
- poetry add package@latest or pin exact versions

### 3.6 Best Practices – recommended techniques

#### 3.6.1 Do’s & Don’ts
- Do: PEP-8, type hints, tests, small functions, explicit is better than implicit
- Don’t: hardcode secrets, mute exceptions, god objects, mutable default args

#### 3.6.2 Performance Guidelines
- Prefer comprehensions & map/filter over loops
- Use asyncio or multiprocessing for I/O vs CPU
- Cache with @cache / @lru_cache
- Always profile before premature optimization

#### 3.6.3 Security Considerations
- .env + python-decouple / pydantic Settings
- Validate ALL external input (Pydantic, marshmallow)
- Rate limiting + timeout on public endpoints
- Keep dependencies updated (pip-audit, safety, dependabot)
- Enforce authz on every service/repository method

