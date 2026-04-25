# ATS Resume Optimizer

An AI-powered, browser-based tool that optimizes your resume's **Professional Experience** section to match a Job Description — boosting ATS scores to 95%+.

## Live Demo

> Open `ats-optimizer.html` directly in your browser — no installation, no build step, no server needed.

---

## What It Does

| Step | Action |
|------|--------|
| 1 | Upload your **Resume** (PDF or DOCX) + paste/upload the **Job Description** |
| 2 | Extracts ~60 weighted ATS keywords from the JD using client-side NLP |
| 3 | Calculates your **baseline ATS score** (keyword match %) |
| 4 | Calls **Groq AI (Llama 3.3 70B)** to rewrite only the Professional Experience bullets with JD keywords |
| 5 | Shows **Before → After ATS score** + keyword hit/miss chips |
| 6 | Renders the optimized resume in your **exact original design** |
| 7 | Download as **PDF** |

### Key Rules (Hard Constraints)
- No layout or formatting changes
- No section additions or removals
- No bullet additions or removals
- Numbers, metrics, and `<span>` tags preserved exactly
- Only text tokens inside existing bullets are rewritten

---

## How to Use

### 1. Get a Free Groq API Key
1. Go to [console.groq.com/keys](https://console.groq.com/keys)
2. Sign up (free, no credit card needed)
3. Click **Create API Key** → copy the `gsk_…` key

### 2. Run the App
1. Open `ats-optimizer.html` in any modern browser
2. Paste your Groq API key
3. Upload your resume (PDF/DOCX) — or skip to use the built-in template
4. Paste the Job Description text (or upload PDF/image)
5. Click **Analyze & Optimize**
6. Download your optimized resume as PDF

---

## Technology Stack

All dependencies are loaded via CDN — **zero npm, zero build step**.

| Library | Version | Purpose |
|---------|---------|---------|
| [PDF.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | Parse resume/JD PDFs in the browser |
| [Mammoth.js](https://github.com/mwilliamson/mammoth.js) | 1.6.0 | Parse DOCX resume files |
| [html2pdf.js](https://github.com/eKoopmans/html2pdf.js) | 0.10.1 | Export optimized resume as PDF |
| [Groq API](https://groq.com) | — | Llama 3.3 70B for bullet rewriting (free tier) |

### Client-Side NLP (no API needed)
The keyword extraction runs entirely in the browser:
- Stopword filtering
- n-gram extraction (unigrams + bigrams)
- Pattern matching for 25+ ATS skill categories (Agile, CRM tools, KPIs, etc.)
- Weighted scoring (skills > action verbs)

---

## File Structure

```
ATS-Resume-Optimizer/
├── ats-optimizer.html    # Complete single-file app (HTML + CSS + JS)
└── README.md             # This file
```

Everything is self-contained in `ats-optimizer.html`. The resume template (Vartika Singh) is embedded as a JS string — edit the `RESUME_TEMPLATE_HTML()` function to swap in your own resume HTML.

---

## Customizing for Your Resume

To use your own resume instead of the built-in template:

1. Open `ats-optimizer.html` in a text editor
2. Find the `RESUME_TEMPLATE_HTML()` function (~line 780)
3. Replace the HTML string with your own resume markup
4. Also update `getResumeTemplateText()` (~line 760) with your resume's plain text for accurate baseline scoring

Alternatively, just **upload your resume PDF/DOCX** — the app will parse it automatically.

---

## ATS Scoring Method

```
Score = (JD keywords found in resume) / (total JD keywords) × 100
```

Keywords are weighted by category:
- **Skills & tools** — 2× weight (e.g., Salesforce, Agile, KPI)
- **Action phrases** — 1× weight (e.g., cross-functional, data-driven)
- **General terms** — 1× weight

Target score: **95%+**

---

## Privacy

- Your API key is stored only in your browser's memory for the session — never saved to disk or sent anywhere except `api.groq.com`
- Your resume and JD text never leave your browser (except the bullet text sent to Groq for optimization)

---

## Deployment

This is a static file — deploy anywhere:

```bash
# GitHub Pages
git init && git add . && git commit -m "Add ATS optimizer"
git remote add origin https://github.com/YOUR_USERNAME/ats-resume-optimizer.git
git push -u origin main
# Enable GitHub Pages in repo Settings → Pages → main branch
```

Your app will be live at `https://YOUR_USERNAME.github.io/ats-resume-optimizer/ats-optimizer.html`
