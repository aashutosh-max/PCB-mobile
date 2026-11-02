# PocketEDA

**PocketEDA** is a mobile-only, touch-first, installable Progressive Web App (PWA) for schematic capture and 2-layer PCB layout, inspired by KiCad. Designed from the ground up for two-thumb smartphone ergonomics, it uses HTML5 Canvas 2D rendering, unified Pointer Events gestures, a pure TypeScript CAD engine, live ratsnest airwires, Design Rule Checking (DRC), and RS-274X Gerber + Excellon drill fabrication exports.

---

## Key Features

- **Touch-First Dual Editors (Schematic + 2-Layer PCB)**:
  - **Schematic Editor**: KiCad Eeschema-style crisp white sheet, `1.27 mm` snap grid, thin green orthogonal wires (`#008400`), maroon symbol bodies (`#840000`), net labels, and power symbols (`VCC`, `GND`).
  - **2-Layer PCB Editor**: Dark high-contrast canvas with configurable `Grid` or `Dots` raster (`0.50 mm` / `1.27 mm`), **Copper Top (`F.Cu`)**, **Copper Bottom (`B.Cu`)**, **Silkscreen Top/Bottom**, **Solder Mask Top/Bottom**, rounded board outlines (`Edge.Cuts`), through-hole & SMD pads, vias, and optional auto ground copper pours.
- **Unified Pointer Events Gesture Engine**:
  - **Tap**: Select item, start/finish wire or copper trace, or place footprint/symbol.
  - **1-Finger Drag**: Move components/footprints on the snap grid, route traces, or pan empty canvas.
  - **2-Finger Drag & Pinch**: Smooth pan and zoom around focal point.
  - **Double-Tap**: Auto-fit schematic or PCB board to screen.
  - **Long-Press**: Context menu (`Rotate 90°`, `Properties`, `Delete`, `Fit Board to View`).
- **Live Netlist & Ratsnest Engine**:
  - Union-Find connectivity engine over schematic pins, wires, labels, and PCB pad nets.
  - Live Minimum Spanning Tree (Prim's algorithm) ratsnest airwires that automatically clear as copper traces and vias complete connections.
  - Supports both **Schematic-driven sync** and **direct PCB pad-to-pad airwire/trace routing**.
- **20-Category Official Footprint Library + Web `.kicad_sym` Importer**:
  - Built-in parts across 20 categories (`Audio`, `Capacitors`, `Connectors`, `Crystals & Oscillators`, `Diodes`, `Displays`, `Fabrication`, `Fuses & Protection`, `ICs`, `Inductors`, `LEDs`, `Mechanical`, `Modules`, `Potentiometers & Trimmers`, `Relays`, `Resistors`, `Sensors`, `Sockets`, `Switches`, `Transistors & Regulators`).
  - Import external `.kicad_sym` or `.json` libraries via URL or local file upload, or create custom footprint macros in the **Mine** tab.
- **Design Rule Check (DRC) & Fabrication Export**:
  - Checks unrouted nets, trace width violations, via drill rules, pad/trace copper clearances, courtyard overlaps, and board boundary violations.
  - Exports RS-274X Gerber layers (`.gtl`, `.gbl`, `.gto`, `.gm1`), Excellon drill files (`.drl`), Bill of Materials (`.csv`), and full project `.json` / `.zip` bundles.

---

## Repository File Structure

```markdown
pocketeda/
├── .env.example
├── .gitignore
├── FILE_STRUCTURE.txt
├── README.md
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
```

### Architecture Breakdown

| Path | Responsibility |
| :--- | :--- |
| `src/core/types.ts` | Immutable document model (`Project` → `Sheet` → `Item`), layer types, board & view settings, DRC & netlist interfaces. |
| `src/core/geometry.ts` | Coordinate transforms (`mm`, Y-down), grid snapping (`1.27 mm` SCH, `0.5 mm` PCB), touch hit-testing (`~22 px` hit-slop), 20-category component library, and `.kicad_sym` parser. |
| `src/core/netlist.ts` | `UnionFind` solver for schematic pin/wire/label connectivity and live PCB ratsnest MST calculation. |
| `src/core/drc.ts` | Design Rule Checker for clearance, trace width, annular ring, courtyard overlap, and unrouted nets. |
| `src/core/gerber.ts` | RS-274X Gerber generator (`F.Cu`, `B.Cu`, `F.SilkS`, `Edge.Cuts`), Excellon `.drl` writer, BOM CSV exporter, and pure-TS `.zip` archiver. |
| `src/core/commands.ts` | Command pattern mutations with 60-step Undo/Redo history and Schematic ↔ PCB footprint synchronization. |
| `src/core/serialization.ts` | Project JSON serializer/deserializer and built-in starter circuit templates (*Pocket LED Module* & *NE555 Astable Blinker*). |
| `src/ui/gestures/gestureRecognizer.ts` | Unified Pointer Events state machine (`tap`, `double-tap`, `long-press`, `1-finger drag`, `2-finger pan/pinch`). |
| `src/ui/canvas/CadCanvas.tsx` | HTML5 Canvas 2D renderers for `SchematicCanvas` and `PcbCanvas`, including rulers, coordinate readout, and touch magnifier loupe. |
| `src/ui/components/EditorChrome.tsx` | Mobile top bar, floating bottom-left tool palette, zoom controls, context menu, and custom on-screen numeric keypad. |
| `src/ui/components/BottomSheets.tsx` | Slide-up sheets and full-screen modals for `Footprints` (`Official` / `Import` / `Mine`), `Board Settings` (`Layers` / `Board` / `View`), `Properties`, `DRC`, and `Export`. |

---

## Getting Started

### Prerequisites

- **Node.js** `>= 18.0.0` (Node 20+ recommended)
- **npm** `>= 9.0.0` (or `pnpm` / `bun`)

### 1. Clone or Extract the Repository

```bash
git clone https://github.com/<your-username>/pocketeda.git
cd pocketeda
```

*(If you downloaded `PocketEDA_Source.zip`, extract it into a `pocketeda` folder and open a terminal in that directory.)*

### 2. Install Dependencies

```bash
npm install
```

### 3. Start the Development Server

```bash
npm run dev
```

- Open `http://localhost:3000` in your mobile browser or desktop browser DevTools **Device Toolbar** (e.g., iPhone 14 Pro / Pixel 7 viewport).

### 4. Run Core Unit Tests (Vitest)

```bash
npm run test
```

### 5. Type-Check & Build for Production

```bash
npm run lint
npm run build
npm run preview
```

---

## Tech Stack

- **Framework**: React 19 + TypeScript + Vite
- **Rendering**: HTML5 Canvas 2D (`touch-action: none`)
- **State Management**: Zustand with immutable Command Pattern Undo/Redo
- **Styling**: Tailwind CSS v4 (for mobile chrome, toolbars, and bottom sheets)
- **Testing**: Vitest (`src/core/__tests__/core.test.ts`)
- **PWA**: Web App Manifest (`standalone`, portrait + landscape)