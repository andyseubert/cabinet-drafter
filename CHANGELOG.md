# Changelog

## 1.2.7 - 2026-09-14

### Added

- Optional cabinet width labels in the browser 3D preview and SketchUp export.

## 1.2.6 - 2026-09-14

### Changed

- Adjacent open entries in drawer/mixed stacks now remain separate cubbies with fixed divider panels instead of being rejected.
- Contiguous mixed-cubby open runs now use one applied back panel instead of overlapping per-cubby backs.
- Save Project JSON now reuses a remembered project file handle instead of falling back to a new destination picker when write permission needs attention.

## 1.2.5 - 2026-09-14

### Added

- Cabinet table move controls for reordering the cabinet run while preserving cabinet IDs and build-progress state.

## 1.2.4 - 2026-09-14

### Added

- Plywood material choices for visible/front parts, hidden cabinet-box parts, drawer boxes, mixed dividers, and open-space backs.

### Changed

- Single-door cabinets now use the visible/front material for the cabinet box, shelves, door, and applied back.
- Grain preservation now follows grain-sensitive visible parts rather than being tied only to cherry plywood.

## 1.2.3 - 2026-09-14

### Changed

- Mixed drawer/open-cubby cabinets now omit the rear vertical stretchers when localized open-cubby backs are present.
- Single-door cabinets now include a full cherry applied back matching the door/front material.

## 1.2.2 - 2026-09-14

### Added

- Assembly-level progress checkboxes for whole carcasses, toe-kick bases, drawers, doors/fronts, shelves, and toe-kick skin.

## 1.2.1 - 2026-09-14

### Added

- SketchUp export option to keep existing generated cabinet groups instead of replacing the prior `Generated Cabinet Lineup`.

## 1.2.0 - 2026-09-14

### Added

- Build progress tracking for individual physical plywood cut parts.
- Remaining-work BOM/nesting that excludes completed parts while preserving the full project design.
- Full-sheet inventory by material and a shopping list of additional sheets still to buy.
- Selective printing for remaining plywood layout sheets.
- Project JSON/browser autosave persistence for completed parts and full-sheet inventory.

## 1.1.1 - 2026-09-14

### Fixed

- Auto-sized drawer-front reveal lines now follow the physical bottom-up drawer stack instead of spreading spare face height evenly across every drawer in a run.
- Matching lower drawer stacks now align across adjacent cabinets even when one cabinet changes to an open cubby above.
- Browser 3D preview, BOM/cut-list front dimensions, and SketchUp export use the same corrected front geometry.

## 1.1.0 - 2026-09-14

First public CabinetDrafter release.

### Added

- Public project name: CabinetDrafter.
- Per-cabinet frameless, face-frame overlay, and face-frame inset construction.
- Configurable face-frame stile width, rail width, frame thickness/material, overlay, and inset reveal.
- Face-frame solid-stock takeoff and CSV export.
- Built-in interactive 3D preview with PNG export.
- Open-space applied backs for mixed cubbies and open-shelf cabinets.
- Explicit open-shelf clear opening heights with optional top `auto` opening.
- Inch/mm display and entry units.
- Local project JSON save/autosave.
- Plywood sheet nesting and material-purchase summary.
- Optional SketchUp Desktop Ruby export.

### Notes

- Face-frame support in 1.1 uses a perimeter frame only; intermediate rails between individual drawer/cubby openings are not yet generated.
- Sheet nesting finds a valid layout but does not guarantee the globally minimal sheet count.
