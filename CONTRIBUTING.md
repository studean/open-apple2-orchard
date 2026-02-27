# Contributing to the Apple II Codex

Thank you for contributing! This project converts vintage Apple II reference 
material into structured formats suitable for RAG systems and LLM training.
Consistency across contributors is critical to the dataset's usefulness, so 
please read this carefully before submitting.

---

## Hex and Decimal Conventions

**Always use `$` prefix for hex, never `0x`.** The `0x` convention is from C 
and modern programming — the Apple II community has always used `$`.

✅ `"hex_start": "$C030"`  
❌ `"hex_start": "0xC030"`

For BASIC code examples, **use decimal** — Applesoft BASIC does not accept 
hex in PEEK/POKE/CALL statements.

✅ `POKE 49200,0`  
❌ `POKE $C030,0`

---

## JSON Structure

### Memory Locations (PEEK/POKE)
````json
{
  "address": {
    "hex_start": "$0020",
    "decimal_start": 32
  },
  "basic_variant": "applesoft",
  "category": "display",
  "description": "Text window left edge",
  "usage": {
    "code_snippet": "POKE 32,N",
    "explanation": "N is 0-39. Sets the leftmost column of the text window."
  }
}
````

For **address ranges**, include all four fields:
````json
"address": {
  "hex_start": "$0000",
  "hex_end":   "$0005",
  "decimal_start": 0,
  "decimal_end":   5
}
````

For **single addresses**, omit the `_end` fields entirely — do not null them out.

### CALL Subroutines
````json
{
  "address": {
    "hex": "$FB2F",
    "decimal": -1233
  },
  "basic_variant": "both",
  "category": "display",
  "description": "Clear text and lo-res screen",
  "usage": {
    "code_snippet": "CALL -1233",
    "explanation": "Clears the current text page and lo-res graphics screen."
  }
}
````

---

## Field Reference

### `basic_variant`
| Value | Meaning |
|---|---|
| `"applesoft"` | Applesoft BASIC only (marked `*` in source) |
| `"integer"` | Integer BASIC only (marked `**` in source) |
| `"both"` | Works in both (no asterisk in source) |

### `category`
Use one of the established categories. Add to this list via a PR if genuinely 
needed — don't invent synonyms:

`display`, `input`, `audio`, `memory`, `system`, `graphics`, `dos`, 
`storage`, `io`

### `usage`
Omit the `usage` field entirely if no practical PEEK/POKE/CALL example exists 
for the address. Do not include a null or empty usage object.

---

## Documentation Page Format (Markdown)

For reference pages (statements, concepts, patterns), use this structure:
````markdown
---
topic: "READ, DATA, RESTORE"
category: "data_storage"
statements: ["READ", "DATA", "RESTORE"]
related: ["INPUT", "GOTO"]
keywords: ["sequential data", "data pointer", "restore pointer"]
---

# [Topic Name]

[Plain language explanation — what it does and why you'd use it.]

[Key rules as prose, not bullet points where possible.]

## Example: [Descriptive Title]
```applesoft
10 ...
```

[Explanation of what the example demonstrates.]

**Pattern demonstrated:** [One-line summary of the idiom shown.]
````

---

## What NOT to Do

- Don't use `null` for omitted optional fields — omit them entirely
- Don't use `0x` hex notation anywhere
- Don't put BASIC code examples inside JSON string values that use hex addresses
- Don't invent new `category` values without a PR discussion
- Don't clean up or "modernize" code examples — preserve authentic Applesoft 
  syntax including ALL CAPS keywords

---

## OCR and Source Cleanup

Many pages use `Ø` (the letter) instead of `0` (zero) due to vintage 
typesetting. **Always normalize these to numeric zero** in both JSON and 
code examples. When in doubt, cross-reference the address against the memory 
map to confirm.
