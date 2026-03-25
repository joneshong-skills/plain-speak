---
name: plain-speak
description: >-
  This skill should be used when the user asks to "explain this term",
  "what is X", "what does Y mean", "explain like I'm 5", "這是什麼意思",
  "幫我解釋", "白話解釋", "用簡單的話說", "名詞解釋", "ELI5",
  encounters complex jargon, domain-specific terminology, acronyms,
  or technical concepts that need clear, verified explanation.
version: 0.1.0
tools:
  - WebSearch
  - WebFetch
io:
  input:
    - mime: "text/plain"
      description: "Term, concept, or jargon to explain"
  output:
    - mime: "text/markdown"
      description: "Verified plain-language explanation with examples"
---

# Plain Speak

Turn complex jargon into clear, verified explanations with concrete examples.
Anti-hallucination by design: web search before explaining.

## Agent Delegation

This skill runs entirely in main context. Conversational and lightweight —
no sub-agent delegation needed.

## Workflow

### Step 1: Classify the Term

Determine the domain and complexity tier:

| Tier | Description | Search Depth |
|------|-------------|-------------|
| **Common** | Widely known tech terms (API, DNS, OAuth) | Quick verify — 1 search |
| **Specialist** | Domain jargon (EBITDA, k8s CRD, CAP theorem) | Standard — 1-2 searches |
| **Cutting-edge** | Recent or niche (LoRA, RLHF, zkEVM) | Deep — 2-3 searches + source check |

### Step 2: Verify Before Speaking

**MANDATORY** — search before explaining. This is the core anti-hallucination mechanism.

1. `WebSearch` the term + domain context (e.g., "EBITDA finance definition")
2. Cross-check: Does the search result align with your training knowledge?
   - **Match** → proceed to explain, cite the source
   - **Conflict** → trust the web source, flag the discrepancy
   - **No results** → state uncertainty explicitly, do NOT fabricate
3. For cutting-edge terms, prefer official docs / original papers over blog posts

**Source priority**: Official docs > Wikipedia > Reputable tech blogs > Stack Overflow > Random blogs

### Step 3: Explain with the 3-Layer Pattern

Structure every explanation in three layers — the user can stop at any level:

```
## {Term}

**一句話版本**: [One sentence, zero jargon. A 10-year-old should get it.]

**白話解釋**: [2-3 sentences. Use an everyday analogy.
 Connect to something the user already knows.]

**具體範例**:
[A minimal, concrete example. Code snippet, number, or scenario.
 Show don't tell.]

> 來源: [URL or source name]
```

### Step 4: Adapt to User's Level

Based on user context (from memory or conversation):

| User Level | Analogy Style | Example Depth |
|------------|--------------|---------------|
| Non-technical | Everyday objects, cooking, traffic | No code, pure scenarios |
| Junior dev | Known tech concepts as anchors | Simple code, pseudo-code |
| Senior dev | System design parallels | Production-grade snippets |
| Domain expert | Cross-domain analogies only | Focus on nuance, edge cases |

Default to **junior dev** if unknown.

## Rules

1. **Never guess** — if WebSearch returns nothing useful, say "我找不到可靠來源" and explain what you *think* it means with a clear uncertainty marker
2. **No jargon to explain jargon** — every word in the explanation must be simpler than the term being explained
3. **One analogy minimum** — every specialist/cutting-edge term gets an analogy
4. **Cite sources** — always include where the info came from
5. **Batch mode** — if multiple terms, explain each separately in the 3-layer format
6. **Language** — match the user's language; default to 繁體中文 per global setting

## Quick Reference: Response Template

```markdown
## {Term Name}

**一句話版本**: {簡短定義}

**白話解釋**: {日常類比 + 為什麼它重要}

**具體範例**:
{程式碼/數字/情境}

> 來源: {URL}
```

For batch explanations (3+ terms), add a summary table at the top:

```markdown
| 名詞 | 一句話 | 領域 |
|------|--------|------|
| Term A | ... | Finance |
| Term B | ... | DevOps |
```
