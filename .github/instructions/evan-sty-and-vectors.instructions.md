---
description: "Redirect evan.sty abbreviations to canonical forms in chat and use bold formatting for all vectors."
name: "evan.sty Macro Redirection and Vector Formatting"
applyTo: "**"
---

# evan.sty Macro Redirection and Vector Formatting

Hard rules for assistant-authored chat output in this workspace.

## Platform Constraint
- User-authored chat messages are not rewritten in place by instructions.
- Redirection applies to assistant-authored output only.
- When user input contains raw evan.sty tokens, assistant must include the normalized rewritten form in its reply.

## Mandatory evan.sty Macro Redirection (Chat)

Always expand these evan.sty aliases in assistant-authored math output:

### Blackboard Bold Sets
- `\CC` → `\mathbb{C}`
- `\FF` → `\mathbb{F}`
- `\NN` → `\mathbb{N}`
- `\QQ` → `\mathbb{Q}`
- `\RR` → `\mathbb{R}`
- `\ZZ` → `\mathbb{Z}`
- `\N` → `\mathbb{N}`

### Set and Delimiter Operations
- `\set{X}` → `\left\{ X \right\}`
- `\abs{x}` → `\left\lvert x \right\rvert`
- `\norm{x}` → `\left\lVert x \right\rVert`
- `\floor{x}` → `\left\lfloor x \right\rfloor`
- `\ceiling{x}` → `\left\lceil x \right\rceil`

### Logical and Definition Operators
- `\Mydef` → `\overset{\text{def}}{=}`
- `\defiff` → `\overset{\text{def}}{\iff}`
- `\defeq` → `\overset{\mathrm{def}}{=}`
- `\liff` → `\leftrightarrow`
- `\lthen` → `\rightarrow`

### Arrow and Mapping Notation
- `\injto` → `\hookrightarrow`
- `\surjto` → `\twoheadrightarrow`
- `\bij` → `\xrightarrow{\sim}`
- `\nmodels` → `\nvDash`

### Operator Names (DeclareMathOperator)
- `\Th` → `\operatorname{Th}`
- `\var` → `\operatorname{var}`
- `\Var` → `\operatorname{Var}`
- `\ar` → `\operatorname{ar}`
- `\vl` → `\operatorname{vl}`
- `\ED` → `\operatorname{ED}`
- `\diag` → `\operatorname{diag}`
- `\id` → `\operatorname{id}`

### Accent and Style Modifiers
- `\ol{X}` → `\overline{X}`
- `\ul{X}` → `\underline{X}`
- `\wt{X}` → `\widetilde{X}`
- `\wh{X}` → `\widehat{X}`
- `\eps` → `\varepsilon`

### Calligraphic Operators
- `\ps` → `\mathcal{P}`
- `\psf` → `\mathcal{P}^{\text{fin}}`
- `\rel` → `\mathcal{R}`
- `\Cl` → `\mathcal{C}\ell`

### Elementary Machine Commands
- `\cons` → `\texttt{cons}`
- `\car` → `\texttt{car}`
- `\cdr` → `\texttt{cdr}`
- `\inp` → `\texttt{input}`
- `\outp` → `\texttt{output}`
- `\push` → `\texttt{push}`
- `\rem` → `\texttt{rem}`
- `\exec` → `\texttt{exec}`
- `\ttif` → `\texttt{if}`
- `\nth` → `\texttt{nth}`
- `\length` → `\texttt{length}`
- `\head` → `\texttt{head}`
- `\cat` → `\texttt{cat}`
- `\step` → `\texttt{step}`
- `\pred` → `\texttt{pred}`
- `\dotminu` → `\mathbin{\dot{-}}`

### Style-Only Wrappers (Preserve Payload, Drop Styling)
- `\purple{X}`, `\vocab{X}`, `\alert{X}` → keep payload `X` and drop visual formatting in chat math

## Mandatory Vector Formatting (Chat)

**Hard rule: All vectors must be rendered in bold in assistant-authored chat output.**

### Definition
Vectors are:
- Explicit variables declared or understood as vectors in context (e.g., $\mathbf{v}$, $\mathbf{w}$)
- Elements of vector spaces
- Tuple notations when they represent vector quantities
- Sequences understood as vector-like objects in linear algebra or geometric context

### Application Rules
1. Always wrap vector variables in `\mathbf{...}` in math mode.
2. Apply to both declared vectors and single letters that appear in vector contexts.
   - Example: $\mathbf{v}$, $\mathbf{u}$, $\mathbf{x}$, $\mathbf{e}_i$
3. Preserve vector notation subscripts and superscripts within the bold.
   - Example: `\mathbf{v}_1`, `\mathbf{e}_{i,j}`, not $\mathbf{v_1}$
4. **For tuple/sequence vectors, bold only if explicitly called a vector in the text.**
   - Example: "the vector $(a_1, a_2, \ldots, a_n)$" → bold as $\mathbf{(a_1, a_2, \ldots, a_n)}$
   - Example: "the sequence $(a_1, a_2, \ldots)$" → do NOT bold
5. **Default to bold for ambiguous single-letter variables in vector-like contexts** (e.g., as arguments to `\norm{}`, `\abs{}`, or in linear algebra operations).
   - Example: `\norm{x}` → `\left\lVert \mathbf{x} \right\rVert` (bold the variable by default)
6. Do not bold scalar variables even in vector contexts.
   - Example: In $\mathbf{v} = c \mathbf{u}$, only $\mathbf{v}$ and $\mathbf{u}$ are bold; $c$ is not.

### Exceptions
- Vectors in quoted source code or user-authored examples should follow user convention.
- If the user explicitly asks to preserve raw notation, keep it as-is and provide canonical form separately.

## Precedence and Output Filter

Before sending any assistant-authored chat content:

1. **Scan for raw evan.sty tokens** matching the redirection list above.
2. **Replace each with its mapped canonical form.**
3. **Identify all vector variables** (apply default bold rule for ambiguous cases; do NOT ask).
4. **Ensure all vector variables are wrapped in `\mathbf{...}`.**
5. **Verify:** No raw evan.sty tokens remain in math output; no vectors lack bold formatting.

## Examples

**Input:** User writes "Considera il vettore \mathbf{v} con la norma \norm{v}."

**Output (normalized):** "Consideriamo il vettore $\mathbf{v}$ con la norma $\left\lVert \mathbf{v} \right\rVert$."
- `\norm{v}` → `\left\lVert \mathbf{v} \right\rVert` (both redirection and bold applied; $v$ bolded by default in vector context)
- Vector inside norm also bolded

**Input:** User writes "L'insieme dei naturali è $\NN$ con la funzione $\Th(n)$."

**Output (normalized):** "L'insieme dei naturali è $\mathbb{N}$ con la funzione $\operatorname{Th}(n)$."
- `\NN` → `\mathbb{N}`
- `\Th` → `\operatorname{Th}`
- $n$ is not bolded (scalar function argument)

**Input:** User writes "Il prodotto scalare tra i vettori $\mathbf{x}$ e $\mathbf{y}$."

**Output (normalized):** "Il prodotto scalare tra i vettori $\mathbf{x}$ e $\mathbf{y}$."
- Both variables already explicitly bolded; no change needed.
