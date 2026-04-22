# Module 5: Theory — Building AI Testing Pipelines

## 5.1 Architecture of AI-Enhanced Testing Pipelines

### Traditional vs AI-Enhanced Pipeline

**Traditional Pipeline:**
```
Code Push → Build → Run Tests → Report → Deploy
```

**AI-Enhanced Pipeline:**
```
Code Push → Build → AI: Generate new tests for changed code
                  → Run Tests → AI: Analyze failures
                  → AI: Smart reporting → Deploy
                  → AI: Monitor & alert
```

### Where AI Fits in CI/CD

| Pipeline Stage | AI Enhancement |
|---------------|----------------|
| Pre-commit | AI code review, test suggestions |
| Build | AI-generated tests for new code |
| Test Execution | Smart test selection, prioritization |
| Failure Analysis | Automated root cause analysis |
| Reporting | Intelligent summaries, trend analysis |
| Post-deploy | AI-powered monitoring, anomaly detection |

---

## 5.2 Using AI APIs in Test Frameworks

### OpenAI API Integration

```python
import openai
import os

class AITestHelper:
    """Helper class for AI-assisted testing."""
    
    def __init__(self):
        self.client = openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    
    def analyze_failure(self, test_name, error_message, logs=""):
        """Analyze a test failure and suggest fixes."""
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{
                "role": "system",
                "content": "You are a test failure analyst. Provide concise root cause analysis."
            }, {
                "role": "user", 
                "content": f"""Test: {test_name}
Error: {error_message}
Logs: {logs}

Analyze this failure and suggest:
1. Probable root cause
2. Is this a test issue or application bug?
3. Suggested fix"""
            }],
            temperature=0.3,
            max_tokens=500
        )
        return response.choices[0].message.content
    
    def generate_test_data(self, schema, count=10):
        """Generate test data based on a schema."""
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{
                "role": "user",
                "content": f"Generate {count} test records as JSON array for this schema: {schema}"
            }],
            temperature=0.7,
            max_tokens=2000
        )
        return response.choices[0].message.content
    
    def suggest_tests_for_diff(self, code_diff):
        """Suggest test cases for code changes."""
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{
                "role": "system",
                "content": "You are a QA engineer. Suggest test cases for code changes."
            }, {
                "role": "user",
                "content": f"Code diff:\n{code_diff}\n\nSuggest specific test cases needed."
            }],
            temperature=0.5,
            max_tokens=1000
        )
        return response.choices[0].message.content
```

### pytest Plugin for AI Analysis

```python
# conftest.py — AI-powered failure analysis
import pytest

ai_helper = None

def pytest_configure(config):
    global ai_helper
    try:
        from ai_test_helper import AITestHelper
        ai_helper = AITestHelper()
    except Exception:
        pass

@pytest.hookimpl(tryfirst=True, hookwrapper=True)
def pytest_runtest_makereport(item, call):
    outcome = yield
    report = outcome.get_result()
    
    if report.failed and ai_helper and call.excinfo:
        error_msg = str(call.excinfo.value)
        analysis = ai_helper.analyze_failure(
            test_name=item.name,
            error_message=error_msg
        )
        report.sections.append(("AI Analysis", analysis))
```

---

## 5.3 GitHub Actions with AI

### AI-Enhanced Test Workflow

```yaml
# .github/workflows/ai-testing.yml
name: AI-Enhanced Testing

on:
  pull_request:
    branches: [main]

jobs:
  ai-test-suggestions:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      
      - name: Get changed files
        id: changed
        run: |
          echo "files=$(git diff --name-only origin/main...HEAD | tr '\n' ' ')" >> $GITHUB_OUTPUT
      
      - name: AI Test Suggestions
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          python scripts/ai_test_suggestions.py "${{ steps.changed.outputs.files }}"

  run-tests:
    runs-on: ubuntu-latest
    needs: ai-test-suggestions
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      
      - name: Install dependencies
        run: pip install -r requirements.txt
      
      - name: Run tests
        run: pytest --tb=long -v 2>&1 | tee test_output.txt
      
      - name: AI Failure Analysis
        if: failure()
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: python scripts/ai_analyze_failures.py test_output.txt
```

---

## 5.4 Automated Test Generation on Code Change

### Architecture

```
┌──────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────┐
│  Git Push │───▶│ Detect changed│───▶│ AI generates  │───▶│ Run new  │
│           │    │ files/modules │    │ test cases    │    │ tests    │
└──────────┘    └──────────────┘    └──────────────┘    └──────────┘
```

### Implementation Script

```python
# scripts/ai_test_suggestions.py
import subprocess
import sys
from ai_test_helper import AITestHelper

def get_git_diff(files):
    """Get the diff for changed files."""
    diffs = {}
    for f in files:
        try:
            result = subprocess.run(
                ["git", "diff", "origin/main...HEAD", "--", f],
                capture_output=True, text=True
            )
            if result.stdout:
                diffs[f] = result.stdout
        except Exception:
            pass
    return diffs

def main():
    files = sys.argv[1].split() if len(sys.argv) > 1 else []
    if not files:
        print("No changed files detected.")
        return
    
    ai = AITestHelper()
    diffs = get_git_diff(files)
    
    print("## AI Test Suggestions for Changed Code\n")
    for filename, diff in diffs.items():
        print(f"### {filename}\n")
        suggestions = ai.suggest_tests_for_diff(diff)
        print(suggestions)
        print()

if __name__ == "__main__":
    main()
```

---

## 5.5 AI-Powered Test Reporting

### Smart Test Summary

```python
def generate_test_report(test_results):
    """Generate an AI-powered test summary."""
    ai = AITestHelper()
    
    summary_prompt = f"""
    Analyze these test results and provide:
    1. Executive summary (2-3 sentences)
    2. Key failures that need immediate attention
    3. Trends compared to previous runs
    4. Risk assessment for deployment
    
    Results:
    Total: {test_results['total']}
    Passed: {test_results['passed']}
    Failed: {test_results['failed']}
    Skipped: {test_results['skipped']}
    
    Failed tests:
    {test_results['failure_details']}
    """
    
    return ai.client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": summary_prompt}],
        temperature=0.3
    ).choices[0].message.content
```

---

## 5.6 Cost Management

### Estimating API Costs

| Model | Input Cost | Output Cost | Typical Test Analysis |
|-------|-----------|-------------|----------------------|
| GPT-4o-mini | $0.15/1M tokens | $0.60/1M tokens | ~$0.001 per analysis |
| GPT-4o | $2.50/1M tokens | $10.00/1M tokens | ~$0.02 per analysis |
| Claude Sonnet | $3.00/1M tokens | $15.00/1M tokens | ~$0.03 per analysis |

### Best Practices for Cost Control

1. Use GPT-4o-mini for routine tasks (failure analysis, data generation)
2. Reserve GPT-4o/Claude for complex analysis
3. Cache AI responses for identical inputs
4. Set token limits on API calls
5. Monitor usage with API dashboards

---

## 📖 Module Summary

1. AI enhances every stage of the **CI/CD pipeline**
2. **pytest plugins** can automatically analyze test failures
3. **GitHub Actions** can trigger AI analysis on code changes
4. Always consider **API costs** when building AI pipelines
5. Start small — add AI to **one stage** and expand gradually

> **Next**: [Lab 1 — AI in CI/CD](labs/lab-01-ai-cicd.md)
