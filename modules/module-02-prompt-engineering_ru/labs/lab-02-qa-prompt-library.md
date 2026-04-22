# Lab 2: Building a QA Prompt Library

## 🎯 Objective

Create a reusable collection of tested, optimized prompts for common QA tasks that you can use in your daily work.

## ⏱️ Время: 2 часов

---

## Task

Build a prompt library file `exercises/qa_prompt_library.md` with at least 10 prompts organized by category. For each prompt, test it with an AI tool and refine it until the output quality is consistently good.

## Required Categories and Prompts

### Category 1: Test Case Generation (3 prompts minimum)

Create prompts for:
1. **Functional test cases** from a user story
2. **API test cases** from an endpoint specification
3. **Boundary value test cases** for a form with multiple fields

### Category 2: Bug Reporting (2 prompts minimum)

Create prompts for:
1. **Writing a new bug report** from a description of observed behavior
2. **Improving an existing bug report** that lacks detail

### Category 3: Test Data (2 prompts minimum)

Create prompts for:
1. **Generating realistic test data** in JSON format
2. **Generating edge case data** for specific field types

### Category 4: Analysis (2 prompts minimum)

Create prompts for:
1. **Requirement analysis** — finding ambiguities and gaps
2. **Test coverage analysis** — identifying untested scenarios

### Category 5: Automation (1 prompt minimum)

Create a prompt for:
1. **Generating automated test code** from manual test cases

---

## Template for Each Prompt Entry

```markdown
### Prompt: [NAME]

**Category**: [Category]
**Technique**: [Zero-shot / Few-shot / CoT / Role-based]
**Best AI Tool**: [ChatGPT / Claude / Either]

**Template**:
```
[The actual prompt with [PLACEHOLDERS] for variable parts]
```

**Example Usage**:
```
[The prompt with placeholders filled in]
```

**Sample Output Quality**: ⭐⭐⭐⭐⭐ (rate 1-5)

**Tips**:
- [Tip for getting best results]
- [Common pitfall to avoid]
```

---

## Критерии оценки for Each Prompt

Before adding a prompt to your library, verify:
- [ ] It produces consistent results across multiple runs
- [ ] The output format matches your needs
- [ ] It handles different input variations well
- [ ] You've iterated at least once to improve it

---

## ✅ Completion Checklist

- [ ] At least 10 prompts created
- [ ] All 5 categories covered
- [ ] Each prompt tested with AI and rated
- [ ] Tips documented for each prompt
- [ ] Library saved as `exercises/qa_prompt_library.md`

## 🔗 Next

Continue to [Quiz](../quiz.md)


> 🌐 [English version](../module-02-prompt-engineering/labs/lab-02-qa-prompt-library.md)
