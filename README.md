[English](README.md) | [繁體中文](README.zh.md)

# Plain Speak

A Claude Code skill that turns complex jargon, acronyms, and domain-specific terminology into clear, verified plain-language explanations. Built with an anti-hallucination mechanism: every term is web-searched and cross-checked before being explained.

## Features

- Three-tier classification: common terms (quick verify), specialist jargon (standard search), and cutting-edge concepts (deep search with source priority)
- Mandatory web search before explaining — never guesses, always cites sources
- Three-layer explanation pattern: one-sentence summary, everyday analogy, concrete example
- Adapts depth and analogy style to the user's level (non-technical, junior dev, senior dev, domain expert)
- Batch mode: explains multiple terms with a summary table when three or more are provided
- Source priority: Official docs > Wikipedia > Reputable tech blogs > Stack Overflow

## Usage

```
/plain-speak
```

Trigger phrases: "explain this term", "what is X", "what does Y mean", "explain like I'm 5", "ELI5", "這是什麼意思", "幫我解釋", "白話解釋", "名詞解釋", or when encountering jargon in a passage.

## How It Works

The skill classifies the term's domain and complexity tier, runs a WebSearch to verify the definition, then structures the explanation in three layers — one-sentence version, plain analogy, and a minimal concrete example. If the search result conflicts with training knowledge, the web source takes precedence; if no reliable source is found, uncertainty is stated explicitly rather than fabricated.

## Requirements

- Claude Code CLI
- WebSearch tool available in Claude Code environment

## License

MIT
