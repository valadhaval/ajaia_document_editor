# Architecture and Product Decisions

## Product slice

Ajaia is a focused document workspace rather than a Google Docs clone. The first version concentrates on the most visible end-to-end workflow: find or create a document, write and format it, import existing text, and share it with a teammate.

## Structure

- **React + TypeScript + Vite:** quick feedback loop, typed document state, straightforward static deployment.
- **Tiptap / ProseMirror:** accessible rich-text editing primitives without building a contenteditable engine from scratch.
- **Storage adapter (`src/lib/storage.ts`):** document model, local persistence, demo access checks, seeded data, and import conversion are kept separate from the view.
- **localStorage:** zero-cost default persistence so reviewers can immediately try the product without provisioning a service.
- **Optional Supabase client:** demonstrates a path to a hosted database, but it is not presented as secure multi-user collaboration until authentication and RLS are added.
- **Vitest:** tests access rules and safe text import, two behaviors that matter to the product workflow.

## Data model

A document has an ID, title, HTML content, owner email, list of shared-with email addresses, and created/updated timestamps. `canAccess` returns true for the owner or an invited account. The sidebar derives All, My, and Shared views from this record.

## Prioritization

1. **Editing first:** rich text and a clean writing surface are the product's core value.
2. **Persistence second:** local autosave makes the happy path testable without credentials or infrastructure.
3. **Sharing model:** seeded users let reviewers explore owned/shared distinctions and invitations.
4. **File workflow:** safe `.txt`/`.md` import avoids a large parsing dependency within the timebox.
5. **Small quality details:** empty states, responsive layout, validation, export, deletion confirmation, and tests make the slice feel complete.

## Explicit trade-offs

- Mock identity is not authentication. It is appropriate only for a demo and is labeled in the interface and docs.
- localStorage is per browser profile and does not provide server-side authorization or cross-device collaboration.
- The Supabase hook is an integration seam, not a claim of production-ready cloud persistence. Real deployment requires Supabase Auth, row-level security, schema validation, and tests against access policies.
- `.docx` parsing was intentionally excluded to keep import behavior small, transparent, and testable.
- Real-time collaboration and history were deprioritized in favor of a reliable basic editing workflow.

## Next 2–4 hours

1. Add Supabase Auth and secure row-level security for owner and shared users.
2. Add integration tests for database access policies and import error paths.
3. Improve Markdown import to parse headings and lists rather than treating each line as a paragraph.
4. Add autosave status states (saving/saved/error) and debounce content writes.
5. Run keyboard/screen-reader and mobile viewport checks.
