---
name: Testing the Shoku-Front Expo app
description: How to run the Jest suite and start the Expo web app for manual verification, including the temporary blockers that must be worked around.
---

# Testing Shoku-Front Expo app

## Repository

`C:\Users\Administrator\repos\Shoku-Front` (Windows)

## Run Jest

```powershell
cd C:\Users\Administrator\repos\Shoku-Front
npx jest --ci --no-coverage
```

The only test file is `components/__tests__/ThemedText-test.tsx`.

## Run the web app

The project is missing two things required for bundling:

1. `constants/config.ts` is gitignored and not present.
2. `axios` is imported in `context/OrdersContext.tsx` and `context/MenuContext.tsx` but not in `package.json`.

Create a temporary `constants/config.ts` stub that exports every `Config.*` field referenced in the source:

```ts
export const Config = {
  API_URL: 'http://localhost:8000',
  API_URL_LOCAL: 'http://localhost:8000',
  API_URL_WS: 'ws://localhost:8000',
  API_URL_LOCAL_WS: 'ws://localhost:8000',
  API_URL_WS_PROD: 'wss://example.com',
  APP_URL: 'https://example.com',
  IMGBB_API_KEY: '',
  IMGBB_API_URL: 'https://api.imgbb.com/1/upload',
};
```

Install axios without saving:

```powershell
npm install axios --no-save --legacy-peer-deps
```

Then start the web dev server on an available port:

```powershell
npx expo start --web --port 8082
```

Open the printed URL (usually `http://localhost:8082`) in Chrome. The home route should show the Shoku logo and the `Ver Carta` / `Llamar a Mesero` buttons.

## Common issues

- Port 8081 may already be in use; pick a higher port such as 8082.
- `npm install` currently needs `--legacy-peer-deps` because of the `react-test-renderer@19` / `react@19` peer-dependency setup.
- `app/garzon/` has a case-sensitive filename collision (`Estado.tsx` vs `estado.tsx`) that on Windows leaves `estado.tsx` empty.
- Expo may emit package-version mismatch warnings; they do not necessarily prevent the web bundle from compiling.

## Devin Secrets Needed

None.
