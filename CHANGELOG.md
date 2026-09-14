# Changelog

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
