# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Flask-based web application that displays course information. Uses a simple MVC-style architecture: Flask routing in `app.py`, view logic in `views.py`, and in-memory data in `models.py`.

## Commands

```bash
# Setup (Unix/Mac)
python -m venv venv && source venv/bin/activate && pip install -r requirements.txt

# Run
python src/app.py  # http://127.0.0.1:5000

# Tests
python -m unittest discover -s tests
python -m unittest tests.test_app.AppTestCase.test_index  # single test
```

## Architecture

**Route registration**: Routes use `app.add_url_rule()` in `app.py` rather than `@app.route()` decorators, keeping view functions in `views.py` decoupled from Flask.

**Course lookup**: Course IDs in URLs are 1-indexed integers. `/course/1` maps to `courses[0]` in `models.py`. The view returns a 404 string response (not a rendered template) for out-of-range IDs.

**Contact form** (`/contact`): Handles GET and POST. POST validates name, email (regex), and address (min 10 chars) server-side, returning the form with field-level errors on failure or a success flag on success. Form data is only logged to stdout — no email is sent.

**Template inheritance**: All templates extend `layout.html`, which provides the header nav and footer. Nav hardcodes `/course/1` as the "Course Details" link.

**Test path setup**: `tests/test_app.py` uses `sys.path.insert` to add `src/` so that `from app import app` resolves correctly — tests must be run from the repo root.

## Development Workflow

After any change: add unit tests, run them to confirm they pass, then verify visually using the `ui-testing-agent` (or Playwright MCP at `http://127.0.0.1:5000`). Save screenshots to `test-output/` with descriptive names.

For UI/styling work, use the `/ui-designer` skill which has a project-specific design system at `.claude/skills/ui-designer/references/design-system.md`.
