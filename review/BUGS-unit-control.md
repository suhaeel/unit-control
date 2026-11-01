# Bug review: unit-control

Repo: EssentialBlocks/unit-control (public). Fork: suhaeel/unit-control
Branch: `review/unit-control-bugs` (notes only, main untouched)
What it is: the px/em/% unit switch (`index.js`, 36 lines).

## Findings
| # | Severity | Where | What | Evidence |
|---|---|---|---|---|
| 1 | Medium | index.js:14-16 | ``${unit.value === selectedUnit && "eb-unit-active"}`` renders the text `false` as a class on every inactive button (`eb-unit-control-btn false`). | **Verified (ran it)** |
| 2 | Low | index.js:13 | `<Button>` in a `.map` has no `key`. | **Verified (ran it)** (React warning) |
| 3 | Low | index.js:29 | `unitTypes: PropTypes.shape(...)` is used for an array, so PropTypes should be `arrayOf(shape)`. `selectedUnit` is `isRequired` although callers pass `undefined` for an unset attribute. | From reading the code, not run |
| 4 | Low | index.js:17 | `isSmall` is deprecated in newer `@wordpress/components`. | From reading the code, not run |

## Fix branch
Branch `fix/class-name-and-keys`, pushed to the fork. Builds the class name without `false`, adds `key`, and uses `arrayOf`. Tested by rendering: the inactive buttons had `eb-unit-control-btn false` on master and `eb-unit-control-btn` on the fix branch, and the React key warning is gone.

## Not covered
- Unless a row says "Verified", I only read the code. Components were rendered with React and jsdom using stubbed WordPress components, not inside the real block editor.
- This is a 2021 component that other plugins import. The current `controls` repo (private, open PRs) has replaced it for the standalone plugins, so check there whether the same code still exists.
