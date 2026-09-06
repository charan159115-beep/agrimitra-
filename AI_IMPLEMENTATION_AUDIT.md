# Agri Mitra AI — AI implementation audit

## Before the changes
- The chat layer could call real LLM APIs (NVIDIA/Gemini/OpenAI) when a key was available.
- The leaf/soil scanner itself was **not a trained AI/vision model**. `src/lib/ai.ts` used pixel-color heuristics and hard-coded diagnosis/soil rules.
- The scanner UI described those rules as a “trained AI model”, which overstated what was actually implemented.

## What is implemented now
- Leaf and soil scanning first uses a **multimodal vision model** when an API key is available.
- NVIDIA keys (`nvapi-*`) use `meta/llama-3.2-11b-vision-instruct` through NVIDIA's OpenAI-compatible `/v1/chat/completions` endpoint with a base64 image.
- Gemini keys (`AIza*`) use `gemini-2.5-flash` with inline image data and JSON response mode.
- The vision result is normalized into the existing `LeafScanResult` / `SoilScanResult` types so the existing result UI continues to work.
- The old pixel-analysis engine remains as a **fallback** when no compatible AI key is available or a vision API call fails.
- Soil output explicitly treats pH and nutrient values from a photo as estimates and recommends lab/Soil Health Card verification.
- Scanner wording was changed from “trained AI model” to “multimodal AI vision model” so the UI matches the implementation.

## Practical maturity
The project is now a working **AI-powered prototype**, not a custom-trained agricultural disease model. A production-grade plant-disease system would still benefit from a dedicated agricultural vision model/dataset, evaluation set, calibration, and server-side secret management.

## Run
1. Copy `.env.example` to `.env`.
2. Add your own API key(s).
3. Run `npm install`.
4. Run `npm run dev`.

For production, move provider API calls behind a server/Supabase Edge Function so browser users cannot inspect provider keys.
