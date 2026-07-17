# WORKFLOW.md — Vague Prompt vs. Precise Prompt

Feature: a user settings form (name, email, password, confirm password,
notification toggle) with validation. Built twice with the same AI:
`ROUND1.html` and `ROUND2.html`. Full prompts in `PROMPT_USED.md`.

## Round 1: single vague prompt

Prompt: *"Build me a settings form with validation."* Output was accepted
with no review and saved as-is.

## Round 2: precise prompt + verification loop

Prompt included field-by-field validation rules, explicit edge cases
(whitespace-only name, single-letter TLD, consecutive dots in email,
password missing a digit or letter), an accessibility requirement, and a
mandatory step: write the validation logic as pure functions, write test
cases, and run them before calling it done.

## Correctness

Round 1's whole check is `if (name == "" || email == "" || password == "")`
plus a password/confirm equality check. It never validates email format, so
`"asdf"` passes as valid, and a 1-character password passes as long as it
matches confirm. No tests exist, so none of this shows up until someone
manually pokes it.

Round 2's validation is a set of pure functions: real email structure
checks (rejects missing `@`, single-letter TLDs like `a@b.c`, spaces,
consecutive dots), an 8-character password rule requiring a letter and a
digit, and a 2–50 character trimmed-name rule. Confirmed against 10 sample
cases plus edge cases, all passing. Writing and running the checks caught
one real mistake in the first draft: the initial email check treated any
string containing `@` as valid, so `a..b@example.com` and `a@b` both
passed. That's the AI mistake this drill was meant to surface — precision
reduced the mistake count, but the verification step, not the prompt
itself, is what caught it.

## Accessibility

Round 1: labels are plain text separated by `<br>`, not `<label for>`, so a
screen reader can't associate them with their inputs. Both password fields
use `type="text"`, so passwords render in plain view. One shared `#msg` div
handles every error, giving no field-specific information.

Round 2: every input has a real `<label for>`, both password fields use
`type="password"`, each field has its own `role="alert"` span wired through
`aria-describedby`, and on an invalid submit, focus moves programmatically
to the first invalid field — usable end-to-end with a keyboard.

## Edge cases

Round 1 handles exactly two conditions: empty fields and password mismatch.
It silently accepts a 1-character name, a fake email, and a weak password.

Round 2 explicitly rejects: whitespace-only names, names outside 2–50
characters, malformed emails (`a@b`, `a@b.c`), emails with spaces or
consecutive dots, passwords under 8 characters or missing a digit/letter,
and mismatched confirmation.

## Review effort

Round 1 took under a minute to accept but needs a full rewrite before it's
usable — the review effort was deferred, not avoided. Round 2 took longer
upfront (writing the spec, checking the logic), but the after-the-fact
review was small: one bug (consecutive dots) outside the original spec,
caught by the verification step. Net time was comparable; the difference is
where the debugging happens — before commit vs. after a user reports it.
