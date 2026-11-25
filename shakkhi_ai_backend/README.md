# SHAKKKHI optional AI helper — local hackathon prototype

This adds an optional ChatGPT-like explanation panel to SHAKKKHI. It does **not** replace local keyword checks. It uses OpenAI's Responses API through a small FastAPI server; the API key is kept on the server, never in the browser.

## Privacy / consent behavior
- AI helper is opt-in and asks for a checkbox consent before sending.
- The browser masks email addresses, phone numbers, and URLs before sending.
- Masking is best-effort; names, usernames, or other identifiers may remain. Review text before consenting.
- Only text and the optional question are sent; screenshots/images are not sent to this AI endpoint.
- This backend has no database and does not write request text to disk. It has a small in-memory request rate limit. The API provider processes the request, so external processing still occurs.
- Do not test with real sensitive information. Use fictional sample messages.
- API usage can incur billing. A parent/guardian or adult team lead should handle account setup, API key, and spending limits.

## Run locally

1. Install Python 3.11+.
2. From this folder:
   ```bash
   python -m venv .venv
   # Windows PowerShell:
   .venv\Scripts\Activate.ps1
   # macOS/Linux:
   # source .venv/bin/activate
   pip install -r requirements.txt
   ```
3. Set the API key as an environment variable (never paste it into HTML):
   - PowerShell: `$env:OPENAI_API_KEY="your-key"`
   - macOS/Linux: `export OPENAI_API_KEY="your-key"`
   Optional model setting: `OPENAI_MODEL` (default `gpt-6-luna`). Check model availability in your API account.
4. Start:
   ```bash
   uvicorn app:app --host 127.0.0.1 --port 8000
   ```
5. Open `http://127.0.0.1:8000`. Check `/health` for server status.
6. In Evidence, run local **Review patterns**, then choose **AI explanation & next steps**. Read the consent, check it, and optionally ask a follow-up.

## API
`POST /api/assistant` JSON:
```json
{"text":"masked text","question":"Explain simply","language":"bn"}
```
Response:
```json
{"answer":"...","model":"...","stored":false}
```

## Important limitations
- AI output can be wrong, incomplete, or miss Banglish/context. It does not assess real-world danger, verify evidence, provide legal judgment, or contact services.
- Do not use the app to monitor anyone or upload intimate imagery. Keep demonstration evidence fictional.
- This is a local prototype, not production-ready. Before deployment, add authentication, deployment-specific CORS, secure hosting, privacy/security review, and carefully managed billing/abuse controls.
- The model name is configurable; it must be available to the API account.
