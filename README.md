# Multiple Choice Question Bot (Gemini, Deno)

Minimal bot that helps student to practice for exams by generating high quality multiple choice questions based on the lectures provided. It uses Gemini 2.5 flash and can optionally log to Qualtrics.

## Features
- Can upload many lecture contents and they will be automatically included as options
- Allows user to select the specific lecture to generate question from
- Optionally logs `{lectureName, responseText}` to Qualtrics
- Works as a standalone web page or Brightspace embed

## 1. Create your copy
- Fork this repository or use this template on GitHub (e.g., `syllabus-bot-3210`, `paragraph-marker`)

## 2. Replace lecture content
- Remove the existing lectures and put your lecture content in `1026_midterm_transcripts` folder
- ***Please do not change the folder names, only edit the content.

## 3. Deploy backend to Deno
- Sign in at https://dash.deno.com → **+ New Project** → **Add Github Account** → **Install Deno** -> Click linked account
- Select your repo
- Production branch: `main`
- Entry point: `main.ts`
- Create the project (you'll get a `https://<name>.deno.dev` URL)

## 4. Add environment variables
In **Deno → Settings → Environment Variables**, add:

    GEMINI_API_KEY=your Gemini API key
    QUALTRICS_API_TOKEN=(optional)
    QUALTRICS_SURVEY_ID=(optional)
    QUALTRICS_DATACENTER=(optional, e.g., uwo.eu)
    GEMINI_MODEL=(optional, default gemini-2.5-flash)

Qualtrics variables can be accessed through: 
- **Qualtrics -> Creat New Project -> Survey Flow** -> Add Two **Embedded Data (repsonseText, lectureName) -> Apply**
- Go back to homepage and activate the survey
- Get SURVEY_ID from URL
- Generate API_TOKEN from user settings
- QUALTRICS_DATACENTER can be found in the first part of survey url, ex. 'uwo.eu'

## 5. Point the frontend to your backend
In `index.html`, replace the fetch URL with your Deno URL, e.g.:

    fetch("https://your-app-name.deno.dev/", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ query: userQuery })
    });

## 6. Host the frontend (GitHub Pages)
- Repo → **Settings → Pages**
- Branch: `main`, Folder: `/ (root)` → **Save**
- Use the published URL (e.g., `https://yourusername.github.io/yourbot/`)
- For Brightspace, you can also paste `brightspace.html` as a content item or widget

## 8. Adjust input/output token limit
- Responses are capped at **10000 tokens**; Change the output token limit in `main.ts` line 110.

## Notes
- CORS headers are returned by `main.ts`, so the Brightspace iframe can call your backend.
- Each deployment has its own backend; ensure the frontend fetch URL matches the correct Deno project.
- The gemini free version is typically limited to a 32,000-token input context window, paid tier can go up to 1 million token window.
- Here is the link to all Gemini models and pricing: https://ai.google.dev/gemini-api/docs/pricing#gemini-3-pro-preview.

## Qualtrics (optional)
- In your survey, add embedded data fields: `responseText`, `lectureName`.
- The response includes an HTML comment like `<!-- Qualtrics status: 200 -->` to confirm logging.

## Files
- `index.html` — public interface
- `brightspace.html` — LMS-friendly wrapper
- `main.ts` — Deno backend (OpenAI API)
- `README.md` — this file

## License
© Anny Zheng
