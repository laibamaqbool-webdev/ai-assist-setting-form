# Project rules (learned from the Round 1 vs Round 2 drill)

1. **Validation logic must be written as pure functions with no DOM
   access**, so it can be tested independently of the UI. Never inline
   validation checks directly inside a submit event handler without first
   separating the rules from the DOM code.

2. **Every form input must have a real `<label for="id">`**, error messages
   must be per-field (`id="<field>-error"`, `role="alert"`, wired via
   `aria-describedby`), and password fields must always use
   `type="password"`. This is a hard requirement, not a nice-to-have —
   Round 1 shipped without it and would fail a basic accessibility review.

3. **Never accept AI-written validation without testing it first**, and
   always add at least one test case for an edge case that was NOT in the
   original prompt/spec (e.g. consecutive dots in an email). The
   precise-prompt round still shipped a bug on the first pass; only
   verification caught it.

4. **Prompts for any form/validation feature must state exact field-by-field
   rules and explicit edge cases** (empty, whitespace-only, boundary
   lengths, malformed input) rather than a one-line request — vague prompts
   silently drop edge-case handling.

5. **On invalid submit, move focus to the first invalid field** — showing
   errors with no focus management makes the form unusable by
   keyboard-only users, even if the error text itself is correct.
