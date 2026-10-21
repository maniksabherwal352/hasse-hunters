# THE POSET DUNGEON — HASSE TRIALS

> "Where every path is a proof."

A browser-based educational dungeon puzzle game for **Discrete Mathematics (Relations, POSETs & Functions)**. The player escapes six chambers by analyzing relations with a **real math engine**, repairing/constructing **Hasse diagrams** (SVG), and reading step-by-step proofs for every verdict.

**No backend. No login. No build step.** Open `index.html` and play.

---

## 1. Problem Statement

Students can often *recite* "reflexive / antisymmetric / transitive" but fail to *apply* them: minimal vs least, cover vs transitive edge, or reading a Hasse diagram. This project turns those exact misconceptions into game mechanics:

- Trial 1 punishes confusing symmetry with antisymmetry.
- Trial 2 traps players who equate minimal with least.
- Trials 3–4 reject transitive edges **with the middle element cited as proof**.
- The Final Boss accepts **genuinely new user input** and judges it live.

## 2. Features

- 6 playable rooms: Forge Bay · Restore Hall · Hasse Forge · Broken Diagram · Wild Master · The Broken Order (final boss)
- Real math engine (reflexive / symmetric / antisymmetric / transitive / partial order / comparability / minimal / maximal / least / greatest / upper-lower bounds / LUB / GLB / covers) — every verdict computed live, zero answer banks
- Directed relation playground: drag nodes, click-click arrows, double-click loops ↺, click arrow to remove, undo/redo, live REFLEXIVE/ANTISYMMETRIC/TRANSITIVE lamps
- Interactive SVG Hasse editor: drag nodes (snap assist), click-click cover edges, click edge to delete, undo/redo, auto-arrange by rank, placement checks — answers never listed
- Procedural generation (divisibility / numeric / subset / wild, Easy→Expert, validated) + adaptive difficulty + fresh structure every replay
- Diagram-play for bounds: collect treasures/peaks, crown least/greatest (with empty pedestals), tap LUB/GLB on the graph with bound highlighting
- Step-by-step live status panel with animated steps
- Gamification: 3 lives/room, XP (100 / 75 / 50 by hints + streak bonus), 6 keys, progress map, result + victory screens
- Mathematics Guide (13 sections incl. LUB/GLB), How-to-Play, Demo Mode (6 jumps for a 5-min presentation), sound toggle (WebAudio blips)
- Progressive Hasse ladder: L1 chain {1,2,3,4} → L2 branching {1,2,3,6} → L3 {1,2,3,4,6,12} (covers computed, never hardcoded) + wild practice forge
- “You discovered” feedback, concept badges (Hasse Apprentice → Poset Master)
- 18 unit tests (`tests/poset-tests.js`, runnable in browser or Node): core properties + covers + LUB/GLB + generation validity + damage/restore + adaptive difficulty
- Responsive (desktop 2-column → mobile stacked), accessible labels, keyboard-operable nodes

## 3. Mathematical Concepts Covered

Relations · reflexive · antisymmetric · transitive · partial orders · POSETs · comparable/incomparable · minimal/maximal · least/greatest · upper/lower bounds · LUB/GLB · covering relations · Hasse diagrams.

## 4. Technology Stack

HTML5 · CSS3 · Vanilla JavaScript (ES6, no framework) · SVG for Hasse diagrams · `localStorage` for progress · WebAudio for optional sound.

## 5. Architecture

```
UI LAYER (index.html + css/) 
   ↓
GAME ENGINE (js/game.js — state, XP, lives, keys)
   ↓
CHALLENGES (js/levels.js, js/challenges.js — generators; answers computed, never hardcoded)
   ↓
MATH ENGINE (js/relation.js, js/poset.js, js/hasse.js — pure logic, zero DOM)
   ↓
VISUALIZATION (js/renderer.js — SVG editor; js/ui.js — panels/toasts/modals; js/app.js — trial wiring)
```

Rule enforced in code: `relation.js / poset.js / hasse.js` never touch the DOM; they return `{result, explanation, evidence[], violations[]}` and the UI decides how to display it.

## 6. Folder Structure

```
poset-dungeon/
├── index.html
├── css/
│   ├── style.css        # theme, layout, components, hasse svg styles
│   ├── dungeon.css      # dungeon flavor (banners, stone, torches)
│   └── responsive.css   # tablet / mobile breakpoints
├── js/
│   ├── relation.js      # isReflexive/Symmetric/Antisymmetric/Transitive, partial order, validation
│   ├── poset.js         # comparable, minimal/maximal, least/greatest
│   ├── hasse.js         # isCover, getCoverRelations, validateHasseEdge, gradeHasseDiagram, ranks
│   ├── levels.js        # 6 room metadata
│   ├── challenges.js    # classic generators + boss input parsers + progressive L1/L2/L3 + LUB/GLB picker
│   ├── generator.js     # procedural generatePosetChallenge (Easy→Expert) + damageRelation + adaptDifficulty
│   ├── game.js          # state, XP/lives/keys/hints, localStorage
│   ├── renderer.js      # SVG HasseEditor (drag+snap, click-connect, undo/redo, collect + bound highlights)
│   ├── relation-forge.js # directed relation playground (arrows, loops, undo/redo)
│   ├── ui.js            # toasts, analysis panel, topbar, modals, sfx
│   └── app.js           # navigation + all 6 trials + guide + demo + victory
├── tests/
│   └── poset-tests.js   # 18 unit tests (browser modal + `node tests/poset-tests.js`)
└── README.md
```

## 7. Algorithms

| Task | Algorithm | Complexity |
|---|---|---|
| Reflexivity | scan each `a` for `(a,a)` | O(n) |
| Symmetry / antisymmetry | scan all pairs for mirrors | O(n²) |
| Transitivity | for each `(a,b)`, each `(b,c)`, require `(a,c)` | O(n³) |
| Minimal / maximal | for each `a`, scan others for strict below/above | O(n²) |
| Least / greatest | candidate `l` must satisfy `l≤x` (resp. `x≤g`) ∀x | O(n²) |
| Cover `a<b` | `aRb` and no `c` with `a<c<b` | O(n) per pair |
| All covers | cover test over all `a≠b` | O(n³) |
| Hasse layout | longest-chain rank from minimals over cover DAG | O(n·e) |

Brute force is deliberate: n ≤ 10, so clarity beats cleverness.

### Cover relation (the heart of the game)

```
a covers b  ⟺  aRb, a≠b, and ¬∃c distinct: aRc ∧ cRb
Hasse edges  =  exactly the covers (no loops, no transitive edges)
```

## 8. How to Run

**Option A — double-click:** open `poset-dungeon/index.html` in any modern browser. (Uses classic `<script>` tags, so `file://` works.)

**Option B — local server (recommended):**
```powershell
cd poset-dungeon
python -m http.server 8000
# then visit http://localhost:8000
```

## 9. How to Use / Play

1. **Home** → ENTER THE DUNGEON (or DEMO MODE for the scripted tour).
2. **Map** → enter the open room (replay finished rooms anytime — every replay generates a **fresh structure**).
3. **Room 1 (Forge Bay):** 4 hands-on stations — draw arrows, add loops ↺, remove both-way arrows, close chains. No questions; the live lamps turn green.
4. **Room 2 (Restore Hall):** repair a sabotaged relation in the graph until REFLEXIVE / ANTISYMMETRIC / TRANSITIVE are all ✓.
5. **Room 3 (Hasse Forge):** Level 1 chain → Level 2 branching → Level 3 grand hierarchy + 🎲 wild forge. Drag nodes (snap assist), click-click edges, ↺ undo / ↻ redo. The forge never lists the answer.
6. **Room 4:** same builder, starting from a sabotaged diagram — interrogate every line, restore covers, delete junk.
7. **Room 5 (Wild Master):** a fresh generated structure — restore the order, then forge its Hasse inline. Difficulty adapts to you.
8. **Boss (The Broken Order):** type a set + pairs → ANALYZE → tap treasures, peaks, least/greatest crowns and LUB/GLB **on your own diagram**.
9. Hints cost XP (100→75→50). Failed checks cost knowledge, never option-lists — every mistake names the repair.

### Sample presentation data (classic)

`A = {1,2,3,6,12}`, `aRb ⇔ a|b`. Computed covers: `1→2, 1→3, 2→6, 3→6, 6→12`:

```
       12
        |
        6
       / \
      2   3
       \ /
        1
```

## 10. Testing

- In-app: Home → **RUN MATH TESTS** → RUN TESTS (18/18 expected).
- CLI: `node tests/poset-tests.js` (loads the engine files with `vm`, no dependencies).

Covers: identity POSET · divisibility POSET + exact covers · missing reflexive · both-way antisymmetry fail · missing transitive link · least/greatest on chain · multi-minimal/maximal with no least/greatest · transitive edge rejection · invalid pair · empty input · LUB/GLB · L1 chain covers · L2/L3 divisibility covers · LUB/GLB puzzle picker · generator validity (all difficulties) · damage/restore round-trip · adaptive difficulty · generator kinds.

## 11. Screenshots

> Placeholder — capture and drop images here:
> `assets/home.png` · `assets/map.png` · `assets/trial1.png` · `assets/forge.png` · `assets/boss.png`

## 12. Team Contribution

| Member | Contribution |
|---|---|
| _Manik Sabherwal_ | Math engine (`relation.js`, `poset.js`, `hasse.js`) + unit tests |
| _Chaitanya Chavan_ | Hasse SVG renderer + Trials 3/4 |
| _Ayush Raina_ | Trials 1/2/5, Final Boss, generators |
| _Aditi Srivastava_ | UI/CSS, map/progression, guide, README, presentation |

*(Replace with actual names/roles.)*

## 13. Future Enhancements

- Matrix + digraph views of relations; equivalence classes / partitions room
- Timed "blitz" mode and daily seeded dungeon
- Export/import custom POSETs as JSON; shareable challenge links
- Function properties room (injective/surjective/bijective) for the Functions half of the unit
- Soundtrack + reduced-motion respect (`prefers-reduced-motion`)

---

**Priority contract kept:** mathematical correctness › dynamic input › Hasse generation/validation › explanations › gameplay › polish › cosmetic effects. Every button works; every verdict is computed.
