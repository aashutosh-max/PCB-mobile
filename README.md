PocketEDA — Complete Project File Structure & Setup Guide
=========================================================

When you extract `PocketEDA_Source.zip` into a project folder (e.g. `pocketeda/`),
your folder and file hierarchy should match the exact tree below:

pocketeda/
├── .env.example <br>
├── .gitignore
├── FILE_STRUCTURE.txt
├── index.html
├── metadata.json
├── package.json
├── tsconfig.json
├── vite.config.ts
├── public/
│   ├── icon.svg
│   └── manifest.webmanifest
└── src/
    ├── App.tsx
    ├── index.css
    ├── main.tsx
    ├── core/
    │   ├── commands.ts
    │   ├── drc.ts
    │   ├── geometry.ts
    │   ├── gerber.ts
    │   ├── netlist.ts
    │   ├── serialization.ts
    │   ├── types.ts
    │   └── __tests__/
    │       └── core.test.ts
    └── ui/
        ├── canvas/
        │   └── CadCanvas.tsx
        ├── components/
        │   ├── BottomSheets.tsx
        │   └── EditorChrome.tsx
        ├── gestures/
        │   └── gestureRecognizer.ts
        └── store/
            └── useEditorStore.ts


=========================================================
File-by-File Placement List (Relative to Project Root)
=========================================================

1. Root Configuration Files:
   - /.env.example
   - /.gitignore
   - /FILE_STRUCTURE.txt
   - /index.html
   - /metadata.json
   - /package.json
   - /tsconfig.json
   - /vite.config.ts

2. Public / PWA Assets (`public/`):
   - /public/icon.svg
   - /public/manifest.webmanifest

3. Application Entry (`src/`):
   - /src/main.tsx
   - /src/App.tsx
   - /src/index.css

4. Pure TypeScript CAD Core Engine (`src/core/`):
   - /src/core/types.ts
   - /src/core/geometry.ts
   - /src/core/netlist.ts
   - /src/core/drc.ts
   - /src/core/gerber.ts
   - /src/core/commands.ts
   - /src/core/serialization.ts
   - /src/core/__tests__/core.test.ts

5. Touch Gestures, State & Canvas UI (`src/ui/`):
   - /src/ui/gestures/gestureRecognizer.ts
   - /src/ui/store/useEditorStore.ts
   - /src/ui/canvas/CadCanvas.tsx
   - /src/ui/components/EditorChrome.tsx
   - /src/ui/components/BottomSheets.tsx


=========================================================
How to Run Locally
=========================================================

1. Install dependencies:
   npm install

2. Start development server (runs on port 3000):
   npm run dev

3. Run core unit tests (Vitest):
   npm run test

4. Build production bundle:
   npm run build