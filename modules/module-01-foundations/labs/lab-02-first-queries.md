# Lab 2: First Queries to AI

## 🎯 Objective

Learn to interact with AI assistants through both web interfaces and APIs, and understand the basics of crafting queries for QA tasks.

## ⏱️ Estimated Time: 1.5 hours

## 📋 Prerequisites

- Completed [Lab 1: Setting Up the Working Environment](lab-01-setup.md)
- At least one AI account (ChatGPT or Claude)

---

## Part 1: Using Web Interfaces

### Exercise 1.1: ChatGPT for Test Case Ideas

1. Open https://chat.openai.com
2. Enter the following prompt:

```
I am a QA engineer. I need to test a login page that has:
- Email field
- Password field
- "Remember me" checkbox
- "Forgot password" link
- "Login" button
- "Sign up" link

Please generate a list of test cases covering positive, negative, 
boundary, and security scenarios. Format as a numbered list with 
test case name and brief description.
```

3. Review the output
4. **Your task**: Compare the AI output with what you would write manually. Note:
   - What did AI catch that you might have missed?
   - What did AI miss that you would include?
   - Were there any incorrect suggestions?

### Exercise 1.2: Claude for Requirements Analysis

1. Open https://claude.ai
2. Enter the following prompt:

```
Analyze the following requirement and identify potential issues, 
ambiguities, and missing details that a QA engineer should clarify:

"The system shall allow users to upload files. Files should be 
validated and stored securely. Users should receive a confirmation 
after successful upload."
```

3. **Your task**: Write down 3 additional questions you would ask that AI didn't mention.

### Exercise 1.3: Comparing AI Responses

1. Ask the **same question** to both ChatGPT and Claude:

```
Write a bug report for the following issue:
When clicking the "Submit" button twice quickly on the payment form, 
the payment is processed twice and the user is charged double.
```

2. **Your task**: Compare both responses. Which is more detailed? Which follows better bug report structure?

---

## Part 2: Using APIs Programmatically

### Exercise 2.1: OpenAI API

Create a file `exercises/lab02_openai.py`:

```python
import os
from openai import OpenAI

# Load environment variables
try:
    from dotenv import load_dotenv
    load_dotenv()
except ImportError:
    pass

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

def ask_openai(prompt: str, model: str = "gpt-4o-mini") -> str:
    """Send a prompt to OpenAI and return the response."""
    response = client.chat.completions.create(
        model=model,
        messages=[
            {
                "role": "system",
                "content": "You are a senior QA engineer assistant. "
                           "Provide detailed, practical answers for testing tasks."
            },
            {
                "role": "user",
                "content": prompt
            }
        ],
        temperature=0.7,
        max_tokens=1000
    )
    return response.choices[0].message.content

# Example: Generate test cases
prompt = """
Generate 5 test cases for a search functionality that:
- Accepts text input
- Has filters for date range and category
- Shows results in a paginated list
- Highlights matching terms

Format each test case as:
- ID: TC-XXX
- Title: <title>
- Steps: <numbered steps>
- Expected Result: <expected outcome>
"""

if __name__ == "__main__":
    print("Sending request to OpenAI...")
    result = ask_openai(prompt)
    print("\n--- AI Response ---\n")
    print(result)
```

Run it:
```bash
python exercises/lab02_openai.py
```

### Exercise 2.2: Anthropic API

Create a file `exercises/lab02_anthropic.py`:

```python
import os
import anthropic

# Load environment variables
try:
    from dotenv import load_dotenv
    load_dotenv()
except ImportError:
    pass

client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

def ask_claude(prompt: str, model: str = "claude-sonnet-4-20250514") -> str:
    """Send a prompt to Claude and return the response."""
    message = client.messages.create(
        model=model,
        max_tokens=1000,
        system="You are a senior QA engineer assistant. "
               "Provide detailed, practical answers for testing tasks.",
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )
    return message.content[0].text

# Example: Analyze a bug report
prompt = """
Analyze this bug report and suggest improvements:

Title: Button doesn't work
Steps: Click the button
Expected: It should work
Actual: It doesn't work
Priority: High

Please rewrite this as a professional bug report with all necessary details,
and explain what information is missing.
"""

if __name__ == "__main__":
    print("Sending request to Claude...")
    result = ask_claude(prompt)
    print("\n--- AI Response ---\n")
    print(result)
```

Run it:
```bash
python exercises/lab02_anthropic.py
```

### Exercise 2.3: Experimenting with Temperature

Create a file `exercises/lab02_temperature.py`:

```python
import os
from openai import OpenAI

try:
    from dotenv import load_dotenv
    load_dotenv()
except ImportError:
    pass

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

prompt = "Suggest 3 creative edge cases for testing a date picker component."

print("=" * 60)
print("TEMPERATURE EXPERIMENT")
print("=" * 60)

for temp in [0.0, 0.5, 1.0]:
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}],
        temperature=temp,
        max_tokens=500
    )
    print(f"\n--- Temperature: {temp} ---")
    print(response.choices[0].message.content)
    print()
```

Run and observe how temperature affects creativity:
```bash
python exercises/lab02_temperature.py
```

---

## Part 3: Reflection

Answer these questions in a file `exercises/lab02_reflection.md`:

1. **Which AI tool felt more natural to use** for QA tasks — ChatGPT or Claude? Why?
2. **What surprised you** about the AI responses?
3. **What limitations did you notice** in the AI's understanding of testing?
4. **How would you use AI** in your daily QA work after this lab?
5. **What concerns do you have** about using AI for testing?

---

## ✅ Completion Checklist

- [ ] Completed web interface exercises (1.1, 1.2, 1.3)
- [ ] Successfully called OpenAI API
- [ ] Successfully called Anthropic API
- [ ] Completed temperature experiment
- [ ] Written reflection answers

## 🔗 Next

Continue to [Quiz](../quiz.md)
