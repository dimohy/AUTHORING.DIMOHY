# AUTHORING.DIMOHY History

## r29 — 2026-04-28

### Major Features

- Issue-only upstream improvement flow — External users can submit reusable AUTHORING improvements through Issue creation or Issue draft preparation.
- Maintainer Issue triage workflow — Maintainers can classify registered Issues, evaluate general usefulness, and apply accepted changes consistently.
- Revision history documentation — Bilingual release history now records user-visible changes whenever the revision increases.

### Major Improvements

- Revision reference minimization — User-facing guidance now prefers language directories and generic revision selectors to reduce release churn.
- English/Korean guide synchronization — Both localized guides and entry documents now describe the same upstream improvement choices and maintainer loop.
- English-only root entry point — The root entry document now avoids non-English prompt examples and keeps localized examples in the Korean guide.

### Major Bug Fixes

- TaskSync session stability — Session reuse, `auto` handling, and numbered-choice fallback guidance were clarified to prevent session drift and invisible options.
- Stale upstream submission wording — Outdated alternate-submission language was removed so reusable improvements consistently route through Issues.
