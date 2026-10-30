# Ajaia — Collaborative Document Editor

A lightweight browser-based document workspace built for the Full Stack Product Engineer assignment. It supports rich-text editing, document organization, `.txt`/`.md` import, Markdown export, and a seeded-user sharing demonstration.

## Current implementation

- Create, rename, edit, and reopen documents.
- Rich text: bold, italic, underline, paragraph, headings, bullet lists, and numbered lists.
- Autosave to browser `localStorage` and restore after refresh.
- Import `.txt` and `.md` files into new editable documents (maximum 1 MB).
- Export document text as Markdown.
- Seeded demo accounts, owned/shared filters, share and revoke access.
- Basic validation, confirmation before deletion, and error toasts.
- Unit tests for access logic and safe text import.
- Optional Supabase sync hook for a configured `documents` table.

## Demo accounts

Use the account selector at the bottom of the sidebar to switch users:

- `dhaval@example.com` — owner of the welcome and product launch documents.
- `aarav@example.com` — owns the meeting notes and has access to the welcome document.
- `priya@example.com` — additional seeded teammate for sharing demonstrations.

This is **mock authentication** for a portfolio exercise, not a secure production login. The browser's local storage is shared by accounts in the same browser profile; the app simulates access in the UI.

## Requirements

- Node.js 20.19+ or 22.12+
- npm

## Run locally

```bash
npm install
npm run dev
```

Open the local URL Vite prints (usually `http://localhost:5173`).

## Test and build

```bash
npm test
npm run build
npm run preview
```

## File import

Use the floating **Import file** action. Only `.txt` and `.md` are supported; files larger than 1 MB are rejected. Imported text is escaped before being represented as HTML paragraphs. DOCX, PDF, and image OCR are not supported.

## Persistence and cloud setup

By default, this demo stores document records in browser `localStorage`. That makes refresh persistence work in the same browser profile, but it is not multi-device or multi-user server persistence. Sharing is a seeded-user simulation and is not a security boundary.

Optional Supabase setup:

1. Create a Supabase project.
2. In the SQL editor, create a table:

```sql
create table public.documents (
  id text primary key,
  title text not null,
  content text not null default '',
  owner_email text not null,
  shared_with text[] not null default '{}',
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);
```

3. Copy `.env.example` to `.env.local` and set `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`.
4. Restart the dev server.

**Security note:** the optional sync is a convenience hook, not production-ready access control. Do not store sensitive documents or expose a Supabase table publicly. Before using cloud sync with real users, implement Supabase Auth, row-level security policies, owner/share authorization, and server-side validation. The local browser storage remains the demo's immediate read source.

## Deploy

### Frontend-only demo
Deploy this Vite project to Vercel, Netlify, or another static host:
1. Push the source to your Git repository.
2. Import the repository into your host.
3. Build command: `npm run build`
4. Output directory: `dist`
5. Deploy and test the public URL.

No backend service is required for the default local-storage demo. Each browser/device gets its own demo data.

### Cloud persistence
Configure Supabase environment variables and complete authentication/RLS before treating the app as a true multi-user shared service. The current Supabase hook is intentionally documented as an integration starting point, not a secure hosted collaboration implementation.

## Known limitations

- No real login; demo account switcher is simulated.
- Local storage does not synchronize changes between different browsers or devices.
- The optional Supabase sync hook does not implement authenticated row-level security and is not suitable for production as-is.
- Sharing is simulated through seeded account emails.
- No real-time co-editing, comments, version history, DOCX/PDF import, or rich-text-preserving Markdown export.
- Markdown export currently exports readable text and a title, not all formatting.

## Project structure

```text
src/
  App.tsx                 Main workspace and editor experience
  main.tsx                React entry point
  styles.css              Responsive visual system
  types.ts                Document and demo-user types
  lib/
    storage.ts            Local persistence, access logic, safe text import
    storage.test.ts       Unit tests
    supabase.ts           Optional cloud client configuration
```
