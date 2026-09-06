# Agri Mitra — Vercel deployment

## Architecture

Browser (Vite/React)
  -> POST /api/agri-ai
  -> Vercel Serverless Function
  -> OpenAI Responses API (server-side)

Leaf Scanner, Soil Scanner, and Ask Agri AI all use the same real OpenAI backend. No OpenAI secret is shipped to the browser.

## Deploy

1. Import the project into Vercel.
2. Vercel detects Vite and uses `npm run build`, outputting `dist`.
3. Add `OPENAI_API_KEY` under Vercel Project Settings → Environment Variables.
4. If you use the existing Supabase auth/database features, add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` too.
5. Redeploy after adding environment variables.

## Local

```bash
npm install
npm run dev
```

For local AI calls, provide `OPENAI_API_KEY` in a local `.env`/`.env.local` file (never commit it). The frontend still calls `/api/agri-ai`.
