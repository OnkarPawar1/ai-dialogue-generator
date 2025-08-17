# AI Dialogue & Image Generator (Client-Side)

Generate a short two-speaker podcast script (Woman ↔ Man), synthesize audio for each line with Google Cloud **Text-to-Speech**, and create a title image with **Vertex AI Imagen** — all from the browser.

---

## Table of Contents
- [Demo (GitHub Pages)](#demo-github-pages)
- [Features](#features)
- [Screenshots](#screenshots)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [How It Works](#how-it-works)

---

## Demo (GitHub Pages)

1) Push this repo (with `index.html`) to GitHub.  
2) In your repo: **Settings → Pages → Build and deployment**  
   - **Source:** “Deploy from branch”  
   - **Branch:** `main` (or `master`) / **Folder:** `/ (root)`  
3) Wait for the green **Pages** check.
4) Your site will be live at: https://<your-username>.github.io/<your-repo>/

---
## Features

- ✍️ **Script generation** with Gemini via Vertex AI (model selectable)
- 🔊 **Two voices** (Woman & Man) using Google Cloud Text-to-Speech
- 🎚️ **Speaking speed** control
- 🌐 **Multi-language voice menus** (e.g., English, Marathi, Hindi, etc.)
- 🖼️ **Title image generation** via Vertex AI Imagen (model + aspect ratio)
- ⬇️ **Download** final MP3 + generated script
- ⚡ **Zero build tools** — runs as a static page (perfect for GitHub Pages)

---

## Screenshots
<img width="742" height="825" alt="image" src="https://github.com/user-attachments/assets/cbb124e5-066a-45f8-9137-826d38d7433a" />
<img width="741" height="831" alt="image" src="https://github.com/user-attachments/assets/48038858-b62e-43a0-9644-9d266c4832f1" />
<img width="744" height="122" alt="image" src="https://github.com/user-attachments/assets/8cacc566-f41b-4d24-afdb-d09b895016d4" />


---

## Tech Stack

- **Frontend:** HTML, Tailwind (CDN), Vanilla JS
- **AI (Script):** Gemini via **Vertex AI** `generateContent`
- **TTS (Audio):** Google **Cloud Text-to-Speech** REST API
- **Images:** Vertex AI **Imagen** `predict`
- **Hosting:** GitHub Pages (static)

---

## Prerequisites

### 1) Google Cloud
- A Google Cloud **Project** with billing enabled
- Region used in this app: `us-central1`

### 2) Enable APIs
In **APIs & Services → Library**, enable:
- **Vertex AI API**
- **Cloud Text-to-Speech API**

### 3) Create a **Browser API Key** (for Text-to-Speech)
- **APIs & Services → Credentials → Create credentials → API key**
- **Restrict** the key:
  - **API restrictions:** Restrict to **Cloud Text-to-Speech API**
  - **Application restrictions:** HTTP referrers (add your GitHub Pages domain)

> This key is visible in the browser. Restrict it!

### 4) Get a **Vertex AI access token** (short-lived)
- Install the CLI: `gcloud`  
- Authenticate:  
  ```bash
  gcloud auth login
  gcloud config set project <YOUR_PROJECT_ID>
- Print token: gcloud auth print-access-token
- Paste this token into the app.


### How It Works

## 1. Prompt Builder
You type a topic & choose a target language.
The app builds a strict format prompt: short lines, Woman:/Man: prefixes.
## 2. Script Generation (Gemini via Vertex AI)
POST to:
https://us-central1-aiplatform.googleapis.com/v1/projects/<PROJECT_ID>/locations/us-central1/publishers/google/models/<MODEL_ID>:generateContent
Auth: Bearer <ACCESS_TOKEN> (short-lived)
## 3. Dialog Parsing
The app extracts lines by speaker and sanitizes them.

## 4. TTS For Each Line
POST to Cloud TTS:
https://texttospeech.googleapis.com/v1/text:synthesize?key=<API_KEY>
Returns base64 MP3 per line → converted to Blob.

## 5. Concatenation
Combines MP3 blobs into a single MP3 Blob (simple join).

## 6.Imagen (Optional)
POST to Vertex AI Imagen :predict with your prompt & aspect ratio.
Displays the returned base64 image.

  
