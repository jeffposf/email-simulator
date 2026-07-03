# Email Simulator — Rebuild Spec

**Output file:** `email-simulator.html` (standalone, no server, no dependencies)

---

## What it is

A self-contained Gmail UI simulation with a 2-way conversational email thread.
Everything driven by a visual config panel — no raw JSON editing.
Export produces a standalone HTML file (config panel stripped) for demo use.

---

## Layout

Split-screen while editing:
- Left: visual config panel (collapsible sections)
- Right: live Gmail preview (updates in real time)

Export button → downloads `email-simulator-demo.html` (preview only, no panel)

---

## Config panel sections

### 1. Language toggle
- FR / EN switch at the top
- Changes all Gmail UI chrome labels (Boîte de réception / Inbox, Répondre / Reply, etc.)

### 2. Sender identity
- From name
- From email
- Avatar (URL or initials fallback)

### 3. Recipient
- To name
- To email

### 4. Email header
- Subject line

### 5. Email body blocks (ordered list, drag ↑↓, add/remove)
Each block is one of:
- **Text** — mini rich text editor (bold, italic, underline, link, bullet list, numbered list, font size, text color)
- **Image** — URL input + alt text + optional caption
- **Image + text overlay** — URL + overlay text (position: top/center/bottom) + overlay color + opacity
- **Spacer** — height in px

Empty field = block not rendered.

### 6. CTAs (add/remove, reorder)
Each CTA:
- Label
- Background color
- Text color
- Action: `trigger-simulator` | `external-link` | `inactive`
- URL (shown only when action = external-link)

### 7. Signature
- On/Off toggle
- Name
- Title
- Company
- Optional logo URL

### 8. Conversation turns (add/remove)
Each turn:
- Role: `user` | `bot`
- Rich text body (same editor as email body text blocks)
- Bot turns show typing animation before appearing

---

## Rendering rules

- Any field left empty → corresponding element not rendered (no empty `<p>`, no blank lines)
- Signature off → no signature block in email or in bot replies
- No CTAs → CTA section not rendered
- Zero conversation turns → reply button not shown

---

## Export

- Exports a standalone HTML file
- Config panel removed from export
- Gmail preview is the full page
- All images inlined as-is (URL references kept — no base64 conversion)
- Conversation replay works identically to the editor preview

---

## Technical constraints (learned from prior simulator)

- `window.processNextMessage` must be on global scope (not inside a closure)
- All CTAs use `href="javascript:void(0)"` — never `href="#"`
- Conversation JSON built programmatically — never hand-edited when HTML is involved
- Inline onclick wraps calls in `try/catch`
- No external libraries — vanilla JS + CSS only

---

## Files

| Path | Role |
|---|---|
| `email-simulator/SPEC.md` | This document |
| `email-simulator/email-simulator.html` | The app (editor + preview) |

---

## Status

- [ ] Initial build
- [ ] FR/EN toggle working
- [ ] All body block types working
- [ ] CTA wiring working
- [ ] Conversation replay working
- [ ] Export working
- [ ] Tested with Cegos scenario (Marc Fontaine / Alstom / Webinar Grand Est)
