# React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type-aware lint rules:

```js
export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...

      // Remove tseslint.configs.recommended and replace with this
      tseslint.configs.recommendedTypeChecked,
      // Alternatively, use this for stricter rules
      tseslint.configs.strictTypeChecked,
      // Optionally, add this for stylistic rules
      tseslint.configs.stylisticTypeChecked,

      // Other configs...
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```

You can also install [eslint-plugin-react-x](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-x) and [eslint-plugin-react-dom](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-dom) for React-specific lint rules:

```js
// eslint.config.js
import reactX from 'eslint-plugin-react-x'
import reactDom from 'eslint-plugin-react-dom'

export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...
      // Enable lint rules for React
      reactX.configs['recommended-typescript'],
      // Enable lint rules for React DOM
      reactDom.configs.recommended,
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```


## Real OpenAI AI setup

Leaf Scanner, Soil Scanner, and Ask Agri AI now call the `agri-ai` Vercel Serverless Function. The browser does not contain an OpenAI API key.

Set the server secret: `npx supabase secrets set OPENAI_API_KEY=YOUR_OPENAI_API_KEY` and deploy with `npx supabase functions deploy agri-ai`. The Edge Function uses `gpt-5.6-luna` with image input.

The scanner is strict: Leaf mode rejects non-leaf images and Soil mode rejects non-soil images. AI failures are shown to the user instead of silently pretending that a local heuristic result is AI.

## Vercel deployment (OpenAI AI)

This project is Vercel-ready. The browser calls `/api/agri-ai`, and Vercel runs `api/agri-ai.ts` server-side. The OpenAI API key is never bundled into the browser.

1. Import this repository/project into Vercel.
2. In Vercel: Project → Settings → Environment Variables, add `OPENAI_API_KEY` with your OpenAI API key for Production (and Preview if desired).
3. If using the existing Supabase login/database features, also add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`.
4. Deploy. Vercel runs `npm run build` and serves the Vite `dist` output plus the `/api/agri-ai` serverless function.
5. Test Leaf Scanner, Soil Scanner, and Ask Agri AI from the deployed URL.

Do not put `OPENAI_API_KEY` in a `VITE_` variable. Vite exposes `VITE_*` values to the browser.
