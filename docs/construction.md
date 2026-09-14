# Construction model and assumptions

CabinetDrafter is configurable, but its generated geometry and BOM must still start from explicit construction assumptions. The default example project reflects the cabinet system used during development.

## Carcasses

- Default carcass thickness: 3/4 in plywood.
- Pure drawer/single-door defaults: birch carcass.
- Mixed drawer/open-cubby and open-shelf defaults: cherry carcass for visible interiors.
- Carcass joints in the development project use pocket screws.
- No automatic edge banding/solid edging is included.

## Face frames

CabinetDrafter 1.1 supports three front-construction modes per cabinet:

- frameless
- face-frame overlay
- face-frame inset

The face frame is applied to the front of the carcass and consists of two full-height stiles plus top and bottom perimeter rails cut to fit between the stiles (simple butt-joint length accounting). The app separately reports face-frame solid stock by thickness/member width/total cut length.

**1.1 limitation:** intermediate face-frame rails between individual drawers/cubbies are not generated.

## Drawer boxes

- Default sides/front/back: 1/2 in birch plywood.
- Default bottom: 1/2 in birch plywood.
- Bottom fits completely between all four walls; no dado/rabbet is assumed.
- Drawer box height is the outside wall height; bottom thickness does not reduce that entered height.
- Default side-slide clearance: 1/2 in per side / 1 in total.

## Open spaces and backs

- Visible open spaces receive applied backs on the outside rear face.
- Default back: 1/4 in cherry, configurable to birch/cherry and thickness.
- Mixed cabinets receive backs only behind open cubbies; drawer-only areas remain open-backed.
- A mixed-cubby back spans full cabinet width and covers clear opening height plus one carcass thickness above and below.
- Open-shelf cabinets use one full-width applied back.

## Shelves

- Adjustable shelves are pin-supported.
- Fixed mixed-cubby divider panels are pocket-screwed.
- Explicit open-shelf entries represent clear opening heights from bottom to top.

## Stretchers

Default open-back drawer/door carcass structure uses four stretchers: two flat top stretchers and two vertical rear stretchers. Open-shelf cabinets with full backs use two flat top stretchers and no rear vertical stretchers.

## Toe kick

Default base is a separate 2 in-high 3/4 in birch ladder frame using narrow offcuts, with a visible 1/4 in cherry skin across the installed run. The current BOM treats the listed cabinets as one continuous run for toe-kick skin planning.

## Countertop

The development project assumes the countertop is screwed through the top stretchers.

## Hardware

Hardware counts and drilling templates are not yet complete. Development-project assumptions include two concealed European hinges per door and side-mount drawer slides using the configured clearance.

## Drawer-front alignment

For automatically sized drawer fronts, reveal lines between adjacent drawers are centered in the physical drawer-box gap. This keeps matching bottom-up drawer stacks aligned across neighboring cabinets. Extra face height is no longer distributed evenly among all drawer fronts, because doing so moved shared reveal lines when an adjacent cabinet had a different upper layout.

A drawer immediately below an open cubby may still have a different upper edge from a drawer that has another drawer above it; the open-cubby divider/clear opening determines the top edge of that final drawer-front run.
