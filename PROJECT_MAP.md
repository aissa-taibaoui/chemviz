# ChemViz: Visionary Chemistry Mechanism Player

## TECH_STACK

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| **UI** | React | 19.2.6 | Component framework |
| **3D** | Three.js | r184 (0.184.0) | 3D rendering engine |
| **3D React** | @react-three/fiber | 9.6.1 | React renderer for Three.js |
| **3D Drei** | @react-three/drei | 10.7.7 | R3F helpers (OrbitControls) |
| **Chem Engine** | @rdkit/rdkit | 2025.3.4-1.0.0 | Molecule operations (WASM) |
| **Reactions** | openchemlib | 9.22.1 | Reaction SMILES, RXN files |
| **Naming** | OPSIN / PubChem (REST API) | — | IUPAC name → SMILES |
| **Build** | Vite | 8.x | Dev server + bundler |
| **State** | Zustand | 5.x | Lightweight state management |
| **Language** | TypeScript | 6.0 | Strict mode, ESM |

## ARCHITECTURE

```
C:\chemviz\
├── index.html
├── package.json
├── tsconfig.json / tsconfig.*.json
├── vite.config.ts
├── eslint.config.js
└── src/
    ├── main.tsx
    ├── config.ts
    ├── vite-env.d.ts
    ├── core/
    │   ├── types.ts               # Molecule, Reaction, MechanismStep, BondChange
    │   ├── logging.ts             # Async log (debug|info|warn|error)
    │   └── chemistry/
    │       ├── bridge.ts          # ChemistryEngine: init RDKit WASM
    │       ├── molecule.ts        # getFormula(), getMW(), getInChI(), get3DCoords()
    │       ├── reaction.ts        # executeReaction(), getMechanism(), validateBondChanges()
    │       └── naming.ts          # iupacToSmiles() via OPSIN + PubChem fallback
    ├── ui/
    │   ├── theme.ts               # Carbon Dots dark theme
    │   ├── Button.tsx
    │   ├── Card.tsx
    │   └── Input.tsx
    ├── app/
    │   ├── App.tsx                # Tab router (3 views)
    │   └── layout.tsx             # App shell with header
    └── features/
        ├── compound-browser/
        │   ├── CompoundInput.tsx
        │   ├── CompoundViewer.tsx
        │   └── useCompound.ts
        ├── reaction-simulator/
        │   ├── ReactionInput.tsx
        │   ├── ReactionViewer.tsx
        │   ├── MechanismViewer.tsx
        │   └── useReaction.ts
        └── mechanism-player/
            ├── MechanismScene.tsx  # R3F Canvas with controls
            ├── CurvedArrow.tsx     # Quadratic Bezier arrow in 3D
            ├── Molecule3D.tsx      # 3D atom/bond rendering
            ├── BondBreakAnimation.tsx
            └── useMechanism.ts
```

## PENDING

| Item | Status | Notes |
|------|--------|-------|
| RDKit WASM init strategy | ✅ Done | Async init in bridge.ts, loading state in components |
| OPSIN rate limits | ✅ Done | Fallback to PubChem API |
| Reaction mechanism prediction | ⚠️ Basic | Predefined templates; ML future |
| Carbon Dots theme tokens | ✅ Done | Dark theme with glow effects |
| Curved arrow 3D math | ✅ Done | Quadratic Bezier + cone arrowhead |
| Reaction step validator | ✅ Done | Bond change sanity checks |
| Bundle size optimization | ⚠️ Pending | RDKit WASM is ~13MB; need lazy loading |
| Ketcher sketcher integration | ❌ Future | Molecule editor canvas |
| PWA / Service Worker | ❌ Future | Offline support |

## DEV

```powershell
cd C:\chemviz
npm run dev     # Start dev server
npm run build   # TypeScript check + production build
npm run preview # Preview production build
```
