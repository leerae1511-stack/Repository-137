# Tool Pocket Generator V137 — Release Notes

## Provenance
- Built directly from `Tool_Pocket_Generator_V136_FINAL.zip`.
- V136 was the sole source build for this release.

## Problem addressed
In V136, the Length/Width input fields were also used to size the orange workspace rectangle. When those fields were changed to enlarge the workspace, Close Profile could mistake those workspace values for the project's saved final dimensions. The Geometry Check then reported the workspace size as the master measurements.

## V137 fix
- Added a separate `projectFinalDims` state for the project's final Length/Width.
- On JSON import, initialize it from the saved project dimensions (or from `targetLength` / `targetWidth` for older JSON files).
- On New Tool, initialize it from the default target dimensions.
- When **Set dimensions from physical grid** succeeds, update the separate final dimensions from that measured outline.
- Close Profile restores the final-dimension fields from `projectFinalDims` but leaves `workspaceDims` untouched, so the orange rectangle can remain enlarged.
- Saved tool records and autosave now retain the separate final dimensions. The established `targetLength` / `targetWidth` JSON fields continue to represent final project dimensions.

## Process / order
1. Import JSON or start New Tool.
2. Set/enlarge the workspace rectangle as needed; workspace dimensions remain separate.
3. Confirm the physical photo grid and run Detect Tool Outline as required.
4. Edit and confirm the profile.
5. If using physical-grid measurement, run **Set dimensions from physical grid** to establish/update final dimensions.
6. Close Profile; final dimensions are restored independently of the workspace.
7. Run Geometry Check, then export JSON/STL.

## What was not changed
- Curve/Straight point selection or curve algorithms.
- Finger Relief behaviour.
- The physical-grid confirmation or outline-detection algorithms.
- Geometry Check rules.
- STL mesh/export algorithm, apart from the version label.
- Existing profile point coordinates and IDs during Close Profile.

## Verification performed
- Confirmed V136 ZIP was used directly as source.
- Checked the state initialization, import fallback, physical-grid update, Close Profile handling, autosave and saved-tool serialization edits in the delivered HTML.
- Confirmed Close Profile no longer assigns `workspaceDims={L,W}`.
- Confirmed ZIP integrity and exactly one README.

## Test status
Not tested in a live browser or on iPad/Safari. Retest required:
- Import the same JSON and note its saved target dimensions.
- Enlarge the orange workspace to 150 × 45 mm.
- Edit/confirm the outline and close it.
- Confirm the orange rectangle remains 150 × 45 while master measurements return to the imported saved dimensions.
- Run Geometry Check and confirm dimensions remain correct.
- Export JSON, re-import it, and verify the final dimensions persist.
- Verify outline shape and STL export are unchanged.
