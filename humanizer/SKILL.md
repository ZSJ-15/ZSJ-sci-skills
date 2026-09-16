---
name: humanizer
description: Polish academic text to remove AI-typical phrasing while keeping precision and natural flow.
---

# Humanizer for Academic Writing

## Goal
Make AI-generated text sound like a native researcher wrote it, avoiding robotic patterns.

## Rules
- Vary sentence length (short + long mix).
- Replace repetitive transition words ("Furthermore", "Moreover").
- Avoid clichés: "It is worth noting", "In conclusion", "Overall".
- Use active voice naturally; passive only for methods.
- Keep discipline-specific terminology but reduce redundancy.

## Workflow
1. Read original text for core meaning.
2. Rewrite 20% of sentences with different structure.
3. Break long paragraphs (>5 lines) into shorter ones.
4. Check rhythm: no 3 consecutive similar-length sentences.
5. Final pass: ensure no hallucinated facts.

## Constraints
- Do NOT change data, p-values, citations.
- Preserve author's original intent.
- Output should pass AI-detection heuristics (varied perplexity).
