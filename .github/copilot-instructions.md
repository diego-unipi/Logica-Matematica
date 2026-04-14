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

## evan.sty Macro Rendering Policy (Chat)
- Treat custom macros from `evan.sty` as input aliases.
- In assistant-authored chat math, prefer renderer-safe canonical expansions instead of raw custom macros.
- Keep assistant output renderable with standard LaTeX/KaTeX commands whenever possible.
- Never rewrite existing user-authored source macros unless the user explicitly asks for conversion.
- If the user explicitly asks to preserve raw macro syntax, provide both:
	1. a rendered canonical math form using `$...$` or `$$...$$`
	2. the exact raw macro form in a fenced `tex` code block

### Preferred Chat Expansions From evan.sty
- `\CC`, `\FF`, `\NN`, `\QQ`, `\RR`, `\ZZ` -> `\mathbb{C}`, `\mathbb{F}`, `\mathbb{N}`, `\mathbb{Q}`, `\mathbb{R}`, `\mathbb{Z}`
- `\N` -> `\mathbb{N}`
- `\set{X}` -> `\left\{ X \right\}`
- `\abs{x}` -> `\left\lvert x \right\rvert`
- `\norm{x}` -> `\left\lVert x \right\rVert`
- `\floor{x}` -> `\left\lfloor x \right\rfloor`
- `\ceiling{x}` -> `\left\lceil x \right\rceil`
- `\Mydef` -> `\overset{\text{def}}{=}`
- `\defiff` -> `\overset{\text{def}}{\iff}`
- `\liff` -> `\leftrightarrow`
- `\lthen` -> `\rightarrow`
- `\injto` -> `\hookrightarrow`
- `\surjto` -> `\twoheadrightarrow`
- `\bij` -> `\xrightarrow{\sim}`
- `\nmodels` -> `\nvDash`
- `\Th` -> `\operatorname{Th}`
- `\var`, `\Var` -> `\operatorname{var}`, `\operatorname{Var}`
- `\ar`, `\vl`, `\ED`, `\diag`, `\id` -> `\operatorname{ar}`, `\operatorname{vl}`, `\operatorname{ED}`, `\operatorname{diag}`, `\operatorname{id}`
- `\defeq` -> `\overset{\mathrm{def}}{=}`
- `\ol`, `\ul`, `\wt`, `\wh`, `\eps` -> `\overline`, `\underline`, `\widetilde`, `\widehat`, `\varepsilon`
- `\ps`, `\psf`, `\rel`, `\Cl` -> `\mathcal{P}`, `\mathcal{P}^{\text{fin}}`, `\mathcal{R}`, `\mathcal{C}\ell`
- `\cons`, `\car`, `\cdr`, `\inp`, `\outp`, `\push`, `\rem`, `\exec`, `\ttif`, `\nth`, `\length`, `\head`, `\cat`, `\step`, `\pred` -> corresponding `\texttt{...}` forms
- `\dotminus` -> `\mathbin{\dot{-}}`
- `\dotminu` -> `\mathbin{\dot{-}}` (compatibility alias)
- `\purple{X}`, `\vocab{X}`, `\alert{X}` -> preserve payload `X` and drop style-only formatting in chat math

### Elementary Machine Command Redirection (Chat)
- `\cons` -> `\texttt{cons}`
- `\car` -> `\texttt{car}`
- `\cdr` -> `\texttt{cdr}`
- `\inp` -> `\texttt{input}`
- `\outp` -> `\texttt{output}`
- `\push` -> `\texttt{push}`
- `\rem` -> `\texttt{rem}`
- `\exec` -> `\texttt{exec}`
- `\ttif` -> `\texttt{if}`
- `\nth` -> `\texttt{nth}`
- `\length` -> `\texttt{length}`
- `\head` -> `\texttt{head}`
- `\cat` -> `\texttt{cat}`
- `\step` -> `\texttt{step}`
- `\pred` -> `\texttt{pred}`
- Preserve superscripts/subscripts and arguments during redirection.
- Example: `\exec_{2,2} g` -> `\texttt{exec}_{2,2} g`.
- Example: `\step^n(\ulcorner \delta \urcorner, c)` -> `\texttt{step}^n(\ulcorner \delta \urcorner, c)`.
- Hard precedence rule: when user input contains `\push`, assistant-authored math output must render `\texttt{push}` and must not mirror raw `\push`.
- The only exception is explicit verbatim quoting requested by the user, provided in a fenced `tex` block.

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
