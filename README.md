# cliagent


# 🤖 Gemini Architecture Agent

**Generate production-ready folder structures and boilerplate code** – using built‑in clean architecture templates or AI‑driven design via Google Gemini.

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Gemini API](https://img.shields.io/badge/Gemini-API-orange)](https://aistudio.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📦 Features

- **Built‑in templates** – exact folder structures from the provided architecture blueprints (React frontend, Node.js/Express backend, full‑stack).
- **AI‑powered customisation** – describe your project in plain English, and Gemini designs a clean, scalable structure.
- **Planner / Task Graph** – breaks down project descriptions into dependent tasks (DAG) for logical generation order.
- **Incremental regeneration** – only regenerates files that changed; `.blueprint.json` manifest tracks hashes.
- **Generator registry** – pluggable code generators for React components, Express routes, `package.json`, and more.
- **Plugin architecture** – easily add support for Next.js, FastAPI, Vue, etc. without touching the core.
- **Internal AST layer** – intermediate representation decouples AI output from file system logic.
- **Strong schema typing** – Pydantic v2 models validate every AI response.
- **Safe by design** – dry‑run mode, path sanitisation, retry/self‑healing prompts for Gemini.
- **Beautiful terminal UI** – progress bars, syntax‑highlighted JSON trees, and tables (powered by `rich`).

---

## 🚀 Installation

### 1. Clone or download the script

Save the final agent script as `gemini_agent.py`.

### 2. Install dependencies

```bash
pip install google-generativeai rich pydantic