# Vitality Food Demo

This repository contains the React demo app plus the local Express adapter used for the nutrition logging flow.

## Current Integration Scope

Only the `Log Meal` vision path has been switched to the local nutrition model.

- Frontend still posts meal images to `POST /api/vision/analyze`
- Express now forwards that request to the local Python nutrition service
- Daily coach and other app flows remain on the existing app architecture

## Architecture

- Vite / React frontend on `http://127.0.0.1:3000`
- Express API on `http://127.0.0.1:8787`
- Local Python nutrition service on `http://127.0.0.1:8000`

Request flow for meal logging:

1. `src/lib/vision.ts` sends `imageDataUrl`, `ingredientsText`, and `portionText` to Express.
2. `server/index.ts` converts the image Data URL into a multipart upload.
3. Express calls the Python model service at `VISION_PYTHON_URL`.
4. Express adapts the Python response back into the frontend shape expected by `LogMeal.tsx`.

## Repository Layout

- `src/`: React frontend
- `server/index.ts`: Express API
- `src/screens/LogMeal.tsx`: meal logging screen
- `src/lib/vision.ts`: frontend vision request helper

## Large Model Assets

Large runtime assets are intentionally not stored in this repository.

Examples of excluded files:

- model checkpoints such as `.pt` and `.bin`
- large FAISS indexes
- large `.npy` feature arrays
- bundled frozen runtime archives

GitHub is used here for code, integration logic, and docs only.

## Environment Variables

Create `.env.local` in the repository root:

```env
AI_API_KEY="sk-or-v1-..."
AI_HTTP_REFERER="http://127.0.0.1:3000"
AI_APP_TITLE="Nutritional Advisor"
AI_COACH_MODEL="moonshotai/kimi-k2-thinking:nitro"
VITE_APP_API_BASE_URL="http://127.0.0.1:8787"
VISION_PYTHON_URL="http://127.0.0.1:8000/api/vision/analyze"
```

Notes:

- `VISION_PYTHON_URL` is required for the local `Log Meal` model integration.
- `AI_API_KEY` is still needed if you want the daily coach flow to use the existing external AI path.

## Local Run

Install frontend and Express dependencies:

```powershell
npm install
```

Start the Express API:

```powershell
npm run dev:api
```

Start the frontend:

```powershell
npm run dev
```

Start the Python nutrition service separately from your runtime bundle:

```powershell
python vision_api_server.py --host 127.0.0.1 --port 8000
```

Then open:

```text
http://127.0.0.1:3000
```

## What Changed In This Version

- Replaced the `Log Meal` Express vision route with a Python-service adapter
- Preserved the existing React to Express contract for the meal logging UI
- Kept the coach flow untouched
- Added `VISION_PYTHON_URL` configuration for local routing

## Publishing Notes

If you need a teammate handoff with runtime assets, keep those assets in a separate package or release bundle rather than committing them to GitHub.
