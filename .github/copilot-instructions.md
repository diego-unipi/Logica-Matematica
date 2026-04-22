# Copilot Workspace Instructions

These rules apply to assistant behavior in this workspace.

## Math Delimiter Output Policy
- In assistant-authored content, inline math must use `$...$`.
- In assistant-authored content, display math must use `$$...$$`.
- Never output `\(...\)` or `\[...\]` in assistant-authored mathematics.

## Italian Accent Preservation Policy
- Absolute rule: never replace accented Italian letters with apostrophe approximations in assistant-authored text.
- Forbidden replacements include forms like `e\`` , `a'`, `i'`, `o\`` , `u\`` when used as substitutes for accented letters in prose.
- During corrections, preserve original accented letters exactly in user-authored text.
- In assistant-authored Italian prose, write proper accented letters and never apostrophized substitutes.
- This rule applies to chat output and generated edits.

## evan.sty Macro Rendering and Vector Formatting Policy

See [evan-sty-and-vectors.instructions.md](.github/instructions/evan-sty-and-vectors.instructions.md) for complete redirection rules and vector formatting guidelines.

**Summary:**
- All evan.sty macros redirect to canonical LaTeX forms in chat output
- All vector variables render in bold (`\mathbf{...}`) in assistant-authored content
- Single-letter variables in vector-like contexts (e.g., `\norm{x}`) default to bold
- Tuple vectors bold only when explicitly called vectors in the text
- No user clarification needed; apply defaults consistently

## Deterministic Expansion Rules (Chat)
- For macros declared in `evan.sty` via `\DeclareMathOperator{\cmd}{name}`, render as `\operatorname{name}`.
- For zero-argument aliases declared in `evan.sty`, expand exactly to their declared body.
- For one-argument wrappers declared in `evan.sty`, substitute argument into the declared template exactly.
- For style-only wrappers, keep semantic payload and drop visual styling if it harms renderer compatibility.
- Prefer deterministic expansion over emitting raw custom macros when a verified mapping exists.

If a custom macro from `evan.sty` has no verified direct expansion, ask one concise clarification question and do not guess.

## Strict No-Invention Policy (Chat)
- Never invent new symbols, macros, operators, variable names, or indices.
- Never substitute with look-alike symbols unless the substitution is an explicit mapping from `evan.sty`.
- Expand macros only with:
	1. exact meanings from `evan.sty` mappings
	2. standard renderer-safe LaTeX/KaTeX commands
- If a macro is unknown or mapping is uncertain:
	1. do not guess
	2. keep the original token verbatim only in quoted input blocks, not in assistant-authored math output
	3. ask exactly one concise clarification question
- Do not replace LaTeX commands with Unicode math glyphs unless the user explicitly asks for Unicode output.
- Preserve user symbol fidelity in assistant-authored math (same identifiers and subscripts), unless the user explicitly requests renaming.

## User Custom Command Redirection (Chat)
- If the user writes custom shorthand or non-canonical delimiter commands in chat, treat them as input aliases.
- Redirect assistant output to canonical delimiters only: `$...$` and `$$...$$`.
- User-authored message text is not rewritten in place; only assistant-authored output is normalized.
- When user input contains raw elementary-machine tokens, include the normalized rewritten form in the assistant reply.
- Do not leave raw elementary-machine tokens in assistant-authored output when a mapping exists.
- Never change user-authored math delimiters in existing code during normal edits, including `\[...\]` and `\(...\)`.
- Convert existing user-authored delimiters only when the user explicitly asks to convert that exact text.
- If a custom command is ambiguous, ask exactly one concise clarification question and then continue.
