# Construction model and assumptions

CabinetDrafter is configurable, but its generated geometry and BOM must still start from explicit construction assumptions. The default example project reflects the cabinet system used during development.

## Carcasses

- Default carcass thickness: 3/4 in plywood.
- Default visible/front plywood: cherry.
- Default hidden cabinet-box plywood: birch.
- Default drawer-box plywood: birch.
- Pure drawer cabinet boxes use the hidden cabinet-box material by default.
- Mixed drawer/open-cubby, open-shelf, and single-door cabinet boxes use the face/front material by default because those interiors are visible.
- Each cabinet row can override the box/carcass plywood species and the face/front plywood species independently.
- Single-door doors and applied backs use the face/front material so the back matches the door.
- Carcass joints in the development project use pocket screws.
- No automatic edge banding/solid edging is included.

## Face frames

CabinetDrafter supports three front-construction modes per cabinet:

- frameless
- face-frame overlay
- face-frame inset

The face frame is applied to the front of the carcass and consists of two full-height stiles plus top and bottom perimeter rails cut to fit between the stiles (simple butt-joint length accounting). The app separately reports face-frame solid stock by thickness/member width/total cut length.

**Current limitation:** intermediate face-frame rails between individual drawers/cubbies are not generated.

## Drawer boxes

- Default sides/front/back: 1/2 in drawer-box plywood.
- Default bottom: 1/2 in drawer-box plywood.
- Default drawer-box material: birch.
- Bottom fits completely between all four walls; no dado/rabbet is assumed.
- Drawer box height is the outside wall height; bottom thickness does not reduce that entered height.
- Default side-slide clearance: 1/2 in per side / 1 in total.
- The drawer-box gap applies between adjacent drawer boxes.
- Fixed mixed-cubby divider panels separate drawer boxes from open cubbies.

## Open spaces and backs

- Visible open spaces receive applied backs on the outside rear face.
- Default open-space back: 1/4 in plywood matching the visible/front material.
- Open-space backs can instead match the hidden cabinet-box material or be set explicitly to birch/cherry.
- Mixed cabinets receive backs only behind open cubbies; drawer-only areas remain open-backed.
- Adjacent open entries in a mixed cabinet remain separate cubbies and receive fixed divider panels between them.
- A single mixed-cubby back spans full cabinet width and covers clear opening height plus one carcass thickness above and below.
- Adjacent open cubbies share one localized back spanning the combined clear openings, the divider panel(s) between them, and one carcass thickness above and below.
- Open-shelf cabinets use one full-width applied back.
- Single-door cabinets use one full-width applied back in that cabinet's face/front material using the configured back thickness.

## Shelves

- Adjustable shelves are pin-supported.
- Fixed mixed-cubby divider panels are pocket-screwed.
- Explicit open-shelf entries represent clear opening heights from bottom to top.

## Stretchers

Pure drawer open-back carcasses use four stretchers: two flat top stretchers and two vertical rear stretchers. Mixed drawer/open-cubby cabinets use only the two flat top stretchers because the open cubbies receive localized applied backs. Single-door and open-shelf cabinets also use two flat top stretchers and no rear vertical stretchers because they have full applied backs.

## Toe kick

Default base is a separate 2 in-high 3/4 in ladder frame using the hidden cabinet-box material and narrow offcuts, with a visible 1/4 in skin using each cabinet's face/front material. The current BOM treats adjacent cabinets with the same face/front species as continuous runs for toe-kick skin planning.

## Countertop

The development project assumes the countertop is screwed through the top stretchers.

## Hardware

Hardware counts and drilling templates are not yet complete. Development-project assumptions include two concealed European hinges per door and side-mount drawer slides using the configured clearance.

## Build progress and full-sheet inventory

Completed work is tracked as individual physical plywood cut parts. A completed part is removed from the remaining-work nesting, but the cabinet definition and 3D design remain unchanged.

The progress UI groups those physical parts into practical shop assemblies and lists the material species generated for each cabinet and assembly. Whole carcasses mark the cabinet box parts complete without also marking loose fronts or drawer boxes. Whole drawer assembly checkboxes mark the drawer box parts and matching drawer front together. Individual part checkboxes remain available for partial work.

Part completion IDs are based on the cabinet instance, part role, material, and dimensions. Label-only edits should not lose progress, while material or size changes should invalidate affected completed parts.

Full-sheet inventory is counted separately by material and means full, uncut usable sheets only. Scraps and partial sheets are not modeled as inventory in the current workflow.

Sheet layout diagrams are designed for black-and-white shop printing. The sheet header names the material and thickness, part rectangles use species hatch patterns instead of material-color fills, and dashed guides show the full-width cuts between strips. Birch and Cherry can independently use Auto, crosscut-first, or rip-first strip strategies; Auto selects the better practical direction for each material.

Each sheet part label includes the cabinet instance, the part role, and the cut dimensions. Quantity copies therefore remain distinguishable when multiple cabinets share the same drawer geometry.

The selected remaining sheets for printing are saved as stable sheet selections in the project JSON and browser recovery state. Older projects without this field continue to select all current remaining sheets by default.

## 3D and SketchUp labels

Cabinet summary labels, short cabinet width labels, drawer box-height labels, drawer face-height labels, and open-space height labels are independently configurable. Cabinet width labels show the overall cabinet width in the active display unit and are placed near the front bottom of each cabinet.

The browser 3D preview can switch from material-color rendering to a hidden-line print style with white/light-gray faces and black visible edges. This only changes the preview and saved PNG appearance; geometry, BOM, nesting, and SketchUp export data stay unchanged.

## Drawer-front alignment

For automatically sized drawer fronts, reveal lines between adjacent drawers are centered in the physical drawer-box gap. This keeps matching bottom-up drawer stacks aligned across neighboring cabinets. Extra face height is no longer distributed evenly among all drawer fronts, because doing so moved shared reveal lines when an adjacent cabinet had a different upper layout.

A drawer immediately below an open cubby may still have a different upper edge from a drawer that has another drawer above it; the open-cubby divider/clear opening determines the top edge of that final drawer-front run.
