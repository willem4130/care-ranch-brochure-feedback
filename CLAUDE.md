# The Care Ranch — Brochure Project

## What this is
An evolving set of HTML brochure drafts for The Care Ranch, deployed via Cloudflare Pages. The client reviews and compares drafts through a `versions.html` index page that lists every version with a changelog. **v20 is the current active draft** (content + type treatment pass).

The booking form on v20 is wired to a real email pipeline: a Cloudflare Pages Function (`functions/api/book.js`) calls the Resend API on submit. Details in the "Booking form + email" section below.

Also contains the original v7 feedback and legacy .docx tooling from earlier rounds.

## Project structure
```
├── index.html                       # Styled v7 feedback page (GitHub Pages root)
├── versions.html                    # Version index — the client's entry point
├── brochure-v8.html                 # Rewrite + client-selected photos
├── brochure-v9.html                 # Content additions (network, nutrition, empathy line)
├── brochure-v10.html                # Full layout redesign
├── brochure-v11.html                # TCR brand font, scroll animations, PDF export
├── brochure-v12.html                # Inline SVG logo + word-by-word scroll reveal
├── brochure-v20.html                # Client feedback pass: kickers removed, hero tagline below hero, TCR pull-out scale, Arizona header removed (CURRENT)
├── images/                          # Photos extracted from client PDFs
│   ├── location-51.jpg              # Pool → Experience section
│   ├── location-92.jpg              # Entrance gate → Hero background
│   ├── retreat-38.jpg               # Scott — inner reflection → Method band MIDDLE
│   ├── retreat-66.png               # Equine therapy → Method band RIGHT (horses)
│   ├── retreat-77.png               # Lotte — ramada session → Method band LEFT (cropped via object-position: 50% 100%)
│   ├── retreat-85.png               # Walking in trees → Journey banner
│   ├── retreat-108.png              # Food → Arizona section
│   └── retreat-118.png              # Team group photo → Team section
├── client-input/
│   ├── brochure-v7-original.docx    # Original brochure from the client
│   └── v9/                          # Screenshots + transcription for v9 input
├── feedback/
│   └── brochure-v7-feedback.md      # Feedback on v7 (markdown)
├── output/
│   └── brochure-v8-improved.docx    # Legacy — only used for v8 round
├── scripts/
│   └── generate_brochure_v8.py      # Legacy — only used for v8 round
├── functions/
│   └── api/
│       └── book.js                  # Cloudflare Pages Function: POST /api/book → Resend
├── .references/
│   └── voice.md                     # WB-copywriter voice profile derived from v20 + CLAUDE.md rules
├── content/
│   └── email-booking-notification-*.md  # Drafted HTML + text email templates (not yet wired)
├── package.json                     # wrangler devDep
├── package-lock.json
├── .gitignore
└── CLAUDE.md
```

## Version workflow

Each iteration creates a **new file in `main`**, not a new branch:

1. Copy the previous version: `cp brochure-vN.html brochure-v(N+1).html`
2. Edit the new file.
3. Add a new `<a class="version">` block at the top of `versions.html` with a "What changed" bullet list.
4. Commit + push. Cloudflare Pages auto-deploys in ~30–60 sec.

Every version gets its own permanent URL. The client bookmarks `versions.html` once and can click into any version at any time. Old URLs never expire — critical for the "she might want to revisit v8 next week" use case.

Branches are optional; use them for experimental work, but sharing with the client always goes through the main-branch versioned file.

## Content rules

- **When layout changes, preserve text byte-for-byte** (e.g. v9 → v10 was pure layout, zero copy edits). The client's previous copy approvals carry forward.
- **Never filter, recolor, crop, or harmonize client photos.** Client's explicit rule. CSS `object-position` for display cropping is OK as a narrow exception when explicitly requested (used for `retreat-77` to show the ramada/ground). In v20 the selector is URL-based (`img[src*="retreat-77"]`) so the crop travels with the file regardless of DOM order.
- **No em-dashes (`—` / `&mdash;`).** Client explicitly flagged them as "too AI". Use commas, colons, semicolons, or parentheses per context. This rule applies to all new copy and any edits — do not reintroduce them.

## Animations (v20 policy)
- GSAP + ScrollTrigger via CDN (cdnjs, `defer`) drive: section fade-ups, staggered grid reveals, hero parallax, word-by-word reveal on h2s and `[data-split]` poetic lines.
- **FOUC prevention** hides certain selectors at `opacity: 0` until GSAP animates them in. The gotcha: on slow connections, `window.load` waits for the GSAP CDN, so gated elements stay invisible for 2–3 seconds. That looked broken on the client's connection.
- **Above-the-fold / first-visible sections must NOT be FOUC-gated.** In v20 this means `.arizona-narrative`, `.split-text`, and `.split-media` are explicitly excluded from both the FOUC opacity rule AND the GSAP single-element reveal list. They render immediately with the HTML. Animated reveals kick in from Clean Vision downward.
- When adding a new early section to the page, do not add it to the FOUC list and do not add a ScrollTrigger for it. Keep the rest of the animations as-is.
- **Photos** are extracted via `pdfimages -all` from the PDFs in `client-input/Pictures/` (gitignored — the PDFs are ~155 MB combined).

## Deployment
- **Repo:** https://github.com/willem4130/thecareranch-brochure
- **Current draft:** https://thecareranch-brochure.pages.dev/brochure-v20.html
- **Versions index:** https://thecareranch-brochure.pages.dev/versions.html
- Commit to `main` → Cloudflare Pages auto-deploys in ~30–60 sec.
- The old GitHub Pages URLs (`willem4130.github.io/care-ranch-brochure-feedback/*`) are no longer served. GitHub does not redirect Pages URLs after a repo rename, and we intentionally moved off GitHub Pages when the booking form was wired to a Cloudflare Pages Function.

### CLI access
Wrangler is a local devDep (`package.json`). Use `npx wrangler ...` for everything. Logged in as `willem@scex.nl` via `wrangler login` (OAuth). Verify with `npx wrangler whoami`.

Useful commands:
- `npx wrangler pages deployment list --project-name=thecareranch-brochure` — recent deployments
- `npx wrangler pages secret list --project-name=thecareranch-brochure` — list configured secrets (names only, values stay encrypted)
- `npx wrangler pages secret put <KEY> --project-name=thecareranch-brochure` — add or update a secret (interactive; paste value at prompt, do NOT put it on the command line)

GitHub repo operations use `gh` (authenticated as `willem4130`).

## Booking form + email

The "Book now" button opens a modal form. On submit, the form POSTs JSON to `/api/book`, handled by `functions/api/book.js` (a Cloudflare Pages Function). That function validates input and calls the Resend API to send a notification email.

**Flow:** browser form → `fetch` POST `/api/book` → `functions/api/book.js` → Resend API → email lands.

**Env var:** `RESEND_API_KEY` is stored as a Cloudflare Pages secret (encrypted, never in the repo). Set or rotate via `npx wrangler pages secret put RESEND_API_KEY --project-name=thecareranch-brochure` (interactive; paste at prompt). The function reads it as `env.RESEND_API_KEY`. Pages Functions need a fresh deployment after a new secret is set, so follow any `secret put` with an empty commit + push to retrigger a build.

**Current sender:** `onboarding@resend.dev` (Resend's shared sandbox sender). `reply_to` is set to the booker's email so a reply goes back to the guest directly from whoever opens the notification.

**Current recipients:** `willem@scex.nl` only. Resend's sandbox blocks unverified accounts from sending to any address other than the account owner's. `contact@thecareranch.com` is intentionally NOT in the recipient list until the domain is verified (see Pending). The email `to` list lives in the `RECIPIENTS` constant at the top of `functions/api/book.js`.

**Success / error UX:** On successful POST, the modal swaps to a "Thank you, {name}. We've received your booking request..." state. On error (network failure or non-2xx from the function), the modal shows an inline message with a fallback mailto to `contact@thecareranch.com`. State resets on modal close so a reopen is always fresh.

**Drafted email templates:** `content/email-booking-notification-2026-04-14.md` contains two brand-voiced HTML + plain-text templates (Concierge slip + Landed letter) generated by `/WB-copywriter`. Both use the Care Ranch palette via inline styles with table-based layout for Outlook compatibility. They are NOT wired yet: the Concierge slip would replace the current plain-text internal notification; the Landed letter is the booker-facing confirmation, which requires domain verification before it can be sent (see Pending). Once unblocked, swap the `text:` field in the Resend call for `html:` plus `text:` and add the template substitutions.

**Public contact address in the brochure:** `contact@thecareranch.com` is linked inline at the CTA's `.cta-fine` line (centered block, Arial, terracotta underline) and in the new `<footer>` (`.footer-contact`). Same address appears as the `reply_to` on notification emails, so pre-booking questions, post-booking replies, and the "if something went wrong" fallback all converge on one inbox.

## .docx generation (legacy — v8 only)
The `scripts/generate_brochure_v8.py` script was used in the v8 round to produce a Word doc alongside the HTML. Not used for v9+. Kept for reference.

```bash
python3 scripts/generate_brochure_v8.py
```
Requires `python-docx`. Loads `client-input/brochure-v7-original.docx` as a template to inherit theme fonts (Calibri headings, Cambria body). Always `Document(TEMPLATE)`, never `Document()` (the latter creates Word's default blue-serif theme).

The original (Google Docs export) stores formatting as paragraph- and run-level overrides:
- **H1:** run-level 23pt bold, paragraph space_before=24pt
- **H2:** run-level 17pt bold, paragraph space_after=4pt
- **H3:** run-level 13pt bold, paragraph space_before=14pt
- **Body:** paragraph space_after=12pt
- **No bullet styles** — list items are plain 'normal' paragraphs

## Context
- Brochure was written in Dutch first, then translated/composed into English
- v8 incorporated all feedback from `feedback/brochure-v7-feedback.md`
- v8 → v9: content additions — international network/Wageningen, whole food nutrition as Method pillar, empathetic opener, outcome bullets, explicit expert language, target-groups note
- v9 → v10: pure layout redesign — full-bleed hero, 2-column splits, 2×3 method grid, 3-column team cards, 3-step journey timeline, 2-column benefit + info grids
- Reference project for brand/design: `/Users/willemvandenberg/Dev/The Care Ranch/thecareranch-landingpage`
- **Brand colors:** sand `#F7F4F0`, cream `#F2EDE4`, terracotta `#C47D5C`, saddle `#79584A`, charcoal `#3D3632`
- **Brochure fonts:** custom `@font-face` "The Care Ranch" (display + pull-outs) + Arial (body). Loaded from `Fonts/TheCareRanch0525-RegularNEW.ttf|otf`.
  - **Shorthand: `TCR`** — always refer to "The Care Ranch" font as TCR throughout code, commits, and conversation.
  - **TCR pull-out size (canonical, v20+):** `clamp(1.4rem, 2.2vw, 1.85rem)` — ~22–30px. This applies to EVERY TCR italic pull-out across the brochure: section openers below an h2 (`.poetic`), inline quotes (`.inline-quote p`), hero tagline (`.hero-tagline`), interstitial pull-outs (`.shift-block`), CTA quote (`.cta-quote`), and statement list items (`.statement-list li`). Consistency across these is required — a brochure reads as one document, so if you change one, change them all. Do NOT introduce a new size without asking.
- **Landing page fonts (reference project only):** Playfair Display + Lora via Google Fonts

## Pending (as of v20)
- Individual team portraits: placeholder tiles gone in v20; real portraits still awaited
- Tara-removal AI edit on the team group photo (`retreat-118`)
- **Resend domain verification for `thecareranch.com`**: unlocks sending to `contact@thecareranch.com` and to arbitrary booker addresses. Requires 3 DNS records on the client's domain (SPF + DKIM + DMARC). Once verified: (a) add `contact@thecareranch.com` back to the `RECIPIENTS` list in `functions/api/book.js`, (b) wire the "Landed letter" template from `content/email-booking-notification-*.md` as a second Resend call sent to the booker's email as a confirmation, (c) swap sender from `onboarding@resend.dev` to a branded address like `bookings@thecareranch.com`.
- Fonts: the original focus of the session that spawned this whole Cloudflare/Resend deployment was to fine-tune the brochure typography. That work is still untouched.
