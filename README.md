# CabinetDrafter

**Free cabinet design, material planning, nesting, and 3D.**

CabinetDrafter is a free, offline-first browser cabinet planner for frameless and face-frame cabinet construction. It combines cabinet layout, drawer/open-space sizing, material takeoff, sheet nesting, an interactive 3D preview, and optional SketchUp export in a single HTML file.

No account, web service, or installation is required for the core app. Open `index.html` in a modern desktop browser.

![CabinetDrafter 3D preview](screenshots/3d-preview.png)

## What it does

- Frameless, face-frame overlay, and face-frame inset cabinets on a per-cabinet basis
- Drawer cabinets, single-door cabinets, mixed drawer/open-cubby cabinets, and open-shelf cabinets, with reorder controls for the run
- Explicit drawer heights and clear shelf/cubby opening heights
- Inches or millimeters for entry/display while keeping canonical geometry stable internally
- Applied backs behind open spaces and single-door cabinets
- Role-based plywood material choices for visible/front parts, hidden cabinet-box parts, drawer boxes, open-space backs, and mixed dividers
- Drawer-box sizing with configurable slide clearance
- Auto-sized drawer fronts keep matching bottom-up drawer stacks aligned across adjacent cabinets
- Plywood BOM, detailed cut list, grain-aware sheet nesting, kerf, edge trim, and spare-sheet allowance
- Build progress tracking by cabinet/drawer assembly or physical plywood part, with remaining-work nesting and selective layout printing
- Full-sheet inventory by material with a shopping list of additional sheets still to buy
- Separate face-frame solid-stock takeoff by thickness and member width
- Interactive browser 3D preview with orbit/zoom/pan and PNG export
- Project JSON save/load with browser autosave
- Optional SketchUp Desktop Ruby export, with a choice to replace or keep previous generated groups

## Try it

When GitHub Pages is enabled for this repository, the live app will be available at:

`https://andyseubert.github.io/cabinet-drafter/`

You can also download the repository and open `index.html` directly. The app does not require a server or internet connection once downloaded.

## Quick start

1. Open `index.html`.
2. Choose inches or millimeters.
3. Add, edit, or reorder cabinets in the cabinet table.
4. For each cabinet choose **Frameless**, **Face frame - overlay**, or **Face frame - inset**.
5. Review the 3D preview and sizing warnings.
6. Review the material BOM and sheet layouts before purchasing material.
7. As you build, check off completed cut parts and enter full sheets already on hand.
8. Use **Save Project JSON** to keep a portable project file.

### Drawer / mixed stack syntax

`D` means Drawer. `O` means **Open** - the letter O ("oh"), not zero.

```text
D7, D7, D9          three drawers
D9, O15             one drawer, then a 15-inch clear open cubby
D9, Open 15, D7     drawer, open cubby, drawer
```

A legacy all-number list such as `7, 7, 9.5` remains an all-drawer list. Once `D` is used, a bare number is treated as an open space, so `D9, D9, 15` means two drawers and a 15-unit open cubby.

### Open-shelf heights

Open-shelf cabinets use the same field for clear opening heights from bottom to top:

```text
9.5, 12, auto
```

`auto` may be used for the final/top opening to consume the remaining clear height. Leaving the field blank falls back to evenly spaced shelves using the Shelves count.

## Drawer-front alignment

Auto-sized drawer-front reveal lines are tied to the physical drawer-box stack. The reveal between adjacent drawers is centered in the actual drawer-box gap. This keeps a shared bottom-up sequence such as `D7.5, D7.5` aligned across neighboring cabinets even when one cabinet changes to an open cubby above.

## Face frames

Each cabinet can be:

- **Frameless** - the original full-overlay behavior
- **Face frame - overlay** - door/front overlays the face-frame opening by the configured amount
- **Face frame - inset** - door/front fits inside the face-frame opening with the configured reveal

Global face-frame settings control stile width, rail width, frame thickness, material, overlay, and inset reveal. Face-frame members are treated as **solid linear stock**, not plywood sheet parts, and are summarized separately in the BOM.

**Current limitation:** face frames are perimeter frames only: two full-height stiles plus top and bottom rails. Intermediate face-frame rails between individual drawers or cubbies are not generated yet.

## Build progress and inventory

CabinetDrafter tracks completed work as individual physical plywood cut parts. The progress UI also groups those parts into practical assemblies, so you can check off a whole carcass, toe-kick/base, drawer, door/front, or shelf set without clicking every piece. Checking off work does not delete or change the cabinet design; it only removes those parts from the remaining-work nesting and remaining cut-list CSV.

Each cabinet instance has a stable internal ID so progress survives label changes and ordinary editing. Part IDs include the part geometry and material, so a completed part is not silently reused for a newly sized or newly specified part.

The sheet inventory fields are for **uncut full usable sheets on hand** by material. Scraps and partial sheets are intentionally not treated as full sheets. The shopping list calculates:

```text
additional sheets to buy = max(remaining nested sheets + spare sheets - full sheets on hand, 0)
```

The remaining plywood layouts are individually selectable before printing, with Select all and Select none controls.

## Material calculations

The **Material choices** section keeps visible and hidden work separate without pretending the app knows every sheet good in the lumber rack. Choose the plywood for faces/doors/drawer fronts and visible cabinet parts, the hidden cabinet-box plywood for pure drawer carcasses and toe-kick structure, and the drawer-box plywood independently. Mixed-cabinet dividers and open-space backs can either match one of those roles or be set explicitly to birch or cherry.

Single-door cabinets use the visible/front material for the cabinet box, shelves, door, and applied back. Pure drawer cabinets keep using the hidden cabinet-box material by default.

CabinetDrafter creates a valid, non-overlapping sheet layout using the selected sheet size, edge trim, kerf, and grain constraints. The sheet count is a practical purchase-planning number for the layout it found, but the nesting algorithm does **not** claim to find the mathematical minimum number of sheets in every case.

Face-frame solid stock is reported as net cut length grouped by material, thickness, and member width. Add waste based on available board lengths, defects, milling allowance, and your shop process.

Before cutting, verify dimensions against your hardware, actual material thickness, installation site, and chosen construction method.

## 3D preview

The built-in preview requires no SketchUp and no network connection. It can:

- orbit, zoom, and pan
- switch among isometric/front/left/right/top views
- hide/show fronts, drawer boxes, and backs
- display optional cabinet/drawer/open-space labels
- save the current view as PNG

## SketchUp export

SketchUp is optional. If you have SketchUp Desktop with Ruby support, use **Download Ruby script**, then in SketchUp Pro open **Extensions → Developer → Ruby Console** and paste the exact `load ...` command shown by CabinetDrafter.

By default, the generated Ruby replaces the prior `Generated Cabinet Lineup` group. Turn off **Replace prior Generated Cabinet Lineup in SketchUp** before downloading if you want SketchUp to keep existing generated cabinets and add the new export as a separate timestamped group.

SketchUp for Web does not execute the Ruby generator; the in-browser 3D preview exists so CabinetDrafter remains useful without SketchUp.

## Privacy

CabinetDrafter has no server component. The core app does not upload cabinet/project data anywhere. Browser storage and project JSON files remain local to the user's machine. GitHub Pages, if used to host the static app, only serves the application files.

See `PRIVACY.md`.

## Project files

- `index.html` - complete application
- `README.html` - friendly standalone getting-started guide
- `README.md` - project documentation
- `docs/construction.md` - current construction assumptions and scope
- `examples/mixed-construction-demo.json` - example project using frameless and face-frame cabinets
- `CHANGELOG.md` - release history
- `CONTRIBUTING.md` - bug report and pull request guidance
- `LICENSE` - MIT license

## Contributing

Issues and pull requests are welcome. For geometry or material bugs, please include the CabinetDrafter version, project JSON if practical, expected result, actual result, and a screenshot if the problem is visual.

See `CONTRIBUTING.md`.

## License

MIT License. See `LICENSE`.

## Trademark note

CabinetDrafter is an independent open-source project. SketchUp is a trademark of Trimble Inc. CabinetDrafter is not affiliated with or endorsed by Trimble.

## Important disclaimer

CabinetDrafter is a planning aid, not structural engineering or a substitute for verifying your build. Material dimensions, hardware clearances, installation conditions, and safe construction remain the builder's responsibility. **Measure and verify before cutting.**
