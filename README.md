# AR Museum (markerless)

No marker, no pattern, no target image. Everything runs in the visitor's phone browser, hosted free on GitHub Pages (https is required).

- **Android (Chrome, ARCore phones):** our own WebXR surface detection with tap-to-place. Preview mode and exhibition mode both work.
- **iPhone:** preview mode falls back to Apple's built-in AR viewer (Quick Look) through the model-viewer component. Exhibition mode is Android only for now.

## The pages
| Page | What it is |
|---|---|
| `index.html` | Preview mode: a gallery of single exhibits. On Android up to 4 models can be placed at once (Add model, Remove, Deselect). |
| `exhibition.html` | Exhibition mode: every exhibit of one exhibition is placed around the visitor in a fixed layout. `exhibition.html?e=ID` opens one exhibition. |
| `layout.html` | The gallery maker. Private (not linked from the public pages). Android phone only. |
| `credits.html` | Credits for every public model (from the descriptions in `exhibits.json`). `credits.html?e=ID` lists just the models one exhibition uses. |
| `qr.html` | Prints one entrance QR code per exhibition. |
| `exhibits.json` | The list of models and exhibitions. |
| `models/` | The .glb files. |

## Adding a model
Easiest: open `layout.html` on your phone, open **Models**, then **Add a new model**. Pick the .glb, give it a title and a description or credit, and upload.
It is uploaded to `models/` and added to `exhibits.json` as **unlisted**, so it appears in the camera list but not in the public gallery.
Warns above 15 MB, refuses above 25 MB (GitHub's web upload limit); about 10 to 15 MB or less loads best on phones.
Set its default size and direction in the camera (**Make this the default for this model...**), then tap **Make public** in the Models list when it is ready. **Hide** and **Delete** are there too.

By hand: put a single-file .glb in `models/` and add an entry to `exhibits.json`:
`{"id":"statue","title":"My statue","description":"About it...","model":"models/statue.glb"}`
Optional fields: `"scale":0.2` (1 = as made), `"rotation":-90` (the model's own front-direction fix, in degrees), `"usdz":"models/statue.usdz"` (better iPhone results), `"ground":false` (place the model's origin on the surface instead of its lowest point), `"solid":true` (stops see-through parts showing through each other), `"unlisted":true` (hidden from the public gallery, still opens by direct link).

## Exhibitions
`"exhibitions":[{"id":"movies","name":"Movie characters","description":"Needs a big room","layout":[ ... ]}]`
- With no `?e=`, one listed exhibition opens directly and several show a list. Unlisted exhibitions (`"unlisted":true`) are left out of the list but open from their own link or QR code.
- An older file with a single top-level `"layout"` still works as one exhibition.
- Optional: `"room":{"width":6,"depth":4,"start":"entrance"}` (start can be `entrance`, `corner-right`, `corner-left` or `centre`) shows a "needs a space of about..." note and, in the gallery maker, an outline on the floor with warnings for models outside it.
- Optional: `"warning":"Contains nude sculptures"` shows a notice on the exhibition's start page; visitors must tick "I understand" before the start buttons work. Can also be set in the gallery maker (Content warning).
- Optional: `"recentreAtStart":true` makes Re-centre work only near the start spot (otherwise it asks the visitor to walk back).

Each layout entry: `{"exhibit":"skull","x":-0.7,"y":0,"z":1.8,"rotation":0,"scale":1}`
- `x`: metres to the right of the start spot (negative = left); `z`: metres ahead; `y`: height in metres
- `rotation`: extra turn in degrees (0 = the model faces the start spot); it adds to the exhibit's own `rotation`
- `scale`: extra size multiplier on top of the exhibit's own scale

The start page welcomes the visitor and shows a "Before you start" card (Android and Chrome needed, space needed, tips), a short how-to, a loading bar while models download, and a Credits link. **Tabletop view (miniature)** shrinks the whole exhibition to sit on a table (tap a table or floor; **Move** puts it elsewhere).

Visitors stand on the start spot, aim at the floor about 2 m ahead (the way the exhibition opens) and tap once. **Re-centre** moves the whole exhibition to where they stand and face (hold the phone within about 60 degrees of level). Tapping a model shows its title and description (credit and licence). Accuracy drifts as people walk further from the start spot.

## Gallery maker (`layout.html`)
Open it on an Android phone. Choose New gallery (or an existing one), tap Open camera, stand on the start spot and tap the floor about 2 m ahead.
Then Add model, aim at the floor (or a table) and tap. Tap a model to select it: Size, Turn, Height, Move, Copy, Delete, plus Undo for the last changes.
The selected model shows its real height ("Tall") and a ring around it; the ring turns red when it is outside the room outline.
Tap Lock & save, then **Try it as a visitor** to walk through the unsaved gallery exactly as a visitor sees it (nothing is saved), or **Save gallery to GitHub**.
Saving reads the live `exhibits.json`, changes only what was edited and writes it back. It needs a GitHub fine-grained token for this repository only (Contents: Read and write), pasted once and kept only on that phone.
"Copy the gallery text instead" gives the entry to paste into `exhibits.json` by hand. GitHub Pages takes a few minutes to publish a save.
Galleries can be set to Unlisted, and a gallery can be deleted from the same page.

## Test switches (hidden)
- `exhibition.html?e=ID&anchors=1` pins the exhibition to a real-world anchor (less drift) and shows a small readout (how far the anchor has moved it). `&anchors=0` shows the same readout with anchors off, for comparing. Experimental; without the parameter nothing changes.

## Animated models
Models with built-in animation (e.g. `models/raptor.glb`) play automatically in preview and exhibition mode (Android). The gallery maker shows them in a still pose.
Credits: "Raptor" by Motion Miracles (https://sketchfab.com/3d-models/raptor-0e249b0d0800452f991de063b8fd5a2d), CC-BY-4.0. Keep the credit in the exhibit description.

## Floor fitting and shadows
Models are lowered or raised so their lowest point rests on the surface. If a model looks like it floats or sinks because of how it was built,
add `"ground": false` to its entry in `exhibits.json`. Each model gets a soft shadow on the surface under it.
