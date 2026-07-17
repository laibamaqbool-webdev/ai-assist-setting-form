# COMPARISON.md — Round 1 vs Round 2

Same 8 test inputs run against both forms.

| Input | Round 1 (`ROUND1.html`) | Round 2 (`ROUND2.html`) |
|---|---|---|
| All fields empty | "Please fill all fields." | Rejected — all 4 fields show errors |
| Invalid email `"asdf"` | **"Settings saved!"** ❌ | Rejected — "Email must contain exactly one @." |
| Malformed email `"a@b.c"` | **"Settings saved!"** ❌ | Rejected — "Email top-level domain must be at least 2 letters." |
| Weak password `"a"` | **"Settings saved!"** ❌ | Rejected — "Password must be at least 8 characters." |
| 1-character name `"L"` | **"Settings saved!"** ❌ | Rejected — "Name must be between 2 and 50 characters." |
| Whitespace-only name `"   "` | **"Settings saved!"** ❌ | Rejected — "Name is required." |
| Consecutive dots `"a..b@example.com"` | **"Settings saved!"** ❌ | Rejected — "Email cannot contain consecutive dots." |
| Password ≠ confirm | "Passwords do not match." | Rejected — "Passwords do not match." |
| Fully valid input | "Settings saved!" | "Settings saved!" |

## Summary

| Category | Round 1 | Round 2 |
|---|---|---|
| Validation rules | 2 (empty check, password match) | 4 fields, 10+ specific rules |
| Email format checked | No | Yes (structure, spaces, consecutive dots, TLD length) |
| Password strength checked | No | Yes (length, letter, digit) |
| Accessibility | `<br>` labels, `type="text"` passwords, one shared error message | Real `<label for>`, `type="password"`, per-field `role="alert"` + `aria-describedby`, focus on first invalid field |
| Automated verification | None | 10/10 sample cases confirmed passing |
| Review effort | Looked done in ~1 min, actually needs a full rewrite | Took longer upfront, only one gap found afterward |

## The AI mistake caught

Even in Round 2's "precise" pass, the first draft of the email check
treated any string containing `@` as valid — so `a..b@example.com` slipped
through. This was only caught by testing edge cases beyond what was
explicitly in the prompt, then fixed by adding the consecutive-dots check.
This is the core lesson of the drill: a good prompt reduces mistakes, it
does not remove the need to verify the output.
