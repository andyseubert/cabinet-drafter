# AGENTS.md

## Project

**CabinetDrafter — free cabinet design, material planning, nesting, and 3D.**

Repository: `andyseubert/cabinet-drafter`

Live site: `https://andyseubert.github.io/cabinet-drafter/`

CabinetDrafter is a self-contained browser cabinet planner. The primary application is `index.html`, with no required server, account, CDN, framework, or external JavaScript dependency. It supports cabinet layout, drawer/open-space sizing, plywood BOMs, cut lists, sheet nesting, an interactive browser 3D preview, project JSON save/load, and optional SketchUp Ruby export.

Current public version: **1.2.11**.

Future user-facing changes should bump the version according to semver unless there is a good reason not to.

## Working style

Before making changes:

1. Read this file.
2. Read `README.md`, `docs/construction.md`, and `CHANGELOG.md`.
3. Inspect `index.html` and recent git history before editing.
4. Preserve existing behavior unless the requested change explicitly alters it.
5. Surface geometry/material assumptions instead of silently inventing them.

Use normal git workflow. Prefer small, understandable commits. Run tests before committing. Show the user meaningful diffs or summaries, not giant pasted generated files.

Do not rewrite the application into a framework or add a build system just for convenience. A major advantage of CabinetDrafter is that `index.html` can be downloaded and opened directly.

## User-facing style

Use plain, direct language. Avoid marketing fluff and AI-sounding filler.

The user is actively building real cabinets from these dimensions. Treat geometry, BOM, cut-list, and nesting changes as consequential. If a rule is ambiguous, reason through the physical construction before changing code.

## Important SketchUp instruction

In current SketchUp Pro, the Ruby console path used by this project is:

**Extensions → Developer → Ruby Console**

Do not document it as `Window → Ruby Console`.

SketchUp Web cannot execute Ruby extensions/scripts.

## Core construction model

### Global defaults

- Standard carcass depth: 25 in.
- Standard carcass height: 38 in.
- Cabinet-box plywood thickness: 3/4 in.
- Default visible/front plywood: cherry.
- Default hidden cabinet-box plywood: birch.
- Default drawer-box plywood: birch.
- Cabinet rows can override box/carcass plywood species and face/front plywood species independently.
- Separate toe kick/base: 2 in high.
- Default toe-kick setback: 3 in.
- Default toe-kick depth: 22 in.
- Cabinet + toe kick = 40 in before countertop.
- Structural joints generally use pocket screws.

### Front construction

Each cabinet can currently be:

- Frameless
- Face-frame overlay
- Face-frame inset

Frameless remains the default so old projects do not change.

Face-frame support in 1.1 is intentionally limited to a perimeter frame:

- two full-height stiles
- top rail
- bottom rail
- no intermediate rails between individual drawers/cubbies yet

Default face-frame values:

- stile width: 1.5 in
- rail width: 1.5 in
- thickness: 0.75 in
- material: cherry solid stock
- overlay beyond opening: 0.5 in per edge
- inset reveal: 1/16 in per edge

Face-frame stock is linear solid stock and must not be nested onto plywood sheets.

### Drawer slides and boxes

- Side-mount slides.
- Default total horizontal slide clearance: 1 in = 1/2 in each side.
- Default drawer depth: 24 in.
- Drawer sides/front/back: 1/2 in drawer-box plywood.
- Drawer bottom: 1/2 in drawer-box plywood.
- Drawer-box material defaults to birch plywood and can be selected independently from visible/front and hidden cabinet-box material.
- Bottom fits fully between all four drawer walls.
- No dado/rabbet assumed.
- Entered drawer box height is the outside wall height.
- Default bottom drawer clearance: 0.5 in.
- Default gap between drawer boxes: 0.5 in.
- Drawer stack is entered and resolved bottom-to-top.

For a default 38 in carcass, usable internal drawer-box height is 36.5 in because the box is between the 3/4 in bottom and underside of the 3/4 in top stretcher region.

### Drawer-front alignment

This was fixed in 1.1.1 and must not regress.

Automatic drawer-front reveal lines are tied to physical drawer positions. The reveal between adjacent drawers is centered in the actual drawer-box gap.

Therefore matching bottom-up drawer sequences across neighboring cabinets must align even when the upper layout differs.

Example that must remain aligned:

- cabinet A: `D7.5, D7.5, O19.25`
- cabinet B: `D7.5, D7.5, D7.5, D7.5, D4`

The first shared drawer-front top/reveal must occur at the same elevation in both.

Do not return to the old behavior of evenly redistributing spare face height across every drawer in each cabinet.

### Drawer/mixed stack syntax

The stack field is bottom-to-top.

Explicit syntax:

- `D10, O8, D7`
- `D7, Open 9.5, D7`

`O` is the letter O for Open, not zero.

Accepted open aliases include `O`, `Open`, `S`, `Shelf`, `C`, and `Cubby`.

Compatibility rules:

- An all-number list such as `10, 7, 5.25` means all drawers.
- Once any explicit `D` is present, a bare number means an open cubby.
- `D9, D9, 15` therefore means two 9 in drawers and one 15 in open cubby.
- Adjacent open zones remain separate open cubbies and receive fixed divider panels between them.
- `D4.75, O12, O12, D4.75` therefore means a drawer, two separate 12 in clear open cubbies, and a drawer.

Open height means clear cubby height. Drawer height means outside drawer-box wall height.
The drawer-box gap applies only between adjacent drawer boxes; fixed divider panels separate drawers from open cubbies.

### Pure drawer carcasses

- Default 3/4 in hidden cabinet-box plywood carcass; cabinet-row box/carcass override can change the species.
- 3/4 in solid plywood bottom.
- No full top.
- No full back.
- Four 4 in stretchers:
  - front top, flat
  - rear top, flat
  - upper rear, vertical
  - lower rear, vertical and resting on bottom

### Single-door cabinets

- Default 3/4 in visible/front plywood cabinet box; cabinet-row box/carcass override can change the box, stretchers, and shelf species.
- 3/4 in solid plywood bottom.
- No full top.
- Full applied back on the outside rear face, not inset.
- Back material: cabinet face/front plywood matching the door.
- Back thickness follows the configured back thickness, default 1/4 in.
- No rear vertical stretchers.
- Two flat 4 in top stretchers:
  - front top, flat
  - rear top, flat
- Optional adjustable shelves use the cabinet box/carcass plywood and shelf pins.

### Open-shelf cabinets

- Default 3/4 in visible/front plywood carcass; cabinet-row box/carcass override can change the box and shelf species.
- Applied back on the outside rear face, not inset.
- Default applied back: 1/4 in plywood matching visible/front material, configurable to match hidden cabinet-box material or explicit birch/cherry.
- Full-width, full-height back.
- No rear vertical stretchers.
- Two flat 4 in top stretchers.
- Bottom/shelves extend full carcass depth.
- Applied back adds to physical maximum depth.
- Adjustable shelves use shelf pins.

Explicit clear opening heights may be entered bottom-to-top, for example:

`9.5, 12, auto`

`auto` may only be the final/top opening and consumes remaining clear height. A blank list falls back to evenly spaced shelves from shelf count.

### Mixed drawer/open-cubby cabinets

- Carcass defaults to visible/front plywood because the interior is visible; cabinet-row box/carcass override can change the species.
- Two flat 4 in top stretchers only; rear vertical stretchers are omitted where localized backs provide rear closure.
- Fixed divider panels are 3/4 in by default, 24 in deep by default, pocket-screwed, and default to the visible/front material.
- Divider panels can instead match the hidden cabinet-box material or be set explicitly to birch/cherry.
- Every contiguous open-cubby run gets a localized applied back.
- Drawer-only areas remain open-backed.
- A mixed-cubby back spans the full outside cabinet width.
- A single mixed-cubby back height = clear opening height + one carcass thickness below + one above.
- Adjacent open cubbies share one localized back spanning the combined clear openings, the divider panel(s) between them, and one carcass thickness below and above.
- With 3/4 in carcass, a single-cubby back height = clear height + 1.5 in.
- Top cubbies still use the top-stretcher thickness as the upper allowance even though there is no full top.

Example:

18 in cabinet with `D9, D9, O15` gets an 18 in × 16.5 in × 1/4 in localized back.

### Fronts and doors

- Default material: 3/4 in visible/front plywood, which defaults to cherry.
- Cabinet rows can override the face/front plywood species for drawer fronts, doors, single-door backs, and that cabinet's visible toe-kick skin.
- Frameless construction is full overlay.
- Frameless side reveal default: 1/16 in.
- With 3/4 in carcass, that means 11/16 in geometric overlap per side.
- Two adjacent cabinets therefore have a 1/8 in seam by default.
- Vertical front gap default: 1/8 in.
- Single-door cabinets currently assume two concealed Euro-style hinges.
- Exact hinge make/model and drilling pattern are not yet selected.

### Toe kicks

- Separate ladder frame.
- 3/4 in hidden cabinet-box plywood structural rails.
- Front rail, rear rail, and two side rails.
- Intended to use offcuts where possible.
- Visible front gets 1/4 in the cabinet's face/front plywood or veneer skin.
- BOM treats adjacent cabinets with the same face/front species as continuous installed runs for visible-material skin planning.
- Multiple-wall/run support remains a known limitation.

## BOM and nesting behavior

Materials currently include:

- 3/4 visible/front plywood, default cherry
- 3/4 hidden cabinet-box plywood, default birch
- cabinet-specific 3/4 box/carcass and face/front plywood overrides
- drawer-box plywood, default 1/2 birch including drawer bottoms
- configurable mixed-cabinet divider plywood, default matching visible/front
- configurable thin open-space back, default 1/4 matching visible/front
- single-door back stock, using the configured back thickness and visible/front material
- visible/front toe-kick skin, default 1/4 cherry
- face-frame solid stock tracked separately

Nesting rules:

- Configurable sheet width/length, kerf, edge trim, spare sheets.
- Visible grain preservation is on by default.
- Grain is treated as running along the 96 in sheet direction.
- Hidden cabinet-box and drawer-box parts may rotate.
- Grain-sensitive visible parts do not rotate when grain preservation is on, regardless of whether the selected visible/front species is birch or cherry.
- Layout is a practical rectangle nesting plan, not a table-saw cut sequence.
- Cut-layout sheet diagrams are black-and-white friendly: part rectangles use species hatch patterns, while material/thickness is named in the sheet header.
- Nesting free-space rectangles must remain non-overlapping so placed parts never intersect; preserve configured kerf and edge trim as cutting clearance.
- Do not claim mathematically minimal sheet count.

The detailed cut list, BOM, layout diagrams, browser preview, and SketchUp export should agree on geometry.

## Units

- User can choose inches or millimeters.
- Canonical geometry is stored internally in inches.
- Do not repeatedly convert canonical data back and forth in a way that introduces drift.
- Old project JSON with no units should load as inches.
- Explicit suffixes such as `250 mm` and `9.5 in` are accepted.
- Inch annotations use practical fractions rounded to nearest 1/16.
- Metric labels use nearest mm.

## Project persistence

The application supports:

- browser localStorage recovery
- normal project JSON save/load
- browser File System Access API autosave when available
- IndexedDB storage for the chosen project file handle where available

Cabinet table order is the physical run order used by the BOM, 3D preview, and SketchUp export. Reordering cabinets must preserve each cabinet's stable `uid` so build-progress part IDs survive an order-only edit.

Project JSON is part of the user workflow. New fields must migrate safely.

Rules:

- Existing/old JSON files must continue to load.
- Missing new properties should receive safe defaults.
- New progress/inventory data must persist to JSON and browser autosave.
- Do not silently discard unknown/older state without a migration reason.

## 3D preview

The browser 3D preview is self-contained/offline and uses a Canvas software renderer rather than an external 3D library.

Controls include:

- drag to orbit
- wheel to zoom
- Shift-drag to pan
- double-click to fit
- Isometric / Front / Left / Right / Top presets
- show/hide fronts
- show/hide drawer boxes
- show/hide backs
- optional cabinet width labels in the browser preview and SketchUp export
- hidden-line print style for black-and-white PNG/print output
- PNG export

Keep this offline and dependency-free unless the user explicitly decides otherwise.

## SketchUp export

The generated Ruby script builds a native SketchUp model and replaces the previous `Generated Cabinet Lineup` group when rerun.

Every download uses a unique timestamped Ruby filename so SketchUp cannot accidentally reload a stale file.

The app displays the exact load command to paste into:

**Extensions → Developer → Ruby Console**

Do not break Ruby syntax when changing embedded templates.

## Build/procurement progress tracking

Implemented in 1.2.0 for users who are physically building a project while some carcasses/parts are already complete and some plywood has already been purchased.

The practical progress workflow has these goals:

### 1. Track completed work

The user needs to be able to check off work that is already done.

Prefer tracking **physical cut parts** as completed rather than merely hiding arbitrary layout sheets. The important semantic is: if a physical part has already been made, it should no longer be part of the remaining-work nesting calculation.

A useful UI may also provide higher-level actions such as marking an entire cabinet/carcass complete, but the underlying behavior must be consistent with individual parts.

Do not make completion state dependent only on a sheet-layout number because layout numbering can change after re-nesting.

Stable identity matters. Design part completion IDs so a reasonable unrelated project edit does not unnecessarily lose all progress.

### 2. Recompute remaining cut layouts

Completed physical parts must be excluded from the **remaining work** nesting/layout calculation.

The user should be able to distinguish at least:

- full project / total required
- completed parts
- remaining parts

Do not destroy the original project definition just because something is complete.

### 3. Track plywood already purchased

Track uncut **full sheets on hand** separately by material.

Examples:

- 3/4 hidden cabinet-box full sheets on hand, for example Birch 3/4
- 3/4 visible/front full sheets on hand, for example Cherry 3/4
- drawer-box full sheets on hand, for example Birch 1/2
- configured back-stock full sheets on hand, for example Cherry 1/4 or Birch 1/4

Then show:

`additional sheets to buy = max(remaining required sheets - full sheets on hand, 0)`

Be explicit that this is inventory of full usable sheets. Do not automatically assume scraps or partially used sheets are equivalent to full sheets unless scrap inventory is intentionally added as a separate future feature.

The user needs a clear **shopping list of sheets still to buy**.

Spare-sheet settings need thoughtful treatment. The UI should make it clear whether purchase recommendations include configured spare sheets. Prefer preserving existing spare-sheet semantics unless there is a strong reason to change them.

### 4. Selective layout printing

The user should not have to print all cut-layout sheets.

Each remaining layout sheet should be individually selectable.

Required behavior:

- select one sheet and print it
- select two sheets and print those two
- select any arbitrary subset and print only that subset
- convenient Select all / Select none controls
- completed/irrelevant sheets should not appear in the default remaining-work print set

Use print CSS or an equivalent simple browser mechanism. Do not add a PDF dependency just for selective printing.

### 5. Persistence

Progress state and full-sheet inventory must persist in:

- project JSON
- browser/local autosave

Old JSON files lacking these fields must load normally with:

- no completed parts
- zero full sheets on hand

### 6. Usability

This is for use in a shop while the user is building cabinets. The progress UI should be obvious and not require understanding nesting internals.

A good workflow would let the user answer:

- What have I already cut/built?
- What pieces are left?
- Which cut-layout sheets do I want to print today?
- How many full sheets of each material do I still need to buy?

Avoid forcing the user to recreate or delete completed cabinets to get an accurate remaining-material plan.

## Implemented 1.2.0 progress model

Do not blindly follow this if inspection of the code reveals a better fit, but this is the intended model:

- Add a stable per-part identity during BOM generation.
- Persist a collection/set of completed part IDs in project state.
- Provide a progress UI derived from generated parts, grouped by cabinet and/or material.
- Keep the existing total BOM available.
- Create a remaining-parts view by filtering completed IDs before nesting.
- Nest remaining parts independently to produce remaining layouts.
- Store full-sheet inventory by material key.
- Compute remaining required sheets from remaining nesting and subtract full-sheet inventory for the buy list.
- Add a selection checkbox/control to each remaining sheet layout.
- Make the print function print only selected sheet cards.

Be careful with quantity > 1 cabinets. Individual physical copies need distinguishable stable IDs if one has been completed and another has not.

Consider how a part ID should behave when a cabinet label changes versus when geometry changes. Geometry changes that invalidate a completed part should not silently pretend the old completed piece still satisfies a newly sized part.

## Testing expectations

Before committing feature work, test at least:

1. Existing/default project still renders with no console errors.
2. Old JSON without progress/inventory loads with empty progress and zero inventory.
3. Marking one physical part complete removes exactly that part from remaining BOM/nesting.
4. Marking a whole cabinet complete removes all its relevant cut parts from remaining nesting without deleting the cabinet from the design/3D model.
5. Unchecking completion restores the parts.
6. Quantity > 1 cabinets can be tracked correctly.
7. Remaining sheet count updates when parts are checked/unchecked.
8. Full-sheet inventory reduces the buy quantity but does not remove required parts/layouts.
9. Inventory greater than remaining required sheets never produces a negative buy quantity.
10. Select one layout and print preview includes only that layout.
11. Select two layouts and print preview includes only those two.
12. Select all/select none behave predictably.
13. Project JSON round-trip preserves progress and inventory.
14. Inches/mm switching does not corrupt progress IDs or inventory.
15. Existing drawer-front alignment behavior does not regress.
16. JavaScript syntax passes.
17. Embedded/generated Ruby syntax passes.
18. GitHub Pages/static-file operation still works without a server or external assets.

If browser automation is available, exercise the UI rather than relying only on static inspection.

## Known future gaps — do not accidentally fold them into 1.2 unless needed

- exposed finished end panels
- multiple cabinet runs/walls
- hardware BOM for slides/hinges/pins/screws
- fillers/scribes
- double doors
- per-cabinet height/depth
- duplicate UI
- hinge drilling templates
- shelf-pin drilling templates
- costs
- wall anchoring
- perspective/GLB/OBJ renderer enhancements
- scrap/remnant inventory

Keep the 1.2 progress feature focused.

## Release hygiene

When feature work is complete:

- update displayed version consistently
- update `README.md`
- update `README.html` when relevant
- update `CHANGELOG.md`
- preserve GitHub Pages behavior
- commit meaningful tested changes
- do not leave one-time helper workflows, temporary chunks, test artifacts, or generated scratch files in the repository
