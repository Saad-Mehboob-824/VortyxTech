# Email Integration (Resend) — vortyx Tech

This repository includes a serverless endpoint and a contact form which uses Resend to send contact emails securely.

## Files
- `api/send-email.js` — Vercel/Netlify serverless function that accepts a POST request and sends an email via Resend. The function uses `RESEND_API_KEY` from environment variables and optionally `CONTACT_RECIPIENT` and `SEND_FROM`.
- `email_templates/contact_email.html` — HTML email template with placeholders used by the serverless function.
- `index.html` — the contact form is wired to POST to `/api/send-email`. The page provides inline success/error messaging.

## Environment variables
Set these on your deployment platform (Vercel/Netlify) or locally (in `.env` when testing with `server/sendEmail.js`):

- `RESEND_API_KEY` — Your Resend API key (required).
- `CONTACT_RECIPIENT` — The email address that receives contact form submissions (optional, default: hello@vortyxtech.ai).
- `SEND_FROM` — The "From" address for outgoing emails (optional, default: noreply@vortyxtech.ai).

## Local testing (optional)
1. Install dependencies and start an Express test server (if you want to run locally):

```bash
npm init -y
npm install express node-fetch dotenv
node server/sendEmail.js
```

2. Create `.env` with your `RESEND_API_KEY` and optional `CONTACT_RECIPIENT` and `SEND_FROM`.

3. Test by sending a POST (curl example):

```bash
curl -X POST http://localhost:3000/api/send-email \
  -H "Content-Type: application/json" \
  -d '{"firstName":"Alex","lastName":"Kim","email":"you@company.com","company":"Acme","message":"Hello from curl"}'
```

## Using serverless function
- Deploy to Vercel / Netlify or another serverless platform.
- Ensure environment variables are set.
- The contact form on `index.html` posts to `/api/send-email` and will show an inline success/error message.

## Security and spam protection
- Do NOT expose your `RESEND_API_KEY` in client-side code.
- For production, consider adding reCAPTCHA or a honeypot field on the contact form.
- Add rate-limiting in the serverless function if needed.

## Notes
- The serverless function tries to use `email_templates/contact_email.html` if present; otherwise it builds a fallback inline HTML email.
- The client shows a friendly inline success or error message — no modal alerts.

