# AI-Native Workflow Note

## Tools used

- OpenAI ChatGPT: scope planning, project structure, documentation drafting, and review checklist.
- [Add any other AI coding tools actually used before submission.]

## Where AI accelerated the work

AI assistance helped break the broad prompt into a smaller deliverable: a rich-text editing surface, local persistence, seeded sharing, file import, tests, and concise architecture documentation. It also helped identify risky claims that should not be made about mock authentication or optional cloud sync.

## Output reviewed or constrained

The implementation deliberately does not claim to provide secure authentication, production-grade access control, or real-time collaboration. The Supabase client is an optional integration hook; the default experience is local browser storage. `.docx` import and formatting-perfect Markdown export were left out rather than adding unverified behavior.

## Verification performed

Before submitting, run these checks and update this note with the actual outcome:

```bash
npm install
npm test
npm run build
npm run dev
```

Manual review checklist:
- Create a document, rename it, format text, refresh, and reopen it.
- Import a `.txt` and `.md` file; verify unsupported extensions and >1 MB files are rejected.
- Share with a seeded account, switch account, verify the document appears in Shared with me, and revoke access.
- Confirm empty search results, delete confirmation, and mobile navigation.
- Test the public deployment in a private/incognito window.

## Human judgment

AI-generated code is a starting point, not evidence of correctness. The candidate should run the tests and build, inspect the code, test the main flows in a browser, and revise this note to state only checks actually completed. Do not claim checks passed unless they were run successfully.
