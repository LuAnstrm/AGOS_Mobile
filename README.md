# AGOS Mobile

Flood monitoring platform — mobile citizen app, backend API, ML engine, and simulation layer.
Both this app and the [AGOS Web dashboard](https://github.com/LuAnstrm/AGOS_Web) connect to the same backend.

## Layout

```
mobile/      Expo SDK 58 / React Native citizen app (Expo Router)
backend/     NestJS API (TypeORM + PostgreSQL); EKF in src/filtering/ekf.ts
ml-service/  FastAPI inference service (scikit-learn), managed with uv
simulator/   Node 24 script — sends synthetic sensor readings to the backend
```

## Run locally (four terminals)

```bash
cd backend && npm install && npm run db        # PostgreSQL on :5433
```

```bash
cd backend && npm run start:dev               # API on :3000
```

```bash
cd ml-service && uv sync && uv run uvicorn app.main:app --port 8000
```

```bash
cd simulator && npm run demo                  # 60 ticks, one every 2 s
```

```bash
cd mobile && npx expo start --web             # Expo web on :8081
```

## Environment variables

Copy `backend/.env.example` → `backend/.env` and fill in the values.
Set `EXPO_PUBLIC_AGOS_API` (or `extra.apiUrl` in `mobile/app.json`) to your backend URL.

## Simulation

```bash
cd simulator && npm run demo                  # normal scenario
cd simulator && npm run sos -- --count 5      # generate SOS requests
```

The storm scenario (flash surge + recession):
```bash
cd simulator && npm run listen               # listen for events
cd simulator && node --experimental-strip-types src/index.ts --scenario storm
```

## Deployment

The backend is a standard NestJS app. Deploy to Railway, Render, or any Node.js host.
Set `DATABASE_URL` to a hosted PostgreSQL instance and `WEB_ORIGINS` to your deployed web
dashboard URL.
