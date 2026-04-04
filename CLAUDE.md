# The Care Ranch — Brochure Project

## What this is
A project containing feedback on the client's brochure (v7) and the improved rewrite (v8), plus the tooling to generate it.

## Project structure
```
├── index.html                       # Styled feedback page (GitHub Pages root)
├── brochure-v8.html                 # Improved brochure as styled HTML (GitHub Pages)
├── client-input/
│   └── brochure-v7-original.docx    # Original brochure from the client
├── feedback/
│   └── brochure-v7-feedback.md      # Our feedback on v7 (markdown)
├── output/
│   └── brochure-v8-improved.docx    # Improved brochure (generated)
├── scripts/
│   └── generate_brochure_v8.py      # python-docx script to build the .docx
└── CLAUDE.md
```

## How to regenerate the brochure
```bash
python3 scripts/generate_brochure_v8.py
```
Requires `python-docx` (`pip install python-docx`). Output lands in `output/`.

## Styling approach
The generation script loads `client-input/brochure-v7-original.docx` as a template, clears its content, then adds the v8 text. This inherits theme fonts (Calibri headings, Cambria body) and style definitions. Do NOT use `Document()` (creates Word's default blue-serif theme). Always use `Document(TEMPLATE)`.

**Important:** The original (Google Docs export) stores formatting as paragraph-level and run-level overrides, not in the style definitions. The script must apply these explicitly:
- **H1:** run-level 23pt bold, paragraph space_before=24pt
- **H2:** run-level 17pt bold, paragraph space_after=4pt
- **H3:** run-level 13pt bold, paragraph space_before=14pt
- **Body ('normal'):** paragraph space_after=12pt
- **Method/benefit items:** bold prefix + `\n` + body text in one paragraph
- **No bullet styles** — list items are plain 'normal' paragraphs

## Deployment (feedback page)
- **Repo:** https://github.com/willem4130/care-ranch-brochure-feedback
- **Live URL:** https://willem4130.github.io/care-ranch-brochure-feedback/
- To update: edit `index.html`, commit, and push. GitHub Pages rebuilds automatically.

## Context
- The brochure was written in Dutch first, then translated/composed into English
- v8 incorporates all feedback from `feedback/brochure-v7-feedback.md`
- Key v8 changes: restructured sections (For Whom + Team moved earlier), consistent voice/POV, fixed Dutch-to-English translation points, EBAS acronym moved to Scott's bio, Theory U reference removed, non-profit section bridged, stronger CTA
- Reference project for brand/design: `/Users/willemvandenberg/Dev/The Care Ranch/thecareranch-landingpage`
- Brand colors: sand `#F7F4F0`, terracotta `#C47D5C`, charcoal `#3D3632`, cream `#F2EDE4`
- Brand fonts: Playfair Display (display), Lora (body) via Google Fonts
- Current .docx uses Calibri/Cambria (matching original v7 styling); branded version may follow later
