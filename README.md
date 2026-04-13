# Prompt Enhancer Skill

Turn vague prompts into structured, expert-level prompts that get better results from any AI.

## What is Prompt Enhancer?

This skill takes basic, vague prompts and automatically enhances them using the R-CTCEO framework. It adds role definition, context, constraints, and output structure to help you get better results from any AI model.

## Installation

```bash
# Claude Code
cp -r prompt-enhancer/ ~/.claude/skills/

# ChatGPT — paste SKILL.md contents into Custom Instructions or as a Custom GPT system prompt

# Ollama — use as system prompt via --system flag or Modelfile SYSTEM directive

# LM Studio — paste into the System Prompt field in chat settings

# Open WebUI — add as a System Prompt preset or custom model instruction

# OpenCode
cp -r prompt-enhancer/ ~/.config/opencode/skills/
```

## Usage

```bash
# Claude Code / OpenCode
/skill prompt-enhancer

# ChatGPT — start a conversation with your Custom GPT or paste SKILL.md as the first message

# Ollama
ollama run <model> --system "$(cat SKILL.md)"

# LM Studio — set System Prompt in chat settings, then start a new conversation

# Open WebUI — select your Prompt Enhancer preset, then start a new conversation
```

Then provide your raw prompt:
```
Write code to fetch user data from an API
```

The skill outputs two enhanced versions:
- **Concise** — 5-7 lines, enhanced but brief
- **Detailed** — Full R-CTCEO framework with all sections

## File Structure

```
prompt-enhancer/
├── README.md                    ← This file
├── SKILL.md                     ← Main skill documentation (load this)
└── reference/
    ├── best_practices.md        ← R-CTCEO framework guide
    └── examples_library.md      ← Before/after examples by domain
```

## How It Works

1. Load the skill with `/skill prompt-enhancer`
2. The AI reads `SKILL.md` and enters enhancement mode
3. You paste your raw prompt after `Enter Prompt:`
4. The AI applies R-CTCEO and outputs both versions immediately

**No MCP connection or external agents required.** This is a documentation-only skill — the AI reads `SKILL.md` and applies the framework directly.

## The R-CTCEO Framework

| Component | Purpose |
|-----------|---------|
| **R**ole | Specific expert persona with domain expertise |
| **C**ontext | Background, tech stack, audience, stakes |
| **T**ask | Deliverables, features, success criteria |
| **C**onstraints | Measurable technical and scope boundaries |
| **E**xamples | Few-shot demos for complex tasks |
| **O**utput Format | Explicit structure for the AI's response |

## Supported Domains

- **Security / AppSec** — OWASP, CVSS, severity ratings, remediation code
- **Penetration Testing** — PTES, MITRE ATT&CK, kill chains, PoC, red team reporting
- **Development** — Language-specific, code quality, testing standards
- **DevOps / Infrastructure** — Tool versions, compliance, SLA requirements
- **Data Analysis** — Methodology, statistical rigor, visualization
- **Technical Writing** — Audience level, style guides, format standards
- **Business Analysis** — Stakeholders, KPIs, timelines, budgets
- **Creative Writing** — Audience, tone, brand voice, content structure
- **Marketing** — Campaigns, channels, KPIs, audience targeting
- **Education** — Learning objectives, lesson plans, assessments, Bloom's taxonomy

## Examples

### Security Review

**Before:** `check this PHP for security issues`

**After (Concise):**
```
Act as an AppSec engineer. Perform a security review of the provided PHP code
against OWASP Top 10 2021. Identify vulnerabilities with specific line references,
CVSS 3.1 scores, severity ratings (Critical/High/Medium/Low), and concrete
remediation code examples.
```

**After (Detailed):**
```
ROLE: You are an Application Security Engineer specializing in PHP web
applications, OWASP standards, and secure code review.

CONTEXT: A PHP codebase requires a thorough security audit prior to deployment.
The review must identify exploitable vulnerabilities, rate risk, and provide
actionable remediation guidance.

TASK:
- SQL injection and parameterized query validation
- Cross-site scripting (XSS) — reflected, stored, DOM-based
- Authentication and session management flaws
- Insecure direct object references (IDOR)
- Hardcoded credentials and secrets
- Input validation and sanitization gaps
- Business logic vulnerabilities

CONSTRAINTS:
- Use OWASP Top 10 2021 and ASVS 4.0 as primary frameworks
- Provide CVSS 3.1 scores for each finding
- Include specific line numbers and code snippets
- Provide secure remediation code for each finding
- Rate severity: Critical, High, Medium, Low

OUTPUT FORMAT:
1. Executive Summary
2. Critical Findings (immediate action)
3. High Priority Issues (fix before production)
4. Medium/Low Issues (next sprint)
5. Remediation Roadmap
```

---

**Location:** `~/.claude/skills/prompt-enhancer/`  
**Last Updated:** April 2026
