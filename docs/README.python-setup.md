# 🐍 Python & AI Development Setup (macOS) — Reference Guide

This repository contains my personal notes and instructions for setting up Python, managing environments, and building a clean development workflow on macOS.  
It is designed for near‑term use as I continue learning **Python**, **AI**, **machine learning**, and related tooling.

The steps and recommendations here are based on **2025 – 2026** best practices.

---

# 📌 Why This Guide Exists

Python environments can get messy quickly:
- Different projects require different Python versions  
- Global Python can conflict with macOS system files  
- Managing packages can lead to dependency issues  

Modern tools like **uv** simplify all of this by combining Python version management, virtual environments, dependency locking, and fast package installation.  


This guide gives me a **simple, repeatable, future‑proof setup**.

---

# 🍺 1. Install Homebrew

Homebrew is the recommended package manager for macOS and is widely used for Python installations.  


Install Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Add Homebrew to your shell:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

***

# 🐍 2. Install Python Tools

## Option A — **uv** (Recommended)

uv is a modern Python toolchain written in Rust. It replaces:

*   pyenv
*   venv
*   pip
*   pipx
*   poetry

It provides fast environment creation and Python version installation.

Install uv:

```bash
brew install uv
```

## Option B — pyenv (Optional legacy tool)

If I ever need explicit compatibility with old tutorial workflows:

```bash
brew install pyenv
```

pyenv is still preferred by some teams for predictable Python version isolation.

***

# 📦 3. Install Python Versions

## Using uv (recommended)

List available versions:

```bash
uv python list
```

Install Python 3.12:

```bash
uv python install 3.12
```

Use it for the current project:

```bash
uv python use 3.12
```

(uv is now capable of replacing pyenv for version management.)

## Using pyenv (if required)

```bash
pyenv install 3.12.2
pyenv global 3.12.2
```

***

# 🧪 4. Create Virtual Environments

Each project gets its own environment — this avoids version and dependency conflicts. This seems to be a standard industry best practice.  

## Using uv (recommended)

```bash
cd myproject
uv venv
```

Or specify a version:

```bash
uv venv --python 3.12
```

uv will automatically download the requested Python version if it doesn’t exist.

Activate the environment:

```bash
source .venv/bin/activate
```

## Using built‑in venv (older method)

```bash
python3 -m venv .venv
source .venv/bin/activate
```

***

# 📚 5. Install Packages

## Using uv pip (fastest)

uv provides a Rust‑powered package installer that’s faster than pip.

```bash
uv pip install numpy pandas torch
```

## Using pip (legacy)

```bash
pip install numpy pandas torch
```

***

# 🔒 6. Lock Dependencies

Dependency locking improves reproducibility and consistency across machines.

```bash
uv lock
```

uv acts like Poetry for dependency management.

***

# ▶️ 7. Run Python Code

Run scripts with uv:

```bash
uv run script.py
```

This ensures correct Python versions and dependencies.

***

# 📁 Recommended Project Structure

    myproject/
      ├── .venv/
      ├── pyproject.toml      # if using uv workflow
      ├── requirements.txt    # only if needed
      ├── src/
      └── README.md

