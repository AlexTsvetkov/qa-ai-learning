# Module 6: Theory — Autonomous Testing & Future

## 6.1 Self-Healing Tests

### The Problem: Brittle Tests

Tests break when the UI changes, even if functionality is intact:

```python
# Brittle test — breaks if ID changes
driver.find_element(By.ID, "submit-btn-v2")  # Was "submit-btn"

# Brittle test — breaks if class changes
driver.find_element(By.CSS_SELECTOR, ".btn-primary.new-style")
```

### Self-Healing Strategy

```
Test Fails → Capture error context → AI analyzes the page
           → AI suggests new locator → Retry with new locator
           → Update test if successful → Log the heal
```

### Implementation Approach

```python
import os
from openai import OpenAI
from playwright.sync_api import Page

class SelfHealingLocator:
    """Locator that can heal itself using AI when elements aren't found."""
    
    def __init__(self, page: Page, original_locator: str, description: str):
        self.page = page
        self.original_locator = original_locator
        self.description = description  # Human description of the element
        self.client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    
    def find(self):
        """Try original locator, fall back to AI-assisted healing."""
        try:
            element = self.page.locator(self.original_locator)
            if element.count() > 0:
                return element
        except Exception:
            pass
        
        # Original locator failed — attempt to heal
        return self._heal()
    
    def _heal(self):
        """Use AI to find the element with a new locator."""
        # Get page HTML (simplified for context)
        page_html = self.page.content()
        
        # Truncate HTML to fit in context
        if len(page_html) > 10000:
            page_html = page_html[:10000] + "...[truncated]"
        
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{
                "role": "system",
                "content": "You are a test automation expert. "
                           "Given HTML and a description, return ONLY a CSS selector."
            }, {
                "role": "user",
                "content": f"""The original locator '{self.original_locator}' no longer works.
                
Element description: {self.description}

Page HTML:
{page_html}

Return ONLY a CSS selector that matches this element. No explanation."""
            }],
            temperature=0,
            max_tokens=100
        )
        
        new_locator = response.choices[0].message.content.strip()
        print(f"🔧 Self-healed: '{self.original_locator}' → '{new_locator}'")
        
        element = self.page.locator(new_locator)
        if element.count() > 0:
            self._log_healing(new_locator)
            return element
        
        raise Exception(f"Self-healing failed for: {self.description}")
    
    def _log_healing(self, new_locator):
        """Log the healing for later review."""
        with open("healing_log.json", "a") as f:
            import json
            json.dump({
                "original": self.original_locator,
                "healed_to": new_locator,
                "description": self.description,
                "page_url": self.page.url
            }, f)
            f.write("\n")
```

---

## 6.2 AI Agents for Exploratory Testing

### What is an AI Testing Agent?

An AI testing agent autonomously:
1. Navigates a web application
2. Identifies interactive elements
3. Performs actions (clicks, fills forms, navigates)
4. Observes the results
5. Identifies potential bugs
6. Reports findings

### Architecture

```
┌────────────────────────────────────────────────────┐
│                   AI Testing Agent                   │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │
│  │ Browser   │  │ AI Brain │  │ Bug Reporter     │  │
│  │ (Playwright)│ │(LLM API) │  │ (findings log)   │  │
│  └──────────┘  └──────────┘  └──────────────────┘  │
│       │              │                │              │
│       ▼              ▼                ▼              │
│  See page → Decide action → Execute → Evaluate     │
│       ▲                                    │         │
│       └────────────────────────────────────┘         │
└────────────────────────────────────────────────────┘
```

### Simple Agent Implementation

```python
import json
from playwright.sync_api import sync_playwright
from openai import OpenAI

class AITestingAgent:
    """An autonomous AI agent that explores and tests a web application."""
    
    def __init__(self, base_url: str, max_steps: int = 20):
        self.base_url = base_url
        self.max_steps = max_steps
        self.client = OpenAI()
        self.findings = []
        self.visited_pages = set()
        self.action_history = []
    
    def run(self):
        """Run the testing agent."""
        with sync_playwright() as p:
            browser = p.chromium.launch(headless=False)
            page = browser.new_page()
            page.goto(self.base_url)
            
            for step in range(self.max_steps):
                print(f"\n--- Step {step + 1}/{self.max_steps} ---")
                
                # 1. Observe current state
                state = self._observe(page)
                
                # 2. Decide next action
                action = self._decide(state)
                
                # 3. Execute action
                result = self._execute(page, action)
                
                # 4. Evaluate result
                self._evaluate(page, action, result)
                
                self.action_history.append({
                    "step": step,
                    "action": action,
                    "result": result
                })
            
            browser.close()
        
        return self.findings
    
    def _observe(self, page):
        """Observe the current page state."""
        return {
            "url": page.url,
            "title": page.title(),
            "visible_text": page.inner_text("body")[:2000],
            "interactive_elements": self._get_interactive_elements(page),
            "console_errors": []  # Could capture console errors
        }
    
    def _get_interactive_elements(self, page):
        """Get all interactive elements on the page."""
        elements = page.evaluate("""() => {
            const items = [];
            document.querySelectorAll('a, button, input, select, textarea, [role="button"]')
                .forEach(el => {
                    items.push({
                        tag: el.tagName,
                        type: el.type || '',
                        text: el.textContent?.trim().substring(0, 50) || '',
                        id: el.id || '',
                        name: el.name || '',
                        href: el.href || '',
                        placeholder: el.placeholder || ''
                    });
                });
            return items.slice(0, 30);  // Limit to 30 elements
        }""")
        return elements
    
    def _decide(self, state):
        """Use AI to decide the next action."""
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{
                "role": "system",
                "content": """You are an exploratory testing agent. 
                Decide the next action to test the application.
                Return JSON: {"action": "click|fill|navigate|check", 
                "target": "selector or URL", "value": "text to fill (if applicable)",
                "reasoning": "why this action"}"""
            }, {
                "role": "user",
                "content": f"""Current state:
URL: {state['url']}
Title: {state['title']}
Interactive elements: {json.dumps(state['interactive_elements'][:15])}
Previous actions: {json.dumps(self.action_history[-5:])}
Pages visited: {list(self.visited_pages)}

What should I test next? Return JSON only."""
            }],
            temperature=0.7,
            max_tokens=200
        )
        
        try:
            return json.loads(response.choices[0].message.content)
        except json.JSONDecodeError:
            return {"action": "navigate", "target": self.base_url, 
                    "reasoning": "Failed to parse, returning home"}
    
    def _execute(self, page, action):
        """Execute the decided action."""
        try:
            if action["action"] == "click":
                page.click(action["target"], timeout=5000)
                return "success"
            elif action["action"] == "fill":
                page.fill(action["target"], action.get("value", "test"))
                return "success"
            elif action["action"] == "navigate":
                page.goto(action["target"])
                self.visited_pages.add(action["target"])
                return "success"
            elif action["action"] == "check":
                return "observation"
        except Exception as e:
            return f"error: {str(e)}"
    
    def _evaluate(self, page, action, result):
        """Evaluate if the action revealed any issues."""
        if "error" in str(result):
            self.findings.append({
                "type": "potential_bug",
                "action": action,
                "error": result,
                "url": page.url,
                "severity": "medium"
            })
```

---

## 6.3 Visual AI Testing

### Concept

Visual testing compares screenshots to detect unintended UI changes:

```
Baseline Screenshot → Current Screenshot → AI Comparison
                                            ↓
                                     Differences found?
                                     ├── Yes → Report visual bug
                                     └── No → Pass
```

### AI-Enhanced Visual Testing vs Pixel Comparison

| Approach | False Positives | Handles Dynamic Content | Intelligence |
|----------|----------------|------------------------|-------------|
| Pixel diff | High | No | None |
| Perceptual diff | Medium | Partial | Low |
| AI-powered | Low | Yes | High |

### Инструменты for Visual AI Testing

- **Applitools Eyes** — Industry leader in visual AI testing
- **Percy (BrowserStack)** — Visual testing with smart comparison
- **Playwright Screenshots** + AI — DIY approach with LLM vision

---

## 6.4 Future Trends in AI-Powered QA

### Trend 1: Fully Autonomous Testing
- AI agents that can test entire applications independently
- Self-generating and self-maintaining test suites
- Continuous exploratory testing in production

### Trend 2: AI-Native Test Frameworks
- Testing frameworks built around AI from the ground up
- Natural language test definitions
- Auto-healing as a first-class feature

### Trend 3: Predictive Quality
- AI predicting where bugs are likely to occur
- Risk-based test selection powered by ML
- Code change impact analysis

### Trend 4: AI-Powered Quality Gates
- Intelligent deployment decisions based on AI analysis
- Automatic rollback triggered by anomaly detection
- Quality score prediction before deployment

### Trend 5: Collaborative AI Testing
- AI agents working alongside human testers
- AI suggesting what to test, humans deciding how
- Shared learning between AI and QA teams

---

## 6.5 Ethical Considerations

### Responsibility
- AI augments QA engineers, doesn't replace them
- Humans are accountable for quality decisions
- AI recommendations need human validation

### Bias and Fairness
- AI may have biases from training data
- Test data generated by AI may not represent all user groups
- Accessibility and inclusivity must be human-verified

### Transparency
- Document AI's role in testing decisions
- Keep audit trails of AI-generated artifacts
- Be transparent with stakeholders about AI usage

---

## 📖 Итоги модуля

1. **Self-healing tests** reduce maintenance by adapting to UI changes
2. **AI testing agents** can autonomously explore applications
3. **Visual AI testing** reduces false positives in screenshot comparison
4. The future of QA is **human-AI collaboration**, not replacement
5. **Ethical considerations** must guide AI adoption in testing

> **Next**: [Lab 1 — Building Self-Healing Tests](labs/lab-01-self-healing-tests.md)


> 🌐 [English version](../module-06-autonomous-testing/theory.md)
