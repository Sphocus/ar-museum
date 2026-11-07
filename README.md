# AR Museum (markerless)

No marker, no pattern, no target image. Visitors open an exhibit (the QR code only opens the web page), tap
"Place in your space", move the phone slowly over the floor or a table until a ring appears on the surface,
then tap to place the model. Tap elsewhere to move it. Buttons resize, rotate and reset it.

- Android (Chrome, ARCore phones): our own WebXR surface detection (hit-test) with tap-to-place.
- iPhone: falls back to Apple's built-in AR viewer (Quick Look) through the model-viewer component.
- Must be served over https (GitHub Pages is).

## Add an exhibit
1. Put a single-file .glb in `models/` (under about 10 MB). Models are shown at real size in metres.
2. Add to `exhibits.json`: {"id":"statue","title":"My statue","description":"About it...","model":"models/statue.glb"}
   Optional: "usdz":"models/statue.usdz" for better iPhone results, and "scale":0.2 to start the model smaller (1 = as made, 0.2 = one fifth).
3. Upload the changed files to GitHub.
4. Open `qr.html` on your site and print the exhibit's QR code. It just opens that exhibit's page.

## Old marker-based files to delete from the repository
`markers/` folder, `print.html`, `camera-test.html`, and any `markers-print*.html`. Nothing here uses them.

## Exhibition mode (one room, one QR code per exhibition)
`exhibition.html` places every exhibit of an exhibition around the visitor. Visitors stand on a start spot, aim at the floor
about 2 m ahead (the direction the exhibition opens), and tap once. A Re-centre button moves the whole exhibition to where they
stand and face (hold the phone within about 60 degrees of level).

Exhibitions live in `exhibits.json`:
`"exhibitions":[{"id":"movies","name":"Movie characters","description":"Needs a big room","layout":[ ... ]}]`
- `exhibition.html?e=movies` opens that exhibition. With no `?e=`, one exhibition opens directly and several show a list.
- An older file with a single top-level `"layout"` still works as one exhibition.
- `qr.html` prints one entrance QR code per exhibition.

Each layout entry: {"exhibit":"skull","x":-0.7,"y":0,"z":1.8,"rotation":0,"scale":1}
- `x`: metres to the right of the start spot (negative = left); `z`: metres ahead; `y`: height in metres
- `rotation`: extra turn in degrees (0 = the model faces the start spot). A model's own front-direction fix goes as `"rotation"` on the exhibit itself; it adds to this value.
- `scale`: extra size multiplier on top of the exhibit's own scale
Android only for now. Accuracy drifts as people walk further from the start spot.

## Gallery maker (`layout.html`, not linked from the public pages)
Open it on an Android phone. Choose New gallery (or an existing one), tap Open camera, stand on the start spot and tap the floor
about 2 m ahead. Then Add model, aim at the floor (or a table) and tap. Tap a model to select it: Size, Turn, Height, Move, Copy, Delete.
Tap Lock & save, then Save gallery to GitHub. It reads the live `exhibits.json`, changes only the `exhibitions` list and writes it back.
Saving needs a GitHub fine-grained token for this repository only (Contents: Read and write), pasted once and kept only on that phone.
"Copy the gallery text instead" gives the entry to paste into `exhibits.json` by hand. GitHub Pages takes a few minutes to publish a save.

## Animated models
Models with built-in animation (e.g. models/raptor.glb) play automatically in preview, tap-to-place and exhibition mode (Android).
Credits: "Raptor" by Motion Miracles (https://sketchfab.com/3d-models/raptor-0e249b0d0800452f991de063b8fd5a2d), CC-BY-4.0 - keep the credit in the exhibit description.

## Floor fitting
Models are lowered/raised so their lowest point rests on the surface. If a model looks like it floats or sinks because of how it was built,
add `"ground": false` to its entry in exhibits.json to place its origin point on the surface instead (the old behaviour).
