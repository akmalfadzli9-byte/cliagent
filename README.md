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

Below is a complete README.md for the Gemini-powered CLI scaffolding agent we developed. It covers installation, usage, features, architecture, and contribution guidelines.

---

```markdown
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
```

3. Get a Gemini API key

· Go to Google AI Studio
· Sign in and create an API key
· Set it as an environment variable:

```bash
export GEMINI_API_KEY="your-key-here"
```

4. (Optional) Make it a global command

```bash
chmod +x gemini_agent.py
sudo ln -s "$(pwd)/gemini_agent.py" /usr/local/bin/gemini-agent
```

Now you can run gemini-agent from anywhere.

---

🎮 Usage

Interactive mode

```bash
python gemini_agent.py
```

You will be guided through:

· Selecting built‑in template or describing a custom project
· (If describing) – AI plans tasks, generates a blueprint, then writes files
· Dry‑run confirmation before actual file creation

Command‑line shortcuts

Command Effect
python gemini_agent.py --template frontend Immediately scaffold the React frontend template
python gemini_agent.py --template fullstack --dry-run Preview full‑stack structure without writing files
python gemini_agent.py --describe "a microblog with React, FastAPI, and MongoDB" Let Gemini design a custom project

Example walkthrough

```
$ python gemini_agent.py

╔══════════════════════════════════════════════════╗
║      🤖 GEMINI-POWERED ARCHITECTURE AGENT        ║
║   Generate clean folder structures via AI chat   ║
╚══════════════════════════════════════════════════╝

Describe your project:
> A task management app with React frontend, Express API, and SQLite

[Planner] Planned 4 tasks
┌────────┬─────────────────────────┬────────────────┐
│ ID     │ Name                    │ Depends on     │
├────────┼─────────────────────────┼────────────────┤
│ task1  │ Setup React frontend    │ -              │
│ task2  │ Create Express API      │ -              │
│ task3  │ Add authentication      │ task1, task2   │
│ task4  │ Implement task CRUD     │ task2, task3   │
└────────┴─────────────────────────┴────────────────┘

Proceed? y

[Generating blueprint...]
✅ Project 'task-app' created at /home/user/task-app

File Actions:
CREATE   frontend/src/App.tsx
CREATE   backend/src/routes/tasks.js
...
```

---

🧠 Architecture Overview

```
User input (description)
   │
   ▼
[ Planner ] ──► Task Graph (DAG)
   │
   ▼
[ Gemini JSON pipeline ] ──► ProjectBlueprint (Pydantic model)
   │
   ▼
[ AST Converter ] ──► ASTFolder / ASTFile
   │
   ▼
[ Generator Registry ] ──► Code generators (React, Express, etc.)
   │
   ▼
[ Incremental Generator ] ──► Files + .blueprint.json manifest
```

Key components:

· Separate pipelines – gemini_json_pipeline() for structured blueprints, gemini_text_pipeline() for free‑text Q&A.
· Pydantic schemas – ProjectBlueprint, TaskGraph, Manifest enforce strict typing.
· Planner – uses Gemini (text) to decompose project descriptions into dependent tasks.
· AST layer – decouples AI output from filesystem, enabling dry‑runs and diffs.
· Generator registry – built‑in + plugin generators; each generator knows its language, extensions, and templates.
· Manifest – stores file hashes; incremental generation only updates changed files.

---

🔌 Extending with Plugins

Create a new plugin by subclassing Plugin and implementing the required methods:

```python
from gemini_agent_pro import Plugin, GeneratorRegistry, CodeGenerator, GeneratorMetadata

class FastAPIPlugin(Plugin):
    name = "fastapi"

    def get_blueprint_prompt_extension(self) -> str:
        return "- Use FastAPI with routers, Pydantic schemas, and dependency injection."

    def post_process_blueprint(self, blueprint):
        blueprint.dependencies.append(Dependency(name="fastapi", version="^0.100.0"))
        return blueprint

    def register_generators(self, registry: GeneratorRegistry):
        registry.register(FastAPIRouterGenerator())
```

Then load the plugin by name (the system will discover it if placed in plugins/ folder or via entry points).

---

📄 Built‑in Templates (from reference images)

The tool includes the exact folder structures from the provided architecture diagrams:

· Frontend only – React + Vite with src/assets, components, layout, pages, features, hooks, context, redux, services, utils.
· Backend only – Node.js/Express with src/config, controllers, models, routes, middleware, services, utils.
· Full‑stack – combination of the above plus database/ folder and root package.json.

These templates serve as the fallback when AI generation fails, ensuring you always get a clean, maintainable structure.

---

🛡️ Safety & Reliability

· Dry‑run flag (--dry-run) – preview all file operations without writing.
· Path sanitisation – prevents directory traversal attacks (e.g., ../../etc/passwd).
· AI retry with self‑healing – if Gemini returns invalid JSON, it receives a correction prompt and retries once.
· Manifest backups – previous .blueprint.json is preserved before regeneration.

---

🧪 Development & Testing

Run the test suite (if you have one):

```bash
pytest tests/
```

To contribute:

1. Fork the repository.
2. Create a feature branch.
3. Run black and ruff for formatting/linting.
4. Submit a pull request.

---

❓ FAQ

Do I need a Gemini API key for built‑in templates?
No – built‑in templates work offline without any API key. The key is only required for AI‑driven customisation.

Can I use a different LLM (Claude, GPT‑4)?
Currently only Gemini is supported, but the architecture is LLM‑agnostic; you can replace the API client with a different one.

What if Gemini returns a huge blueprint?
The JSON pipeline truncates output for preview, but the full blueprint is saved in the manifest.

How do I update an existing project with a new description?
Run the agent again on the same project directory – it will read the manifest and regenerate only changed files.

---

📜 License

MIT – use freely in personal and commercial projects.

---

🙏 Acknowledgements

· Built with Google Gemini API
· Powered by Pydantic and Rich
· Inspired by clean architecture principles and the reference folder structures

---

Happy scaffolding! 🚀

```

---

This README is ready to be saved as `README.md` in your project root. It explains everything a new user needs, from installation to advanced extension. Let me know if you want to add badges, more detailed examples, or a troubleshooting section.
```


