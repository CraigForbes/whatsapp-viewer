# WhatsApp Chat Viewer

A single self-contained HTML page for browsing a **WhatsApp chat export** (the `.zip` you get from *Export chat → With media*) — read the conversation, view every photo/video/voice note/link, and **organise media into your own categories**.

🔗 **Live:** https://craigforbes.github.io/whatsapp-viewer/

> **Your data never leaves your device.** The zip is unpacked and read entirely inside your own browser — nothing is uploaded to any server. The hosted page is just the viewer; it contains no chat data.

## Features

- **Browse the chat** in a familiar WhatsApp-style layout, with date separators, per-sender colours, and a live search box.
- **Inline media** — images, videos and voice notes play directly in the conversation; click an image/video for a full-screen lightbox.
- **Link detection** — URLs are picked out automatically, with icons/labels for YouTube, Facebook, Instagram, TikTok, X and Spotify (YouTube links get a thumbnail).
- **Media & Links gallery** — every attachment and link in one grid, filterable by type (Images / Video / Audio / Links / Files).
- **Your own categories** — create, rename-by-recreating, and delete categories on the fly; tag any item; filter the gallery to a single category (plus built-in *All* and *Uncategorised* views with live counts). Jump from any gallery item back to its place in the chat.
- **Export a category as a web gallery** — save the current gallery view as a standalone HTML file with all media embedded, viewable anywhere without the original zip.
- **Backup / restore tags** — export your categories + tag assignments to a JSON file and load them back later (handy for moving tags between your PC and phone).

## How to use

1. On your phone, open the chat in WhatsApp → **⋮ / chat name → Export chat → With media**, and save/share the `.zip` to wherever you'll open the page.
2. Open the [viewer](https://craigforbes.github.io/whatsapp-viewer/) (or the local `index.html`).
3. **Drag the `.zip` onto the page**, or click to choose it.
4. Use the **Chat** and **Media & Links** tabs to browse, tag, and export.

## How it works

It's plain HTML/CSS/JavaScript in **one file — no build step, no dependencies, no server**:

- The `.zip` is read with the browser's `FileReader`, and unpacked by a small **built-in ZIP parser** that uses the native [`DecompressionStream`](https://developer.mozilla.org/docs/Web/API/DecompressionStream) API for deflate — so no external libraries are needed.
- `_chat.txt` is parsed with a flexible matcher that handles **both iPhone and Android** export formats (bracketed `[date, time]` vs `date, time -`, AM/PM, seconds, system messages, multi-line messages).
- Media files are matched to their messages by filename and shown via in-memory blob URLs (created lazily as you scroll, to keep memory low).
- Categories and tags are stored in the browser's `localStorage`, scoped per chat (keyed by the chat file name + size).

## Limitations / notes

- **HEIC photos** (default iPhone format) may not display in Chrome/Firefox — they appear as a file card you can still tag/open. Safari handles them. (WhatsApp usually converts photos to JPG on export anyway.)
- **Link/YouTube thumbnails** need an internet connection; everything else works fully offline.
- **"Me" alignment** (right-side green bubbles) is a best guess — the most frequent sender — since the export doesn't record which device it came from.
- Tags live in `localStorage`, so they're **per-browser**. Use **Backup/Restore tags** to carry them between devices.
- Requires a modern browser (needs `DecompressionStream`: recent Chrome, Edge, Firefox, or Safari).

## Privacy

Everything runs locally in your browser. No analytics, no network calls except optionally loading link thumbnails. The GitHub repository is public, which means the **viewer code** is visible to anyone — but your chat export is only ever read on your own machine and is never transmitted or stored anywhere.
