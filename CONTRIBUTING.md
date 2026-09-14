# Contributing to CabinetDrafter

Thanks for helping improve CabinetDrafter.

## Bug reports

Please include:

- CabinetDrafter version
- concise reproduction steps
- expected result
- actual result
- project JSON when practical
- screenshot for rendering/layout problems when useful

For material or geometry bugs, include the dimensions you expected and why.

## Pull requests

Keep changes focused. For calculation changes, include a reproducible example and explain the construction assumption being modeled. Avoid silently changing existing project semantics; backward compatibility matters because project JSON files may be used in the shop.

Before submitting:

- verify the application still opens as a standalone `index.html`
- run a JavaScript syntax check if Node is available
- syntax-check generated Ruby if Ruby is available
- test at least one frameless and one face-frame cabinet
- test at least one mixed drawer/open-cubby cabinet
- update documentation for changed construction assumptions

## Design principle

Prefer explicit, visible construction assumptions over hidden guesses. A warning is better than silently inventing a cabinetmaking rule.
