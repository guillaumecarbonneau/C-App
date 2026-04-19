# Camille — Lernhelfer (Study Helper)

A mobile-first, installable web app for parents and a 13-year-old in German secondary school.
Upload photos of textbooks or worksheets (German or English); get bilingual parent summaries, German cue cards, written-answer exercises, and an MCQ quiz whose answer key is hidden from the student. Includes a parent-managed points and rewards system, plus one-tap WhatsApp sharing.

Everything runs in the browser. Photos go to the Anthropic Claude API only when you tap **„Lernmaterial erstellen"**. All data (API key, PIN, sessions, points) is stored locally in `localStorage` on the device.

## Files

- `index.html` — the full app (UI + logic, single file)
- `manifest.webmanifest` — PWA manifest (name, icons, theme color)
- `sw.js` — service worker for offline / home-screen install
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — icons
- `camille-app.zip` — everything above zipped up, ready to drag onto a static host

## Get it onto your iPhone

You need to serve the folder over HTTPS (or `http://localhost`) because the service worker and camera only work in secure contexts.

### Easiest path: free static host

1. Go to **Netlify Drop** (<https://app.netlify.com/drop>), **tiiny.host**, **Cloudflare Pages → Direct upload**, or **Vercel**.
2. Drag `camille-app.zip` onto the upload area (zips upload more reliably than folders).
3. You'll get a URL like `https://camille-xyz.netlify.app`.
4. On your iPhone: open the URL in **Safari** → tap the Share button → **Add to Home Screen** → name it "Camille".
5. Launch from the home-screen icon. It runs full-screen like a native app.

### Local alternative (terminal)

From the project folder on your Mac:

```bash
cd "Camille App"
npx surge .
```

Surge deploys the folder in seconds and prints a free HTTPS URL.

## First run

On first launch the app asks for:

- **Child's name**
- **Parent PIN** (4–6 digits — locks the Eltern/parent mode)
- **Claude API key** (`sk-ant-...`) — get one at <https://console.anthropic.com> → **API Keys**

The key is stored only on the device, sent only to `api.anthropic.com` when you process photos. You can change or remove it later in Einstellungen (Settings).

## How it works

1. **Eltern-Modus (Parent mode)**: tap "Neue Lernrunde starten" → "📷 Kamera" (or "🖼️ Galerie") → take/pick multiple photos of textbook pages, worksheets, or learning objectives → tap **„Lernmaterial erstellen"**.
2. Claude Vision reads the photos (German or English) and returns a structured learning pack: title, subject, learning goals, bilingual parent summaries (EN + DE), 8–12 German cue cards, 5–7 open-answer questions with model answers, and 8 MCQ questions with answer key + explanations.
3. **Schüler-Modus (Student mode)** is unprotected and shows cue cards, written exercises, and MCQ quiz **without** the answer key. Submitting the MCQ earns +1 point per correct answer.
4. **Parent mode** sees everything — including the answer key and the student's written answers — and can award additional points manually.
5. **Belohnungen (Rewards)** are created by the parent (name + point cost). The student can request a redemption, which deducts points.
6. **WhatsApp** buttons open `wa.me` with prefilled text (German summary, English summary, both, cue cards, questions, quiz without answers).

## Privacy

- No server. No login. No analytics.
- Photos are uploaded to Anthropic only when you explicitly tap "Lernmaterial erstellen".
- Clearing browser data or tapping "Alle Daten löschen" in Einstellungen removes everything.

## Notes & limits

- 4–8 photos per session works well.
- Student mode has no PIN by design. The answer key UI is gated behind the parent PIN.
- Cost per session with Claude Sonnet is typically a few cents. Switch to Haiku in Settings for the cheapest runs, or Opus for the best quality.
