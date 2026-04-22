# Lab 1: Setting Up the Working Environment

## 🎯 Objective

Set up all necessary tools and accounts to work with AI in QA throughout this course.

## ⏱️ Время: 45 minutes

## 📋 Prerequisites

- Computer with internet access
- Basic command line skills

---

## Шаг 1: Install Python 3.10+

### macOS
```bash
# Using Homebrew
brew install python@3.12

# Verify installation
python3 --version
```

### Windows
1. Download from https://www.python.org/downloads/
2. Run installer (check "Add Python to PATH")
3. Verify: `python --version`

### Linux
```bash
sudo apt update
sudo apt install python3.12 python3.12-venv python3-pip
python3 --version
```

---

## Шаг 2: Set Up Project Environment

```bash
# Clone the course repository
git clone https://github.com/AlexTsvetkov/qa-ai-learning.git
cd qa-ai-learning

# Create a virtual environment
python3 -m venv venv

# Activate it
# macOS/Linux:
source venv/bin/activate
# Windows:
# venv\Scripts\activate

# Install dependencies
pip install openai anthropic requests pytest
```

---

## Шаг 3: Create AI Service Accounts

### 3.1 OpenAI (ChatGPT)

1. Go to https://platform.openai.com/signup
2. Create an account
3. Navigate to API Keys: https://platform.openai.com/api-keys
4. Click "Create new secret key"
5. Save the key securely

### 3.2 Anthropic (Claude)

1. Go to https://console.anthropic.com
2. Create an account
3. Navigate to API Keys
4. Generate a new key
5. Save the key securely

### 3.3 (Optional) GitHub Copilot

1. Go to https://github.com/features/copilot
2. Start a free trial or subscribe
3. Install the Copilot extension in your IDE

---

## Шаг 4: Configure Environment Variables

Create a `.env` file in the project root (this file is already in `.gitignore`):

```bash
# .env
OPENAI_API_KEY=sk-your-openai-key-here
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key-here
```

> ⚠️ **NEVER** commit API keys to version control!

---

## Шаг 5: Verify the Setup

Create a file `test_setup.py`:

```python
import os

def test_python_version():
    import sys
    assert sys.version_info >= (3, 10), "Python 3.10+ is required"
    print(f"✅ Python version: {sys.version}")

def test_openai_installed():
    import openai
    print(f"✅ OpenAI library installed: {openai.__version__}")

def test_anthropic_installed():
    import anthropic
    print(f"✅ Anthropic library installed: {anthropic.__version__}")

def test_env_variables():
    # Load from .env if python-dotenv is available
    try:
        from dotenv import load_dotenv
        load_dotenv()
    except ImportError:
        pass

    openai_key = os.getenv("OPENAI_API_KEY")
    anthropic_key = os.getenv("ANTHROPIC_API_KEY")

    if openai_key:
        print(f"✅ OpenAI API key configured (starts with: {openai_key[:7]}...)")
    else:
        print("⚠️  OpenAI API key not set (optional for now)")

    if anthropic_key:
        print(f"✅ Anthropic API key configured (starts with: {anthropic_key[:7]}...)")
    else:
        print("⚠️  Anthropic API key not set (optional for now)")

if __name__ == "__main__":
    test_python_version()
    test_openai_installed()
    test_anthropic_installed()
    test_env_variables()
    print("\n🎉 Setup verification complete!")
```

Run the verification:
```bash
python test_setup.py
```

---

## Шаг 6: Install IDE Extensions (Optional)

### VS Code
- Python extension
- GitHub Copilot (if subscribed)
- Jupyter extension (for notebooks)

### PyCharm
- GitHub Copilot plugin (if subscribed)
- .env files support plugin

---

## ✅ Completion Checklist

- [ ] Python 3.10+ installed
- [ ] Virtual environment created and activated
- [ ] OpenAI library installed
- [ ] Anthropic library installed
- [ ] At least one AI API key configured
- [ ] Setup verification script runs successfully

## 🔗 Next

Continue to [Lab 2: First Queries to AI](lab-02-first-queries.md)


> 🌐 [English version](../module-01-foundations/labs/lab-01-setup.md)
