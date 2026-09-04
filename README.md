# SatQuery AI

SatQuery AI is an evidence-grounded GeoAI starter for SIH26167: ask questions about satellite imagery and receive explainable answers with evidence regions, metadata, and an auditable execution trace.

## Current milestone

**Phases 1-2 are runnable:** React + TypeScript + Vite frontend, protected login/register routes, Firebase Auth, image selection and preview, typed analysis workspace, deterministic Demo/Fallback VQA, and Firebase Storage/Firestore persistence when those services are enabled.

## Run it

Prerequisites: Node.js 20+ and Python 3.11+.

```powershell
Copy-Item .env.example .env
Copy-Item .env.example frontend/.env
Set-Location frontend
npm install
npm run dev
```

Open `http://localhost:5173`. Without Firebase values, use any email and a password with at least six characters; the dashboard will show **Demo / Fallback mode**. This mode never invents coordinates and intentionally marks its evidence as demo.

To use Firebase Auth and persistence, create a Firebase Web App, enable Email/Password sign-in, enable Firestore and Storage, copy the web config into `frontend/.env`, and restart Vite. Firebase Storage may require the Blaze plan; if you do not upgrade, the app automatically keeps selected images and demo analysis local in the browser. The backend settings belong in `backend/.env`. Do not place service-account JSON or admin credentials in frontend files.

Run the backend in a second terminal:

```powershell
Set-Location backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m uvicorn app.main:app --reload
```

Backend health: `http://localhost:8000/health`. The analyze endpoint requires `Authorization: Bearer <Firebase ID token>`; the current placeholder gate checks presence and format, while production must verify the token with Firebase Admin SDK before trusting the user identity.

## Structure

- `frontend/`: Vite React app, auth store, Firebase client, protected dashboard.
- `backend/app/`: FastAPI routes, model registry, demo adapters, geospatial hooks.
- `demo/`: sample logical dataset and analysis document; no fabricated coordinates.
- `docs/`: architecture, Firebase setup, adapter contract, demo instructions.
- `firestore.rules`, `storage.rules`: starting security rules; test with the Firebase Emulator before deployment.

## Safety boundaries

Real VQA, grounding, change detection, GIS calculations, and satellite catalog access are not implemented by the demo adapter. Model adapters must return evidence and confidence only when supported by input data. Missing CRS must surface: `Geospatial coordinates cannot be reliably determined from this image.` Low confidence must surface: `Low-confidence result - manual verification recommended.`

## Phases

1. Auth + Dashboard - complete starter
2. Upload + Storage + metadata extraction
3. Single-image VQA + captioning
4. Grounding polygons
5. Before/After change detection
6. Optical + SAR fusion
7. Master Agent orchestration
8. GIS calculations
9. Multi-temporal timeline
10. Evidence, confidence, and trace UI
11. Reports, history, and alerts
12. Voice + EN/HI/KN
13. Security hardening, deployment, and polish
