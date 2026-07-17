# Prompts Used

## Round 1 — vague prompt

> Build me a settings form with validation.

Output accepted as-is, no review, saved as `ROUND1.html`.

## Round 2 — precise prompt

> Build a user settings form with fields: name, email, password, confirm
> password, and a notifications checkbox.
>
> Validation rules:
> - Name: required, trimmed, 2–50 characters, whitespace-only counts as
>   empty.
> - Email: required, no spaces, no consecutive dots, must have exactly one
>   `@`, domain must include a top-level domain of at least 2 letters
>   (reject things like `a@b` and `a@b.c`).
> - Password: required, minimum 8 characters, must contain at least one
>   letter and one digit.
> - Confirm password: required, must exactly match password.
>
> Accessibility requirements: every input must have a real
> `<label for="id">`, password fields must use `type="password"`, each
> field must show its own error in a `role="alert"` element wired via
> `aria-describedby`, and on invalid submit, focus must move to the first
> invalid field.
>
> Edge cases to explicitly handle and test: whitespace-only name,
> single-character name, name over 50 characters, missing `@`, single-letter
> TLD, email with a space, email with consecutive dots, password under 8
> characters, password with no digit, password with no letter, and
> mismatched confirm-password.
>
> Verification step (mandatory): write the validation logic first as pure
> functions with no DOM access, write tests covering every rule and edge
> case above, and run them. Do not consider this done until all tests pass.

Output saved as `ROUND2.html` only after the validation logic was written,
tested (10/10 sample cases + edge cases, see COMPARISON.md), and confirmed
passing.
