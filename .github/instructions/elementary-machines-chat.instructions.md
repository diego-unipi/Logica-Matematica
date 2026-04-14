---
description: "Use when user messages mention elementary machine commands like \\push, \\exec, \\rem, \\ttif, \\cons, \\car, \\cdr, \\nth, \\length, \\head, \\cat, \\step, \\pred in chat."
name: "Elementary Machines Chat Redirection"
applyTo: "**"
---
# Elementary Machines Chat Redirection

Hard rule for assistant-authored chat output.

## Platform Constraint
- User-authored chat messages are not rewritten in place by instructions.
- Redirection applies to assistant-authored output only.
- If user input contains raw elementary-machine tokens, assistant should include a normalized rewritten form in its reply.

## Mandatory Redirection
Always rewrite these input aliases in assistant-authored math output:
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
- `\dotminu` -> `\mathbin{\dot{-}}` (compatibility alias)

## Mandatory Normalized Reply
- If a user message contains any raw elementary-machine token, assistant must include a normalized rewritten form in the reply.
- The normalized form should appear explicitly in assistant output, not only be implied.
- This normalization requirement applies even when user-authored message text itself cannot be rewritten in place.

## Priority Rule
- If user input contains `\push`, assistant-authored math output must contain `\texttt{push}` and never raw `\push`.
- Preserve superscripts, subscripts, and arguments.
- Example: `\exec_{2,2} g` -> `\texttt{exec}_{2,2} g`.

## Output Filter
Before sending any assistant-authored content:
1. Scan for raw tokens `\cons|\car|\cdr|\inp|\outp|\push|\rem|\exec|\ttif|\nth|\length|\head|\cat|\step|\pred|\dotminu`.
2. Replace each with mapped `\texttt{...}` form.
3. For `\dotminu`, replace with `\mathbin{\dot{-}}`.
4. Verify no raw elementary-machine token remains.

## Allowed Exception
- Raw tokens are allowed only inside a fenced `tex` block when the user explicitly asks for verbatim quoting.
