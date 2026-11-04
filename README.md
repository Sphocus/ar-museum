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
   Optional: "usdz":"models/statue.usdz" for better iPhone results.
3. Upload the changed files to GitHub.
4. Open `qr.html` on your site and print the exhibit's QR code. It just opens that exhibit's page.

## Old marker-based files to delete from the repository
`markers/` folder, `print.html`, `camera-test.html`, and any `markers-print*.html`. Nothing here uses them.
