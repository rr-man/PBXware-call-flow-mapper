# Changelog — PBXware Call Flow Map Creator

All notable changes to the call flow map. Dates are 2026-07-13 unless noted;
the map was developed iteratively in a single working session.

## [2.44.1] — 2026-07-24
### Changed
- Wider horizontal-layout geometry: node boxes 200→230px and column pitch
  250→300px, so labels get more room and connectors longer, cleaner runs. A
  five-level map grows from 1310px to 1560px wide and fills a 4K workspace at
  a lower magnification with visibly more air. Vertical layout is unchanged.
  The wider canvas flows into the SVG/PDF exports; the Visio export keeps its
  own inch-based spacing. This is an intentional renderer change (the first
  since 2.35.0) — the renderer baseline was refreshed accordingly.

## [2.44.0] — 2026-07-24
### Added
- The Call Map Diagram workspace now auto-fits the map to the window width
  (capped at 1.75× so small maps don’t over-inflate) — on large/4K displays the
  diagram fills the workspace instead of sitting at fixed pixel size, and stays
  crisp at any level since it’s vector.
- Zoom controls for the workspace, in two places driving one shared state: a
  cluster in the Summary actions row (next to Download/Transcribe/Clear) and a
  floating pill pinned top-right over the diagram that stays visible while
  scrolling. Buttons: − / % readout / + / Fit / 100%. Fit re-computes on window
  resize; re-rendering (new DID or layout switch) returns to Fit. The expanded
  view keeps its own existing zoom.

## [2.43.5] — 2026-07-24
### Changed
- Transcribe-audio window: the Deepgram-only toggles (Smart formatting,
  Punctuate, Diarize) now hide entirely when OpenAI is the provider, replaced
  by a one-line caption explaining that formatting and punctuation are
  automatic there and diarization isn’t available. The toggles and the user’s
  previous choices reappear when switching back to Deepgram. Embedded
  transcriber → v1.27.2.

## [2.43.4] — 2026-07-24
### Fixed
- Dark mode: the selected DID (and Queue/Conference) button in the pickers was
  nearly invisible — navy on navy — because no dark-mode selected style existed.
  It now shows an amber ring with amber sub-text.
- Dark mode: the “Transcribe audio” attention highlight never rendered — a
  general dark-mode button rule overrode the amber fill — leaving only a faint
  pulse. The button now shows the intended amber fill (dark text) with the
  pulsing ring.

## [2.43.3] — 2026-07-24
### Changed
- The two “Download template” tooltips now explain the key difference between
  the samples’ Operation Times: Property Management uses one weekly schedule
  (a single Mon–Fri row with an after-hours path), while Office Dental uses
  per-day custom rows — one per weekday with its own Custom Destination — plus
  a closed path.

## [2.43.2] — 2026-07-24
### Changed
- Renamed the sample workbooks' “Download” link to “Download template” in the
  “Load a file” view.

## [2.43.1] — 2026-07-24
### Added
- Tooltips on the sample workbooks' “Attach” and “Download” controls in the
  “Load a file” view, explaining that Attach loads the sample into the tool and
  Download saves the .xlsx. The styled tooltip now also works on any control
  carrying a data-tip, not just the small “i” markers.

## [2.43.0] — 2026-07-23
### Added
- Visio (.vsdx) export, offered as a checkbox in the download options. It
  produces an editable Visio drawing — one shape per node (colour-coded by
  type) with glued dynamic connectors between them, auto-laid-out — so you can
  rearrange and restyle it in Microsoft Visio. Built entirely in the browser:
  a bundled Visio template supplies the fixed package parts and only the page
  contents are generated, then zipped with the existing packer.
  Notes: shapes use flat fills rather than the map's rounded/gradient styling,
  and Visio applies its own connector routing, so the initial layout can differ
  from the on-screen diagram until Visio re-flows it.

## [2.42.3] — 2026-07-23
### Fixed
- Sample-workbook Download now works inside an iframe. The new-tab approach
  failed because the file (a blob) belongs to the iframe's context and a
  separate tab can't read it. When embedded, each sample Download link is now a
  real download link the user clicks directly — the file stays in-context and
  the click is a genuine gesture the browser allows. Standalone still uses the
  native Save dialog.

## [2.42.2] — 2026-07-23
### Fixed
- Embedded (iframe/Confluence) downloads now open in a new browser tab instead
  of an in-frame pop-up. The previous “download ready” card was a fixed overlay
  that, inside an auto-height iframe, centred on the full content height and
  landed off-screen — so nothing appeared. When embedded, the tool now reserves
  a new tab during the click (so it isn't pop-up-blocked) and writes an
  auto-starting download page into it, which escapes the frame entirely. If the
  browser blocks pop-ups, it falls back to the in-frame link card. Standalone /
  GitHub Pages use is unchanged (native Save dialog).

## [2.42.1] — 2026-07-23
### Fixed
- Downloads did nothing when the tool is embedded in an iframe (e.g. Confluence).
  The native Save dialog is disallowed in cross-origin frames and the silent
  fallback download was blocked by the frame sandbox. When embedded, the tool
  now skips the native picker and shows a “Your download is ready” card with a
  Download link the user clicks — a real click gets through where a scripted one
  is blocked. Applies to sample workbooks and map exports. Standalone (non-embed)
  use is unchanged. Embedded transcriber → v1.27.1 (its .txt download now opens
  in a new tab for the same reason).

## [2.42.0] — 2026-07-23
### Added
- Transcribe-audio window: a transcript can now be attached to more than one
  IVR. Each result has an “+ Add another IVR” button that adds an extra IVR
  dropdown, and Attach to map posts the transcript to every IVR chosen (useful
  when several IVRs share the same greeting). Embedded transcriber → v1.27.0.

## [2.41.0] — 2026-07-23
### Added
- Map a DID now opens with a blue notification prompting the user to select a
  call route — “choose one DID from the options below to trace it” — with an
  info tooltip explaining what a call route is.
- After a call flow map is generated, the “Transcribe audio” button pulses amber
  to signal it's the next step; the highlight clears once it's opened.

## [2.40.0] — 2026-07-23
### Added
- “Attach” button on each sample workbook in the upload view: it decodes the
  embedded sample and loads it straight into the tool through the same path as
  a dropped file — no download or re-upload needed. “Download” is still there
  for anyone who wants the .xlsx itself.

## [2.39.0] — 2026-07-23
### Changed
- Landing flow: the sample workbooks no longer appear on the landing card. The
  “Map my system” card now just opens the upload view, which is headed “Load a
  file to view your call flow map” and lists the samples beneath the file drop.
- Renamed the general sample from “Lakeside Property Management” to “Property
  Management” (label, tenant and greeting names).
- In that sample the DID that previously terminated at Deny Access now routes to
  a queue named “Agents” (the three sample agents), demonstrating an agents
  destination instead of a blocked call.
### Added
- Greeting transcripts on the demo IVRs so the greeting callout shows out of the
  box: the Property Management menus and the Office Dental daytime, per-day,
  doctor-selection and after-hours IVRs.

## [2.38.2] — 2026-07-23
### Changed
- Second built-in sample renamed to “Office Dental” (label, filename and the
  workbook’s tenant name).

## [2.38.1] — 2026-07-23
### Changed
- Both built-in samples are now labelled “(Demo)” in the picker and in their
  company/tenant names, so a rendered map is clearly a demo.
- Hardened the Office Dental sample scrub: removed all staff names, the dial
  group that contained personal names, every MAC address, extension PINs,
  email addresses, the tenant domain and the trunk/site identifiers, and
  replaced the external number wherever it was stored (text or numeric). The
  per-day Operation Times structure is unchanged.

## [2.38.0] — 2026-07-23
### Added
- Second built-in sample workbook: “Office Dental”, which demonstrates
  per-day custom Operation Times (a different open-hours destination for each
  weekday). The landing screen now lists both samples — Lakeside (general)
  and Office Dental (per-day hours) — each with a short description.
- The Office Dental sample is a sanitised copy: staff names, emails, PINs, MAC
  addresses and the external phone number are replaced with fictional values;
  the per-day Operation Times structure is preserved.

## [2.37.1] — 2026-07-23
### Changed
- Per-day Operation Times now handle days with different hours: the gate
  subtitle shows the common window and a branch only carries its own window
  when it differs from that common one, so the usual same-hours case stays
  uncluttered. Per-day times are also run through the clock formatter so
  non-standard time encodings display correctly.

## [2.37.0] — 2026-07-23
### Added
- Operation Times now supports per-day custom destinations. When the Operation
  Hours sheet has a Custom Destination column with one row per weekday, the OT
  gate fans out into a branch per day (Mon→…, Tue→…) landing on that day’s
  destination, plus a single closed branch to the after-hours destination.
  Days without a row — and any time outside the window — are treated as closed.
  Day labels are normalised (Tues→Tue, Th→Thu) and the shared window is shown
  in the gate subtitle so the branch labels stay short.
- Purely additive: a workbook with no Custom Destination column keeps the
  existing single open/closed OT gate. Applies at DID level and at IVR / Dial
  Group / ERG / Queue level.

## [2.36.0] — 2026-07-22
### Added
- IVR sheets may include an optional “Greeting Transcript” column (placed after
  the option/timeout columns). When present it fills the greeting-transcript
  callout under that IVR, so a greeting can be shown without running the
  transcriber; an attached transcript still takes precedence.
- The built-in sample workbook now carries a greeting transcript on the Main
  Menu IVR, so the greeting callout is demonstrated out of the box.

## [2.35.0] — 2026-07-22
### Added
- Conference bridges are now a full destination type. The Conference sheet is
  parsed (Name / Conference Number), conference destinations are resolved and
  drawn as their own orange nodes wherever a DID or IVR routes to one, and
  they count as a local destination (active-route filtering).
- “Map a Conference” picker section in the summary, a Conference type in the
  Map-a-DID filter, and a Conferences group in the download picker.
- The built-in sample workbook now includes three conference bridges and an
  IVR option that routes into one, so the feature is demonstrated out of the box.

## [2.34.2] — 2026-07-22
### Changed
- Map-a-DID filter chips now show each destination type’s word inside a box
  tinted with that type’s colour (text in the darker shade), replacing the
  small swatch-beside-label style. Checkbox and behaviour are unchanged.

## [2.34.1] — 2026-07-22
### Fixed
- Transcribe-audio window opened tiny when the tool was running inside an
  embed/iframe (e.g. Confluence). The v2.30.0 embed rule that caps the
  expanded-map overlay was also hitting the transcribe modal, collapsing its
  full-height iframe. That cap is now scoped to the expanded-map overlay only,
  and the transcribe modal keeps a definite full-frame height so its panel
  fills the window again.

## [2.34.0] — 2026-07-22
### Changed
- Replaced the built-in sample workbook with fresh, fully fictional demo data
  (“Lakeside Property Management”, reserved 555-01xx numbers — no real client
  data reused). The new sample exercises the whole tool: an operation-hours
  gate with after-hours routing, nested IVRs, a dial group, a support queue
  with three named agents, deny-access and unassigned DIDs, voicemail and an
  ERG.

## [2.33.0] — 2026-07-22
### Changed
- The “Show DIDs routed to” filter now keys off every destination type a DID’s
  call flow reaches — immediate or downstream — not just its first hop. A DID
  that reaches a queue only through an IVR is now selectable under Queue, and
  a Queue checkbox appears whenever any DID reaches a queue. A DID is listed
  if any of the types it reaches is ticked; Deny-access DIDs stay tagged Deny.

## [2.32.2] — 2026-07-22
### Added
- The download “specific destination” picker now has a Queues group (listed
  below IVRs), so individual queue maps can be exported. Selecting a queue
  exports that queue as the root with its agent members.

## [2.32.1] — 2026-07-22
### Fixed
- Queue members are now read from the paired “Members Name N / Agent” columns
  of the Queues export. The 4th column (“Agents”) is only the member count, so
  the parser was turning it into a single bogus box (e.g. “Agent 5”). Each
  member now shows its real agent number and name taken straight from the
  sheet, with agent/extension name fallbacks. Handles any number of members,
  count-0 queues (no members), quoted values, and legacy comma-list exports.

## [2.32.0] — 2026-07-22
### Added
- “Map a Queue” picker section, placed directly below “Map a single IVR”
  in the summary. It lists every queue; selecting one maps that queue as the
  root and draws its agent members (each with agent number and name).

## [2.31.3] — 2026-07-22
### Changed
- Queue member names resolve more reliably. Each member is matched to its
  agent by agent number or bind extension, and the displayed name is the
  agent’s own name or — when the agent has no name filled in (common in
  Contact Center exports) — the name of the extension it is bound to. Every
  member box shows the agent number with a name beneath it.

## [2.31.2] — 2026-07-22
### Fixed
- The Agents sheet is now read even when the export omits its header row
  (same case as the Extension sheet), so the first agent is no longer
  skipped. This was the likely cause of a queue member showing its agent
  number with a blank name.

## [2.31.1] — 2026-07-22
### Fixed
- Queue members are now resolved as agents via the Agents tab. Each member
  listed on a queue is matched by its agent number or its bind extension, and
  the node shows the agent number and the agent name (rather than being
  treated as a bare extension). Penalty suffixes on the member list are
  ignored.

## [2.31.0] — 2026-07-22
### Added
- “Deny access” is now one of the routing types in the Map-a-DID filter, so
  DIDs routed to Deny access can be shown or hidden like any other type.
### Changed
- Queue members are now drawn as individual nodes on the call-flow diagram
  (each agent/extension in its own box, with the ring strategy on the first),
  matching how Dial Group members are shown — instead of a single summary box.

## [2.30.0] — 2026-07-22
### Added
- Embed / iframe mode for putting the tool inside Confluence (or any page).
  It is detected automatically when the page is framed, or forced with an
  “#embed” (or “?embed”) URL flag. In this mode the layout goes fluid, the
  banner shrinks, and the page reports its height to the parent so the frame
  grows to fit the map instead of clipping it.
- Height beacon: the page posts a `{ type: 'cfmap:height', height }` message
  to the parent on load, resize and content change. A tiny listener on the
  host page (see README) resizes the iframe. The MIT iframe-resizer v4.3.9
  child is also bundled and started in embed mode, so a Confluence page that
  already runs the iframe-resizer parent auto-sizes with no extra code.
- Manual size overrides for when no auto-resize is available: `#embed&h=1400`
  (and `&w=1200`) set an explicit frame height/width and reported height.
### Changed
- In embed mode, modal heights are capped in pixels rather than viewport
  units, so the expanded-map and dialog boxes don’t balloon once the iframe
  auto-grows to the content height.

## [2.29.0] — 2026-07-21
### Changed
- The “Map a DID” checkboxes now filter the DID list itself rather than the
  rendered map. Each checkbox is a routing type (IVR, Extension, Voicemail,
  Dial Group, ERG, Queue); a DID appears in the list only when the type it
  is routed to is ticked — e.g. ticking IVR alone lists only DIDs routed to
  an IVR. All types are ticked by default, and only types actually used by
  the DIDs get a checkbox. DIDs with no destination set are always listed.
  Selecting a DID still renders its full map unchanged.

## [2.28.0] — 2026-07-21
### Changed
- A menu (IVR) reached from more than one place in the same map is now
  expanded in full at each spot, so every branch is a complete map on its
  own — e.g. an after-hours menu’s options now show their full sub-menus
  instead of an “IVR N — shown above” pointer. Genuine loops (a menu that
  routes back into itself or into a menu already on the path) are still
  detected and drawn as a “Repeat” node, so maps stay finite.

## [2.27.1] — 2026-07-21
### Fixed
- Destination labels that lead with a type word — e.g. “IVR - 192”,
  “Voicemail - 110”, “Extension 101 - Desk 1” — are now parsed correctly.
  Previously only a number at the very start of the label was read, so
  those options were dropped or mislabelled (a common symptom was an
  after-hours IVR showing a generic “Remote Access” node or missing a
  branch entirely).
- Resolution now honours an explicit type word in the label, so an option
  and a voicemail that share the same number resolve to the intended one.
- A Dial Group’s Last Destination is now added to the call flow and resolved
  to its real type (IVR, extension, voicemail, …) instead of always being
  drawn as a voicemail.
- The Extension sheet is now read even when it has no header row (some
  exports omit it); the first extension is no longer skipped.

## [2.27.0] — 2026-07-21
### Added
- IVR is now one of the local-destination filter types in “Map a DID.”
  Unticking it hides nested sub-menus (an IVR reached as an option from
  another IVR) and any branch that leads only into them; the DID’s primary
  menu is always kept.
### Changed
- The filter now shows only the destination types actually routed from the
  selected DID. The checkbox row updates each time you pick a DID; each
  type’s ticked/unticked state is remembered as you switch between DIDs.

## [2.26.0] — 2026-07-21
### Added
- “Map a DID” now has a local-destination filter: a row of checkboxes
  (Extension, Voicemail, Dial Group, ERG, Queue) under the heading, all
  ticked by default. Unticking a type hides those endpoints in the DID map
  and drops any route that leads only to them; a “Clear all / Check all”
  button and a warning when nothing is selected round it out. Only the
  destination types actually present in the workbook get a checkbox.
  The filter applies to the Map-a-DID view only; other views and exports
  are unaffected.

## [2.25.1] — 2026-07-21
### Fixed
- Download could silently do nothing on offline (file://) pages and in
  browsers without the Save-As picker (Firefox/Safari). The fallback path
  asked for a file name with a prompt, and if that prompt was blocked or
  dismissed the download was cancelled with no file and no notification.
  The leftover name prompt (the File name field was removed in 2.25.0) is
  gone; the fallback now downloads straight away using the automatic name.

## [2.25.0] — 2026-07-21
### Added
- The Download options dialog now has a Basic / Advanced split. Basic shows
  the common choices; an “Advanced options” toggle reveals the rest.
- Tooltips on every option in the dialog (format, PDF layout, rendering,
  orientation), matching the existing “i” hover style.
### Changed
- Basic view shows: All active information with routes, PDF + Transcripts,
  Fit to page, and Vector rendering.
- Advanced view adds: Download all routes, A specific destination (with its
  picklist), SVG + Interactive HTML, Actual-size PDF layout, and Orientation.
### Removed
- The File name field. Downloads use the automatic name; the browser’s Save
  As dialog still lets you rename on the spot.
- The PDF rendering “Image” option. Vector is now the only mode (shown as a
  badge) and is always used.

## [2.24.1] — 2026-07-21
### Added
- A centred “Generating your download…” card with an animated spinner now
  appears while a multi-format export (PDF/ZIP) is being built, then is
  replaced in place by the completion notice.
### Changed
- The “Download complete” notice is now centred on screen instead of anchored
  to the bottom.

## [2.24.0] — 2026-07-21
### Added
- Live zoom percentage readout in the expanded map view and on the reference
  map toolbar (100% = natural size; after Fit it shows the actual fit value).
### Changed
- Zoom now resizes the drawing instead of CSS-scaling it, so the scroll area
  grows with the zoom — nothing gets clipped at any zoom level, in both the
  expanded map view and the reference map.
- Unified zoom step of 1.25x per click; Zoom out always steps down exactly
  one level. Fit (expanded view) and Reset (reference map) are the only
  resets.
- Zoom limits with visible feedback: max 400% everywhere; min = the fit
  scale in the expanded view and 30% on the reference map. Buttons gray out
  at the limits.
- Stepping keeps the visible centre anchored instead of drifting toward the
  top-left corner.
- Resizing the window no longer resets the expanded-view zoom; it only
  re-clamps the level if the fit minimum changed.
- Zoom steps are instant: the step animation was removed because animating
  the size desynchronises the scroll-position anchoring.

## [2.23.0] — 2026-07-17
### Changed
- Every IVR (and any node with options) now draws a single shared trunk line
  to all of its options instead of each option re-drawing the full connector.
  Options keep their own short stub with the arrowhead and per-route styling.
  Standard routes look identical; loop-back/reference routes now show a
  neutral trunk with only their own stub dashed amber, and in the reveal
  animation the trunk appears together with its IVR.
- Live Trace is unaffected: each option carries an invisible full-route guide
  path, so the trace dot travels box-to-box along the same course as before.

## [2.22.0] — 2026-07-17
### Changed
- Route spacing is back to its original tightness. Attaching a greeting
  transcript no longer spreads every route apart: only the IVR carrying the
  transcript reserves room for its bubble (below it in horizontal, beside it
  in vertical), with the same clearance around the bubble as before. Maps
  without transcripts render byte-for-byte identically to the previous
  version.
### Fixed
- Option pills (the small 1/2/3/t badges) could be struck through by a
  farther option's connector drawn later (e.g. the timeout route crossing
  the "3" pill). Pills are now drawn in a top layer after all lines, so
  nothing can cross them; they still fade in with their route in the reveal
  animation. Verified to produce the identical set of drawn elements — only
  the stacking order changed.

## [2.21.0] — 2026-07-17
### Added
- True vector PDF export (the permanent fix for blurry PDFs). Maps are now
  drawn inside the PDF as real shapes and selectable text — crisp at any zoom,
  exactly like the Interactive HTML export — and files shrink from megabytes
  to kilobytes. A new "PDF rendering" choice in the download dialog offers
  Vector (default, recommended) or Image (the prior method). If the vector
  renderer ever fails on a map, that map automatically falls back to the crisp
  3x image so exports never break. PDF text uses Latin-1 fonts, so the arrow
  appears as "->" (matching the page headers). Powered by an embedded
  svg2pdf.js v2.7.0; the app remains fully offline.

## [2.20.4] — 2026-07-17
### Fixed
- Restored crisp PDF output. Maps are rendered at 3x resolution again (lossless
  PNG) after an earlier size-saving change to 2x made them look soft when zoomed.
  Note: a PDF embeds the map as an image, so it can't be vector-perfect like the
  HTML export at every zoom level; 3x keeps it visually sharp for viewing and
  print. (Files are larger as a result.)

## [2.20.3] — 2026-07-17
### Fixed
- PDF cover and page headers showed garbage (e.g. "DID 8474033053 !' IVR")
  where the map title's arrow appeared. jsPDF's built-in font can't render the
  → arrow, so PDF text now uses "->" instead (and other non-Latin-1 characters
  are mapped to safe equivalents). The app view and HTML export keep the →.

## [2.20.2] — 2026-07-17
### Fixed
- PDF maps looked blurry after the size reduction because wide maps were being
  rendered below their native resolution. They're now rendered at 2x the map's
  own pixels (lossless PNG, PDF compression on): crisp when zoomed, and about
  half the size of the earlier high-quality version. A raster PDF still can't
  match the tiny vector SVG/HTML files — true vector PDF is possible but needs
  an added renderer.

## [2.20.1] — 2026-07-17
### Changed
- Much smaller PDF files. Maps are now rendered at the resolution the output
  actually needs (about 200 DPI at the printed page for Fit to page, 2x native
  for Actual size) instead of a blanket 3x, and PDF stream compression is
  enabled. Still lossless PNG, so it stays crisp. (A PDF embeds the map as an
  image, so it can't be as tiny as the vector SVG/HTML exports, but it's now a
  fraction of the previous size.)

## [2.20.0] — 2026-07-17
### Added
- A "Download complete" confirmation now appears after a download finishes,
  showing the file name and where it went — the location you chose (Save-As)
  or your browser's Downloads folder (with a reminder that Ctrl+J opens the
  downloads list). Note: browsers don't expose the saved file's folder path to
  a web page, so a direct link to the folder isn't possible.

## [2.19.2] — 2026-07-17
### Changed
- Sharper PDF exports. Maps are now embedded as lossless PNG instead of JPEG
  (no more fuzzy text or lines) and rendered at higher resolution (up to 3x,
  capped for safety), so exports stay crisp and easy to read. SVG and
  Interactive HTML exports were already vector and unchanged.

## [2.19.1] — 2026-07-17
### Changed
- Removed the "Tile across A4 pages" PDF size option. PDF size is now just
  Fit to page or Actual size (100%).

## [2.19.0] — 2026-07-17
### Added
- Download options now include a **Transcripts (.txt)** file: one plain-text
  file listing every attached greeting transcript, labelled by IVR number and
  name (and greeting filename when known). It's auto-selected when transcripts
  are attached, bundles into the .zip alongside the maps, or downloads on its
  own if it's the only format chosen.

## [2.18.1] — 2026-07-17
### Fixed
- Export could fail with "state had changed since it was read from disk" when
  saving via the native Save-As dialog: the chosen file handle can go stale
  while a large file is being built (common on Windows, and more likely with
  the new Actual-size / Tile / HTML exports). If writing to the picked file
  fails, the export now falls back to a normal download so the file still
  saves.

## [2.18.0] — 2026-07-17
### Added
- Download options now let you choose how the export is sized, so maps stay
  readable instead of shrinking to fit the page:
  - PDF size: **Fit to page** (default), **Actual size — 100%** (one page sized
    to the map; opens at 100%), or **Tile across A4 pages** (full size, split
    across pages to print and tape together).
  - **Interactive HTML** format: a self-contained file that opens the map at
    100% with scroll/pan and zoom controls (+/−, 100%, Fit, ctrl+wheel, drag).

## [2.17.3] — 2026-07-17
### Fixed
- Map export sometimes didn't save on browsers without the native Save-As
  picker (Firefox, Safari, or a file:// page): the fallback download anchor
  wasn't added to the page (so the click did nothing in some browsers) and
  its object URL was revoked immediately, cancelling the download before it
  started. The anchor is now attached and the URL is released only after the
  download begins. Where the browser is set to ask, this also restores the
  "where to save" prompt; Chrome/Edge still get the full Save-As dialog.

## [2.17.2] — 2026-07-17
### Changed
- Transcriber batch upload limit raised from 20 to 25 files (5 MB per file
  unchanged).

## [2.17.1] — 2026-07-17
### Fixed
- PDF export could come out blank on large maps (greeting bubbles make maps
  taller, which tipped big exports past the browser's canvas-size limit — an
  over-sized canvas rasterises blank). The rasteriser now caps image
  dimensions to stay within that limit and gives the SVG an explicit size and
  UTF-8 encoding so it always renders; very large maps rasterise at a slightly
  lower resolution instead of failing.

## [2.17.0] — 2026-07-17
### Added
- Attaching transcripts now shows a summary first: how many will be
  attached and how many will not, with Proceed and Cancel so you can review
  or go back and adjust. The "attached to the map" confirmation appears
  only after you proceed.

## [2.16.2] — 2026-07-17
### Fixed
- Greeting bubble in horizontal layout is now narrower and centred under
  the IVR box, so it sits clear of the connector trunk lines on both sides
  (it previously ran flush against the branch line to the child nodes).

## [2.16.1] — 2026-07-17
### Fixed
- Greeting bubble no longer overlaps the connector line in horizontal
  layout: the bubble is narrowed to the IVR box width (200px) so it clears
  the vertical routing channel the branches to child nodes travel through.

## [2.16.0] — 2026-07-17
### Added
- After attaching transcripts, a confirmation pop-up ("N transcript(s) have
  been attached to the map") appears with a "Back to map" button that closes
  the transcribe-audio window and returns to the call flow.
### Changed
- Greeting transcript bubble now sits *below* the IVR box in horizontal
  layout instead of to its right, where it could overlap neighbouring nodes.
  Vertical layout keeps the bubble to the right. Horizontal columns no longer
  widen to reserve side room; the row pitch grows to seat the bubble instead.

## [2.15.0] — 2026-07-17
### Added
- Transcribe-audio window now has its own Dark / Light toggle in the
  banner, matching the app's other windows; it drives the shared theme, so
  the panel, banner, and app stay in sync.
- Each transcript now has an "attach" checkbox instead of its own button,
  plus a single "Attach to map" button at the top of the Transcripts panel
  that attaches every ticked file to its chosen IVR in one click. Unticked
  files are left off the map; auto-matched files are pre-ticked.

## [2.14.1] — 2026-07-17
### Fixed
- Loading a new workbook now clears any transcripts attached to the
  previous call flow's IVRs, so a greeting can't carry over onto a
  different map when IVR numbers happen to line up (IVR_TX is emptied
  and its saved copy removed at the start of the load).

## [2.14.0] — 2026-07-17
### Changed
- Transcribe-audio window now wears the app's own banner: the plain
  title bar is replaced by the Coeo header (logo, title, Close pill) with
  the amber pulse line, so the panel reads as part of PBXware rather than
  a bare iframe. It's a real header element, so it inherits the app's
  dark-mode styling automatically. Transcriber contents unchanged.

## [2.13.1] — 2026-07-17
### Changed
- Transcribe panel: raised the per-batch upload limit from 10 to 20 files
  (5 MB per file is unchanged).

## [2.13.0] — 2026-07-17
### Changed
- Transcribe panel results redesign: each transcribed file is now a distinct card
  with a titled header and a clear divider between files; the "Attach to IVR"
  control sits at the top of each card, above the transcript; and the raw JSON
  response is hidden behind a "Show raw JSON" toggle.

## [2.12.1] — 2026-07-17
### Changed
- Consolidated the transcription panel's standalone history (1.0.0–1.23.0) into
  this changelog, so all changes to both the map and the transcriber are visible in
  one place.

## [2.12.0] — 2026-07-17
### Changed
- The attached IVR greeting transcript now displays in full as a **quote callout**
  beside the IVR node (❝ bubble linked by a leader line) rather than a single
  truncated line — visible on the diagram and in the SVG/PDF/zip exports.
- When an IVR in the current view carries a transcript, the renderer reserves extra
  row pitch and a column gap so the callout sits between the node and its key
  destinations without overlap. Long greetings space the diagram out more; maps
  without transcripts render exactly as before.

## [2.11.0] — 2026-07-17
### Added
- Transcribe → IVR integration. A transcript from the Transcribe audio panel can
  be attached to a specific IVR. The panel shows an **Attach to map** control that
  auto-suggests the IVR by matching the audio filename to the greeting name (keyed
  on the IVR number), with a dropdown to confirm or override.
- Once attached, the greeting transcript becomes part of that IVR's call-map
  diagram details: a third line (❝ …) on the IVR node, the full text in the hover
  tooltip, and carried into the SVG/PDF/zip exports. **IVR nodes only.**
- Transcripts persist locally (`localStorage: cfmap.ivrTx.v1`) and re-attach by IVR
  number when the workbook is re-uploaded.
### Changed
- The embedded transcriber is more seamless: its standalone "Test Bench" header is
  hidden inside the panel, and it now follows the map's light/dark theme (resolving
  the earlier light-only limitation). Bridge is `postMessage` over the iframe.

## [2.10.0] — 2026-07-17
### Added
- Transcribe audio panel — the Deepgram Transcriber (v1.23.0) is now built
  into the "map my system" view. A new **Transcribe audio** button (next to
  Download / Clear file) opens a modal panel that converts call recordings and
  greetings to text via Deepgram or OpenAI, including telephony formats such as
  `.gsm`, `.ulaw`, and `.alaw`.
- The panel loads inside an isolated `<iframe srcdoc>`, so it shares none of the
  map's CSS, element IDs, or JS globals — a robust merge with no collisions. The
  transcriber source is embedded as a base64 blob and decoded on first open
  (lazy-loaded), keeping the map's startup unchanged.
- Network is reached only when a transcription is actually run; the map itself
  remains fully offline. The transcriber's encrypted key vault stays disabled
  (`VAULT_ENABLED=false`) and keeps its own storage namespace, so nothing
  collides with the map (which uses no `localStorage`).

### Notes
- The panel currently renders in its native (light) PBXware-matched palette even
  when the map is in dark mode; a dedicated dark palette for the panel is a
  planned follow-up. The surrounding modal chrome still follows the map theme.

## [2.9.42] — 2026-07-16
### Fixed
- "A specific destination" now lists every DID in the DIDs subsection,
  including unassigned ones (shown as "no destination set") — previously only
  routed DIDs appeared, so unassigned DIDs couldn't be picked individually.

## [2.9.41] — 2026-07-16
### Changed
- "Download all routes" is now a complete system export. Instead of just the
  routed DIDs, it produces one full-tree map for every object:
  - every DID, including unassigned ones with no routing;
  - every IVR, dial/ring group, ERG, queue (with agents) and extension.
  Nothing is pruned. The label and tooltip now list the full breakdown with
  live counts.

## [2.9.40] — 2026-07-16
### Fixed
- Download did nothing after clicking the dialog's Download button. Two modal
  listeners ran at the top level of the script and referenced elements defined
  later in the page, so they threw during load and halted the rest of the
  script — which left the internal "local destination" lookup table
  uninitialised. Every export then failed inside map-building before the save
  prompt could appear. The listeners are now delegated on the document, so the
  script loads cleanly and downloads work again.

## [2.9.39] — 2026-07-16
### Fixed
- The "What to download" tooltips were still being clipped by the dialog's
  scroll area. They now render as a floating element on the page body (fixed
  position), so they can't be clipped — and they flip above the icon and clamp
  to the viewport edges when there isn't room below.

## [2.9.38] — 2026-07-16
### Fixed
- The "What to download" info tooltips no longer clip at the top-left; they now
  open below the icon, right-aligned, and are wider so the full text shows.
### Changed
- The "Download all routes" option now summarises what it contains in its
  label — (Full tree, N DIDs, X IVRs) — using live counts from the workbook.

## [2.9.37] — 2026-07-16
### Changed
- Reworked the Download options "What to download" section into three choices,
  each with a hover/focus info tooltip explaining what it produces:
  - "All active information with routes from the DID" — every routed DID,
    active routes only (local destinations; loop-backs, references and
    dead-ends hidden).
  - "Download all routes" — every routed DID, the full unpruned tree.
  - "A specific destination" — the multi-select, now with its own
    "Active routes only / All routes" toggle that applies to the picked items.
### Removed
- The standalone "Active routes only" checkbox / Routes section — that choice
  now lives in the three options above.

## [2.9.36] — 2026-07-16
### Fixed
- Download did nothing / no save dialog appeared. The save prompt is now
  requested immediately after the Download dialog (within the click gesture),
  before the maps are rasterised — the previous order ran the async rendering
  first, which caused the browser to silently block the save picker and its
  fallback download.

## [2.9.35] — 2026-07-16
### Changed
- Renamed the "Everything" download scope to "Active routes" — it exports all
  DIDs with their active routes and auto-applies the active-routes filter when
  selected.
- Each subsection under "A specific destination" now leads with a main
  checkbox that selects the whole category; an "expand" control reveals the
  individual items only when you want to cherry-pick.
- Multiple output files are now bundled into a single .zip with one save
  prompt (e.g. PDF + SVG, or horizontal + vertical). A single output is saved
  directly in its own format, no zip.
### Fixed
- The empty-selection warning now clears when a whole section is selected via
  its main checkbox, not only when individual rows are ticked.

## [2.9.34] — 2026-07-16
### Added
- Each subsection in "A specific destination" (DIDs, IVRs, Dial Groups, ERGs,
  Extensions) now has a "select all" checkbox in its header, with a partial
  (indeterminate) state when only some rows are ticked. This makes it easy to
  export a whole category in one click — including ERGs, extensions, and IVRs
  that have no direct DID routing. Individual ticks keep the group checkbox in
  sync.

## [2.9.33] — 2026-07-16
### Changed
- Redefined "Active routes only" to match how routes are actually used: it now
  keeps every branch that reaches a local destination (extension, voicemail,
  ERG, dial group, queue — plus dial-by-name Directory) and hides only the
  branches that never land on a real endpoint (loop-backs, "shown above" IVR
  references, and dead-end menu options). After-hours paths are kept when they
  reach a local destination. Unchecked still shows all routes.

## [2.9.32] — 2026-07-16
### Changed
- The Routes section is now a single "Active routes only" checkbox: ticked
  skips after-hours / closed-hours branches, unticked includes all routing
  destinations (replaces the previous two-option radio).
### Added
- "A specific destination" can now export more than DIDs and IVRs — the
  picklist adds collapsible Dial Groups, ERGs, and Extensions subsections,
  so any of those (including IVRs not reached by a DID route) can be mapped
  and downloaded on their own. Extension maps include the outbound and E911
  caller-ID lines.

## [2.9.31] — 2026-07-16
### Added
- In the Download options dialog, choosing "A specific destination" with no
  DID or IVR ticked now shows a persistent inline warning ("No option
  selected — no file to download"). It appears as soon as the section is
  selected while empty and clears automatically once at least one item is
  ticked.

## [2.9.30] — 2026-07-16
### Changed
- The Summary page's Download and Clear file buttons are now larger and more
  prominent (Download styled as the primary action) and grouped together,
  aligned to the centre-right of the panel.

## [2.9.29] — 2026-07-16
### Changed
- In the Download options dialog, "A specific destination" now presents the
  DIDs and IVRs as separate collapsible subsections (each with a count) that
  expand on click, instead of one long combined list.

## [2.9.28] — 2026-07-16
### Changed
- Orientation moved into the Download options dialog as Horizontal / Vertical
  checkboxes (choose either or both). The separate "PDF layout" prompt is gone.
- Choosing both orientations emits both: two PDFs (…_horizontal / …_vertical)
  and, for SVG, both variants inside the one .zip. The SVG export now honours
  the chosen orientation(s) instead of always exporting horizontal.

## [2.9.27] — 2026-07-16
### Changed
- Consolidated the two export buttons into a single "Download…" button.
  Format is now chosen inside the Download options dialog via checkboxes —
  PDF, SVG (.zip), or both. The dialog gathers routes, scope, name, and
  format once, then emits each selected format in turn.

## [2.9.26] — 2026-07-16
### Added
- The "All maps" downloads (PDF and SVG .zip) now open a Download options
  dialog with:
  - a Routes choice — "All routing destinations" (default) or "Active
    routes only", which skips after-hours / closed-hours branches from
    Operation Times gates and shows just the live in-hours path;
  - a scope choice — "Everything" (all routed DIDs) or "A specific
    destination", a multi-select of individual DIDs and IVRs;
  - a File name field to name the download up front.
### Changed
- The previous single-DID scope picker is replaced by the multi-select
  above; IVRs can now be exported directly as their own maps. The PDF
  cover lists exactly the maps included and notes when active-only is on.

## [2.9.25] — 2026-07-16
### Added
- Both "All maps" downloads (PDF and SVG .zip) now first ask whether you
  want to export everything or just a specific DID. Choosing a single DID
  scopes the whole export to that one call flow (and names the file after
  it). The prompt is skipped automatically when only one routed DID exists.

## [2.9.24] — 2026-07-16
### Changed
- The extension caller-ID line is now labelled "Outbound Caller ID".
### Added
- Mapping an extension now also shows the Assigned E911 Caller ID on a
  fourth line (🆘 E911 Caller ID: …), read from the Caller ID sheet's
  Emergency column, or "no E911 Caller ID assigned" when none is set.
  Node boxes grow to four lines as needed and the full text is in the
  hover tooltip.

## [2.9.23] — 2026-07-16
### Changed
- The "Map an ERG" section now sits above "Map an Extension".
### Added
- The Caller ID sheet is now parsed. When an extension is mapped, its
  node shows the assigned Caller ID(s) on a third line (☎ Caller ID: …),
  or "no Caller ID assigned" when none is set. Distinct caller-ID
  numbers across all trunk columns are collected per extension; the full
  list is in the node's hover tooltip.

## [2.9.22] — 2026-07-16
### Added
- New "Map an ERG" collapsible section (below Map an Extension) listing
  every Emergency Ring Group by number and name. Clicking one maps that
  ERG as the root, showing its member extension. ERGs also take part in
  route highlighting: selecting any object highlights the ERGs that
  appear in that map. The section starts collapsed.

## [2.9.21] — 2026-07-16
### Changed
- The "Map an Extension" section now starts collapsed (its list can be
  long); click its header to expand. Route highlighting still applies to
  the extension buttons whether the section is open or collapsed.

## [2.9.20] — 2026-07-16
### Added
- New "Map an Extension" collapsible section (below Map a Dial Group)
  listing every extension by number and name. Clicking one maps that
  extension as the root, with its voicemail where configured.
- Extensions now take part in route highlighting: selecting a DID, IVR,
  Dial Group or Operation Hours entry highlights the extensions that
  appear anywhere in that map and dims the rest.

## [2.9.19] — 2026-07-16
### Changed
- Operation Hours entries now show simply the object that owns the
  schedule (e.g. "IVR 601", "DID 8474033053") rather than an inferred
  parent or DID. Only objects that actually have an operation-hours
  schedule appear, each under its own name.

## [2.9.18] — 2026-07-16
### Changed
- Operation Hours entries that apply to an IVR or Dial Group now show
  their immediate parent (the object that routes to them) as the top
  line — e.g. "IVR 600 → IVR 601" instead of attributing the entry to a
  DID it only reaches indirectly. DID entries still show the DID.
### Fixed
- The parent lookup now treats an entry's own Operation Hours gate as
  transparent, so an IVR/Dial Group that has its own schedule no longer
  shows itself as its own parent; the real routing object is used.

## [2.9.17] — 2026-07-16
### Changed
- Operation Hours is now its own section directly below "Map a DID"
  (moved out of the Summary section).
- Each Operation Hours entry now leads with the DID it belongs to as
  the bold top line; the scheduled object (when it is an IVR or Dial
  Group rather than the DID itself), the schedule, and the after-hours
  destination appear on the sub-line. The owning DID is resolved by
  tracing which DID's route reaches the scheduled object. Clicking still
  maps the entry and highlights the active route, including its DID.

## [2.9.16] — 2026-07-16
### Added
- The "Map a DID" picker now takes part in route highlighting. When an
  Operation Hours entry (or an IVR / Dial Group) is selected, the DID
  whose route reaches it is highlighted, and the other DIDs are dimmed;
  selecting a DID highlights that DID. This makes it clear which DID an
  Operation Hours schedule belongs to.

## [2.9.15] — 2026-07-16
### Added
- Operation Hours entries are now clickable. Clicking one maps its
  target (DID / IVR / Dial Group) in the Call Map Diagram, showing the
  open vs. after-hours split via the OT gate, and highlights the active
  route across the IVR, Dial Group and Operation Hours pickers (green
  for IVRs, teal for dial groups). The clicked entry shows as selected;
  entries whose target can't be resolved are shown disabled.

## [2.9.14] — 2026-07-16
### Changed
- Operation Hours is now its own collapsible subsection under Summary
  (with the same click-to-collapse header as the picker sections),
  instead of being embedded inside the file/stats panel.

## [2.9.13] — 2026-07-16
### Added
- The Summary section now lists Operation Hours: each entry shows what
  it applies to (DID / IVR / Dial Group), its schedule (e.g.
  "Mon–Fri 09:00–17:00"), and where after-hours callers are sent
  (IVR / Dial Group / Extension with owner name / Voicemail). Entries
  with no after-hours destination are noted as such. Dark-mode styling
  reuses the existing list style.

## [2.9.12] — 2026-07-16
### Added
- Each picker section — Map a DID, Map a single IVR, Map a Dial Group —
  can now be collapsed or expanded by clicking its header. A caret
  (▾) rotates to show the state, and the button list hides when
  collapsed, which helps when a workbook has long lists. Sections start
  expanded; the caret animation respects "reduce motion".

## [2.9.11] — 2026-07-16
### Changed
- Route highlighting in the pickers now matches each destination's
  colour in the Call Map Diagram: on-route IVRs highlight green and
  on-route Dial Groups highlight teal (previously both were green), so
  the picker colour matches the node colour on the map. Dark-mode
  variants included.

## [2.9.10] — 2026-07-16
### Changed
- The "Map a Dial Group" buttons now show just the group number and
  name (e.g. "Dial Group 200 — New Age Chicago RG"), matching the
  "Map a single IVR" buttons. The full member list is still on the
  group's own map; the group members also remain in the map itself.
### Added
- Dial groups now get the same route highlighting as IVRs: selecting a
  DID (or IVR) highlights the dial groups reached by that route in
  green and dims the rest. The dial group you have selected stays on
  the active route (and keeps its selected style).

## [2.9.9] — 2026-07-16
### Changed
- "Map a Dial Group" is now its own section below "Map a single IVR",
  matching the DID and IVR pickers, instead of sitting inside the
  Summary panel next to the export buttons. The Summary panel again
  shows just the file name, stats line, and export controls.
### Fixed
- The "all clear" green checkmark in a map's findings could render
  huge — a global "svg { width:100% }" rule was stretching the small
  inline check. It is now pinned to 14px so it stays a consistent size
  in every map (seen, e.g., after selecting Dial Group 200).

## [2.9.8] — 2026-07-16
### Changed
- Added a "Summary" section heading above the file summary panel, so it
  matches the "Call Map Diagram" section styling.

## [2.9.7] — 2026-07-16
### Changed
- Renamed the on-screen section heading "Call Map" to "Call Map Diagram".

## [2.9.6] — 2026-07-16
### Changed
- Every destination type now has its own colour. Previously IVR, Dial
  Group and Voicemail nodes were all the same green; they are now IVR
  green, Dial Group teal and Voicemail rose. Directory / Remote-Access
  leaves get their own indigo (previously grey, shared with the
  non-destination "shown above" nodes). ERG (purple), Queue (slate),
  Extension (blue), the OT gate (amber) and the source DID (navy) are
  unchanged, so all seven destination types are now visually distinct.
### Added
- A colour legend under each map's header, listing only the node kinds
  present in that map (with dark-mode styling). This applies to the
  on-screen map; the SVG/PDF exports use the same colours.

## [2.9.5] — 2026-07-16
### Added
- The file summary panel now lists every dial group beneath the stats
  line: group number, name, and its members (each member shown with the
  extension owner's name where known, e.g. "251 (Lily)"). Groups with
  no members listed are noted as such. Dark-mode styling included.

## [2.9.4] — 2026-07-16
### Added
- Route-aware IVR picker: selecting a DID (or a single IVR) now marks
  every IVR button that is part of the traced route with a green
  highlight, while IVRs not reached by that route are dimmed. The set
  includes IVRs reached through key presses, timeouts, Operation Hours
  closed branches, references ("shown above") and loops. Dimmed
  buttons stay clickable (and brighten on hover) so a different IVR
  can still be mapped directly; tooltips state "On/Not on the active
  route". Dark-mode variants included; the selected button keeps its
  dark filled style.

## [2.9.3] — 2026-07-16
### Changed
- The file-confirmation panel's buttons were restyled and repositioned:
  **▶ Start call flow map** and **✕ Cancel** now sit on their own row
  at the bottom of the panel (below the file name and note), rendered
  as large pop-out buttons — bigger type, rounded, drop shadows, a
  springy pop-in on appearance (Cancel staggered slightly), and a
  hover lift. Start is the filled primary button; both have dark-mode
  variants, and all motion is disabled under "reduce motion".

## [2.9.2] — 2026-07-16
### Changed
- Uploading a file no longer starts processing automatically. Choosing
  or dropping an .xlsx now shows a confirmation panel with the file
  name and size and two buttons — **▶ Start call flow map** and
  **✕ Cancel**. Nothing is read from the file until Start is pressed;
  Cancel discards the selection (and the same file can be re-picked).
  Selecting a new file while one is staged or loaded replaces it.

## [2.9.1] — 2026-07-16
### Changed
- IVR greeting names moved from the name line to their own third line
  inside the node for legibility. Boxes with a greeting grow from 40px
  to 52px; two-line nodes are unchanged. Connectors attach to the
  taller boxes' true edges, the canvas gains bottom margin so a tall
  last-row box never clips, and the hover tooltip still carries the
  full name and greeting.

## [2.9.0] — 2026-07-16
### Added — newer PBXware export format
- **Operation Hours (new layout)**: the sheet format `Local Destination ·
  Type · Days · Time From · Time to · Afterhours Destination` is now
  parsed (detected from the header; the old layout still works). The OT
  gate on the map shows the real schedule — e.g. "Mon–Fri 09:00–17:00" —
  with days compressed into ranges, "09;00"-style typos normalised, and
  Excel day-fraction times (0.7083… → 17:00) converted. The Afterhours
  Destination is traced as the closed-hours branch.
- **IVR greeting names**: the "Greeting name" column is read and shown
  on each IVR node (♪ greeting-…), in the app and in all exports; the
  full name is always in the hover tooltip.
- "Remote Access – Voicemail" style options are now drawn as a
  voicemail-login leaf instead of being reported as unresolved.
### Fixed
- With the new IVR sheet layout the greeting column shifts every option
  right by one; the parser previously read the greeting as "Option 1"
  and mis-mapped all keys and the timeout. Column positions are now
  derived from the header, so both layouts trace correctly.
- An OT row that has a schedule but no afterhours destination now shows
  its schedule and still raises the "no closed-hours destination"
  finding (previously the schedule was dropped).

## [2.8.2] — 2026-07-14
### Changed — downloads now ask where to save first
- Every download (single-map SVG, PDF report, SVG .zip bundle, sample
  workbook) now opens the browser's native "Save As" dialog BEFORE the
  file is generated, so the destination folder and filename are chosen
  up front. For the PDF and zip this replaces the name prompt; the
  dialog appears right after the orientation choice (PDF) or button
  click (zip), and the finished file is written straight into the
  chosen location.
- Cancelling the dialog cleanly aborts the export.
- Browsers without the File System Access API (Firefox, Safari) fall
  back to the previous behaviour: a name prompt where applicable, then
  a standard download. Tip: browsers can also be set to "always ask
  where to save" in their own download settings.
### Fixed
- Downloads previously used an invisible auto-clicked link, which some
  browser configurations block silently — the button appeared to do
  nothing. The Save-As dialog path avoids that mechanism entirely, and
  any write error is now surfaced instead of swallowed.

## [2.8.1] — 2026-07-13
### Changed
- App renamed to **PBXware — Call Flow Map Creator** (window title,
  banner, footer, and the changelog/README headers). The reference
  diagram keeps its own descriptive title, "Master Inbound Call Flow
  Map", since that names the diagram rather than the app; historical
  changelog entries quoting the old name are left as written.

## [2.8.0] — 2026-07-13
### Added — animation set 2 (landing polish + generated-map motion)
- **View transitions**: switching landing ↔ reference map ↔ upload now
  fade-slides (~220ms) instead of hard-toggling.
- **Call-map reveal**: generated maps build themselves — each node (with
  its connector) fades in staggered by depth. Skipped automatically on
  maps over 60 nodes so large traces stay instant, and under the OS
  "reduce motion" preference.
- **Live trace (▶ Trace button)**: a call-dot travels the real routed
  paths of the displayed map, root → leaf, one route after another at
  constant speed, flashing each terminating destination on arrival.
  One dot at a time, so cost is independent of map size. Toggles to
  ■ Stop; cancelled by re-render, layout switch, or Clear file.
- **Micro-interactions**: drop zone pulses while a file is dragged over
  it; DID/IVR buttons lift on hover and pop when selected; every modal
  (changelog, expanded map, PDF orientation) scales in.
- **Theme cross-fade**: dark/light toggle transitions chrome colours
  (~250ms) instead of hard-cutting; scoped to UI chrome, not the SVGs.
- **Findings polish**: findings panels slide in staggered, and the
  all-clear checkmark draws itself.
### Changed — existing animations tuned
- Banner pulse now animates `transform` instead of `left`: the endless
  loop runs on the compositor with no per-frame layout work.
- Route-demo dot cycle tightened from 26s to 22s (idle gap between dots
  cut from ~1.6s to ~0.8s) and consolidated: 15 hand-offset keyframe
  blocks became 5 shared ones phased by animation-delay. Constant
  speed-per-unit across routes is preserved (~221 u/s), and box-hit
  timing matches dot arrival exactly.
- Zoom on the reference map and expanded view now glides (180ms) instead
  of snapping.
### Fixed
- Reduced-motion users no longer see five colored dots frozen in a stack
  at the start of the route demo's spine — the guard forced the dots to
  opacity 1 while all five paths start at the same point. Dots are now
  hidden when motion is reduced; the guard also covers every animation
  added in this release.

## [2.7.2] — 2026-07-13
### Fixed
- SVG bundle export no longer fails with "Cannot access 'CRC_T' before
  initialization". The CRC-32 table was a top-level `const`; if anything
  earlier in page load aborted, the hoisted export functions stayed
  callable while that `const` was left permanently in its temporal dead
  zone. The table is now built lazily inside `crc32` and cached, so the
  export never depends on top-level initialisation order.
### Added
- PDF export now asks for the map orientation first — Horizontal
  (landscape pages) or Vertical (portrait pages) — via a small chooser.
  The chosen orientation drives both the page size and how each map is
  laid out.

## [2.7.1] — 2026-07-13
### Polish — generated call maps
- Node labels no longer overflow their box: the primary label is now
  truncated with an ellipsis to fit the box width, matching the sub-line
  (both budgets are computed from the box width rather than hard-coded).
- Every node in a traced map now carries a hover tooltip showing its
  full, untruncated label and sub-line — so nothing is lost to the
  ellipsis. Works in the app and in the exported standalone .svg.
- Findings text fixed: an unresolved top-level DID destination read
  "IVR DID … key destination"; a bare number now shows as "IVR N key K"
  while an already-labelled source (e.g. "DID …") is shown as-is. The
  target string is HTML-escaped.
- Spacing and outline pass: more room between stacked leaves (horizontal)
  and between columns (vertical); the members node gets a visible outline
  instead of the near-invisible pale stroke.

## [2.7.0] — 2026-07-13
### Added — SVG bundle export
- Alongside the PDF, "All maps (SVG .zip)" exports every routed DID's
  call map as an individual .svg, packed into one compressed .zip
  (deflate via the browser's CompressionStream; stored uncompressed
  only if the browser lacks it). Filename prompt included, matching
  the PDF export.

## [2.6.1] — 2026-07-13
- Operation Times gates now drawn at **every** source, not only the
  DID: an IVR, Dial Group, ERG or Queue listed on the Operation Hours
  sheet gets its own amber gate in the trace, forking closed/open just
  like the DID's. The ⏱ badge is replaced by the gate itself. Objects
  whose closed destination is missing from the export are flagged with
  their type (e.g. "IVR 601").

## [2.6.0] — 2026-07-13
### Added — Operation Times closed-hours tracing
- The Operation Hours sheet now supports three more columns: Closed
  Destination, Is Voicemail, and an optional Schedule. When present,
  the DID's OT gate forks on the map: "open" continues down the tree,
  "closed" is traced to its after-hours destination (voicemail or any
  local destination). The schedule prints on the gate node.
- New finding: an OT-enabled DID with no closed destination in the
  export is flagged — the after-hours route is untraceable.
- Sample workbook updated to the new Operation Hours format with
  example rows.

## [2.5.2] — 2026-07-13
- PDF export now asks for a filename before generating (default
  "call_flow_report"); characters invalid in filenames are stripped,
  ".pdf" is appended automatically, and cancelling the prompt aborts
  the export before any work starts.

## [2.5.1] — 2026-07-13
- Banner removed from the PDF report (all pages); page layouts
  re-tightened and the embedded banner image dropped from the file.

## [2.5.0] — 2026-07-13
### Added — PDF export of all call flows
- After loading a workbook, a "Download all (PDF)" button generates a
  complete report in the browser: a cover page (generation time, object
  counts, and every DID with its first hop — unrouted DIDs flagged in
  red), followed by each routed DID's full call map.
- The official Coeo banner heads **every page** of the PDF.
- Large maps are sliced across multiple landscape pages ("part n of m")
  rather than being shrunk to illegibility.
- jsPDF is embedded in the file, so the export works fully offline like
  everything else.

## [2.4.13] — 2026-07-13
- Queue recoloured to slate blue-grey (#607d8b) in the route demo (box,
  label, dot) and in generated call maps' Queue nodes.

## [2.4.12] — 2026-07-13
- Queue recoloured from teal to magenta — the teal read as another
  green shade next to the IVR. Applied in both the landing route demo
  (box, label and travelling dot) and the generated call maps' Queue
  nodes, so the colour language stays consistent.

## [2.4.11] — 2026-07-13
- Dot journey retimed to 3.0–3.3 seconds (was 3.6–3.9): the straight
  middle route crosses in 3.0s and the outer Queue/Voicemail routes in
  3.3s, keeping speed constant per unit. Timings regenerated exactly
  for the current geometry, and the arrival highlights re-synced.

## [2.4.10] — 2026-07-13
- Canvas widened to 791 units: the arrival highlight scales destination
  boxes to x=786, which was clipping at the 784 right edge; a 5-unit
  allowance now sits between the enlarged box and the margin.

## [2.4.9] — 2026-07-13
- Split-to-destination run tightened to 50 units; destination column at
  x=660 and the canvas trimmed to 784 units, keeping the boxes flush
  with the right margin.

## [2.4.8] — 2026-07-13
- Split-to-destination run set to 150 units (was 210): the destination
  column moved left to x=760 and the canvas trimmed to 884 units so the
  boxes stay flush with the right margin at full width.

## [2.4.7] — 2026-07-13
- Fan split point moved to 80 units from the IVR's right edge (was
  190), so branches turn earlier and run longer horizontally into the
  destinations. Total path lengths are unchanged by the move, so all
  dot timings remain in sync.

## [2.4.6] — 2026-07-13
- Route demo widened to fill the landing content edge-to-edge: geometry
  redrawn on a 944-unit canvas (uniform 60px spine legs, longer fan
  runs, destination boxes flush with the right margin) and the SVG set
  to 100%% width, eliminating the dead space on the right.

## [2.4.5] — 2026-07-13
- Spine legs equalised: box widths trimmed so every gap between stages
  (PSTN→Trunk, Trunk→DID, DID→OT, OT→IVR) is a uniform 28px, and each
  arrow now spans its full gap and lands on the next box, instead of a
  short 12px stub floating between unevenly spaced stages.

## [2.4.4] — 2026-07-13
- Route demo geometry corrected. The Voicemail branch's draw-in used a
  dash length shorter than the path, so the line never finished and the
  arrowhead sat detached; all connectors now use pathLength
  normalisation and draw fully to their arrowheads.
- The spine (PSTN → Trunk → DID → OT → IVR) moved to the vertical
  centre of the destination stack: the fan is now symmetric — Queue and
  Voicemail equal-length outer branches, Ring Group and Extension equal
  mid branches, IVR a straight centre line.

## [2.4.3] — 2026-07-13
- Route demo destinations recoloured with genuinely distinct hues: teal
  Queue, purple Ring Group, green IVR, blue Extension, orange Voicemail
  (matching the ERG purple / IVR green of the real maps).
- Branch lines now carry arrowheads and land on the destination boxes
  instead of stopping short.
- On arrival, the destination reacts: the box enlarges briefly and its
  border thickens while the dot lands, then settles back.

## [2.4.2] — 2026-07-13
- Banner pulse made visible in light mode: brighter amber gradient with
  a soft glow (dark mode keeps the ice-blue pulse).
- Footer message line restored ("PBXware Call Flow Mapper · reference
  map + data-driven DID/IVR tracing · runs entirely offline") above the
  credit, with the divider back between them.
- Route demo: each destination now has its own subtly distinct colour
  (teal Queue, emerald Ring Group, green IVR, blue Extension, olive
  Voicemail) and the travelling dot takes the colour of the route it is
  riding. Dot keyframes rebuilt with explicit slots for all five routes
  so none — including Extension — is skipped.

## [2.4.1] — 2026-07-13
- Route-trace demo expanded to the full journey: PSTN → Trunk → DID →
  OT Gate → IVR, fanning out to all five main local destinations
  (Queue, Ring Group, a second IVR, Extension, Voicemail).
- The call-dot now cycles through the five routes, one per pass, at a
  constant slow speed — longer routes take proportionally longer, so
  the dot never rushes a path.

## [2.4.0] — 2026-07-13
### Added — landing screen animation set (all five options)
- Staggered entrance: banner slides in, then heading, lede, route
  animation and the two cards rise in sequence.
- Banner pulse: a light travels the banner's bottom edge every few
  seconds, echoing the line through the Coeo logo.
- Card micro-interactions: the map card's icon ripples like a ringing
  phone on hover; the upload card's icon bobs.
- Drifting node-graph background behind the landing content, at low
  opacity (lower still in dark mode).
- Route-trace demo under the heading: DID → OT gate → IVR → Extension,
  arrows draw themselves in, then a call-dot travels the path on loop.
- All animations respect the OS "reduce motion" preference and are
  disabled for those users.

## [2.3.5] — 2026-07-13
- The version is now a button in the upper-right of the banner (next to
  the theme toggle); clicking it opens the embedded changelog. The
  inline version links in the subtitle and footer are removed.
- Footer trimmed to just the "Created by" credit line.

## [2.3.4] — 2026-07-13
- Sample workbook replaced with a clean template: the same ten tabs
  with example rows demonstrating the expected columns, no customer
  data. Both download links (landing card and upload view) now serve it.

## [2.3.3] — 2026-07-13
- "Clear file" button added next to the loaded workbook's name. It
  drops the parsed model and removes the DID/IVR pickers, findings and
  map, returning the upload view to its empty state — and resets the
  file input so the same file can be selected again immediately.

## [2.3.2] — 2026-07-13
- The sample workbook download is now also available on the landing
  screen, at the bottom of the "Map my system from a file" card
  (clicking it downloads without entering the upload view).

## [2.3.1] — 2026-07-13
- Dial Groups now fan out to their actual members: every member is drawn
  as its own extension node with its name, instead of a single summary
  box with a truncated list. The group node states how many ring at
  once, and the no-answer voicemail stays as the final child.

## [2.3.0] — 2026-07-13
### Added
- **Queues and Agents.** The mapper now reads the workbook's Queues tab
  (Name, Number, Strategy, Agents) and Agents tab (name + number, plus
  the PBXware agent fields). A DID or IVR option landing on a queue
  draws a teal Queue node with its strategy and the agents rung.
- **Sample workbook download** in the upload view — the example .xlsx
  (all ten tabs, including the new Queues and Agents) is embedded in the
  app and downloads offline, to use as a template for real exports.

## [2.2.4] — 2026-07-13
- Theme-adaptive logo done properly: instead of a CSS filter, a real
  light-on-dark variant of the lockup (white circles and wordmark,
  brightened blue "e") is embedded and swapped in when dark mode is on.
  Light mode keeps the original navy lockup on its white pill.

## [2.2.3] — 2026-07-13
- The banner logo now adapts to the theme: light mode keeps the navy
  lockup on its white pill; dark mode drops the pill and inverts the
  artwork (hue-preserving) so it reads as a light lockup directly on the
  dark banner.

## [2.2.2] — 2026-07-13
- Banner logo replaced with the full Coeo "Business Connectivity"
  lockup; sized up slightly to keep the tagline legible.

## [2.2.1] — 2026-07-13
- Expanded view now uses the full screen and auto-fits the map to the
  window on open (Reset returns to the fitted size).
- Horizontal / Vertical layout switching and the dark/light theme toggle
  are available inside the expanded view; in dark mode the expanded map
  re-themes with the same hue-preserving inversion as the reference map.

## [2.2.0] — 2026-07-13
### Added
- The generated map now sits under a named **"Call Map"** section header.
- **Expand** button on every generated map: opens the map full-screen in
  an overlay with its own Zoom in / Zoom out / Reset, **Download SVG**,
  and Close controls.
### Fixed
- The version link previously pointed at CHANGELOG.md as a separate
  file, which showed nothing when the HTML travels alone. The changelog
  is now embedded in the app: clicking the version (banner or footer)
  opens it in an in-app overlay.

## [2.1.2] — 2026-07-13
- The light/dark mode toggle moved into the banner, so it is available
  on the landing screen, the reference map, and the upload/mapper view
  alike; the duplicate button in the map toolbar removed.

## [2.1.1] — 2026-07-13
- Footer restructured: the "Created by Ron Mangune" credit moved to its
  own smaller line below the footer's main row, centred, separated by a
  hairline, with a smaller LinkedIn mark.

## [2.1.0] — 2026-07-13
### Added
- **Map per IVR.** The upload view now lists every IVR in the workbook
  alongside the DIDs; picking one traces that IVR as the root — its
  options → terminating destinations only.
- **Horizontal / Vertical layout toggle** on every generated map. The
  vertical layout narrows boxes and re-routes connectors top-down;
  spacing in both modes guarantees no overlapping nodes or labels.
- **Footer** across all views with version link, and "Created by Ron
  Mangune" with the LinkedIn mark linking to his profile — also shown at
  the bottom of the landing screen's left card.
### Changed
- Responsive layout: the landing cards stack on narrow windows, the
  reference-map canvas scales to the window width, and paddings tighten
  under 900px.

## [2.0.0] — 2026-07-13
### Added — the mapper is now data-driven
- The upload view reads a **PBXware .xlsx export directly in the browser**
  (ZIP + DecompressionStream + DOMParser — no libraries, still one
  offline file). Sheets read: DID routing, IVR, Dial Group, ERG,
  Extension, Voicemail, Operation Hours.
- Every DID in the workbook is listed; picking one traces and draws its
  complete call map: **DID → Operation Times (if enabled) → IVR → every
  configured option → the terminating destination** (extension,
  voicemail, dial-group members, ERG member, directory).
- Findings are reported beside the map, not drawn as nodes: IVRs with no
  Timeout Destination, options whose target matches no object in the
  workbook, and options that route back into a menu already on the path.

### Scope
Only **configured** routes are drawn. An unset option or unset timeout is
the absence of a destination, so it gets no node — it is listed as a
finding instead. An IVR is expanded once; later references to it are
shown as a dashed "shown above" node rather than duplicating the subtree.

### Verified against
New Age Elder Care export — 6 DIDs, 21 IVRs, 4 dial groups, 29 ERGs,
32 extensions. DID 8474033053 traces to 48 terminating destinations.

## [1.3.0] — 2026-07-13
### Added
- **Landing screen.** The HTML now opens on a launcher with two paths:
  "View the call map" (the reference diagram, unchanged) and "Map my
  system from a file". A Home button in the banner returns to it.
- **Upload view.** Drag-and-drop or browse for PBXware CSV exports.
  Files are parsed in the browser — nothing leaves the machine.
  A DID export (detected by an `nr1` column) renders a routing table of
  every DID to its first-hop destination, and flags DIDs with no
  destination set as dead ends. Non-DID files are reported rather than
  silently ignored.
- Guidance panel listing what to export per object type and what each
  additional export unlocks.
- `sample_dids.csv` added so the upload path can be tried immediately.

### Note
The upload view currently traces the **first hop only**. Following DIDs
onward into IVR digit maps, queue members and timeout destinations is the
next step, and needs those exports.

## [1.2.0] — 2026-07-13
- Tooltips added to Parts 4 and 5, completing coverage across the whole
  map (19 in total). Part 4: Ring All, Announce sound, group Default
  Destination, and the ERG panel. Part 5: Max Callers, The Wait, Exit
  Digit, Members vs Agents, and Timeout/Overflow. Same rule as Parts 1-3
  — each adds a constraint, an example, or a failure mode not visible on
  the canvas; nothing restates what is already printed.

## [1.1.11] — 2026-07-13
- Text selection is now visible. Added explicit ::selection styling
  (amber highlight on dark text) for the page and for SVG text, with a
  separate rule for dark mode so the canvas inversion filter does not
  wash the highlight out. Selection explicitly enabled on the canvas.

## [1.1.10] — 2026-07-13
- Part 3: destination-types panel lowered so its vertical middle aligns
  with the keypad's — the "any" arrow from the selections into the panel
  is now a single straight horizontal line instead of an elbow.

## [1.1.9] — 2026-07-13
- Fixed the "no" branch in the Part 3 termination. The 1.1.8 cascade had
  shifted its elbow turn-point inconsistently, leaving the arrow running
  backwards — up from the diamond's bottom tip instead of down. It now
  drops from the bottom tip into the centre of the Extension rings box.

## [1.1.8] — 2026-07-13
- Part 3: the selections column (header, keypad, note, Invalid and
  Timeout Selection) lowered so the keypad's vertical middle aligns with
  the Greeting box — the "key" arrow is now a single straight horizontal
  line rather than an elbow. Termination, the ✱/# panel and the export
  panel shifted down to clear it; Parts 4 and 5 cascaded; canvas extended.

## [1.1.7] — 2026-07-13
- Part 3: the "key" arrow from Greeting plays now elbows up and points
  into the vertical middle of the keypad's left edge, instead of running
  flat off the side of the Greeting box toward empty space.

## [1.1.6] — 2026-07-13
- "Extension rings" subtitle changed from "operator / receptionist" to
  "deskphone · softphone · mobile app" — it describes the devices that
  ring, not an assumed role. Applied in both places it appears (Part 3
  termination and Part 4 Dial Group).

## [1.1.5] — 2026-07-13
- Tooltips added to Part 3, following the same rule as Parts 1 and 2 —
  only where they add something the canvas cannot show. Eight in total:
  Greeting (the two timeouts), Play Greeting (the counter and its
  consequences), Selection ✱, Selection #, Invalid Selection (its replay
  burns the counter), Timeout Selection (silence vs wrong key), Timeout
  Destination (the invisible field where calls vanish), and the
  Is-Voicemail flag (the full fall-through to disconnect).
  No tooltips on the plain digit keys or the destination-type chips —
  those are self-evident.

## [1.1.4] — 2026-07-13
- Part 1 tooltips pruned to match Part 2. Removed the Provider tooltip
  (the box already states it) and the OT Gate tooltip (the priority
  ladder panel sits directly beside it and says the same thing).
  Trimmed Trunk, DID Match, CLI, Range and Destination to their unique
  content; dropped the billing footnote from Destination, consistent
  with its removal from the legend.

## [1.1.3] — 2026-07-13
- Part 2 tooltips pruned. Removed the three OT-gate tooltips (they
  repeated each other, the Part 1 OT panel, and the legend gotcha).
  Trimmed the five node tooltips: dropped billing lines (billing was
  removed from the legend in 1.1.1) and "→ expanded in Part N" lines
  (already printed on the map as labels). Each tooltip now carries only
  what is not visible elsewhere.

## [1.1.2] — 2026-07-13
- Legend rebuilt as two bounded columns with a divider: swatches and
  their (now wrapped) descriptions on the left, the gotcha note on the
  right. The amber and green lines had been running ~570px wide and
  colliding with the gotcha column, and the gotcha's last lines were
  falling outside the box after it was tightened in 1.1.1.

## [1.1.1] — 2026-07-13
- Legend trimmed: removed the "✻ Billing only" footnote and the
  "Red dashed / Amber dashed" line, plus the now-orphaned ✻ marker on
  the Extension node. Legend box tightened to fit. The billing detail
  remains available in the Extension and Destination tooltips.

## [1.1.0] — 2026-07-13
### Corrected
- **Extension recoloured green.** It had been blue, which implied it was
  not a local destination. It is one — Extension is an internal DID
  destination exactly like IVR, Queue, Conference and Voicemail. The
  colour channel had been overloaded to carry a *billing* distinction.
- Billing difference moved to a secondary "✻" marker on the Extension
  node and a legend footnote: IVR/Queue/Conference/VM bill as "Local
  destinations" from the Service Plan; an Extension bills per the DID's
  E.164 incoming price. Same routing category, different invoice line.
- Extension and Part 1 Destination tooltips reworded to lead with the
  routing fact and demote billing to a footnote.

## [1.0.24] — 2026-07-13
- Tooltips added to Part 2: all five destination nodes (IVR, Queue,
  Ring/Dial Group, Extension, Conference/Voicemail) and the three OT
  gates. Each explains the object, its billing treatment, and where it
  is expanded; the OT gate tips call out the double-divert behaviour.
  Reuses the amber tooltip engine introduced in 1.0.8.

## [1.0.23] — 2026-07-13
- Light mode is now the default on load. The page no longer follows the
  OS colour-scheme preference; dark mode is opt-in via the toolbar button.

## [1.0.22] — 2026-07-13
- Part 2: horizontal distribution bus trimmed from x=1055 to x=1035 —
  it had overshot the Conference/Voicemail drop point by 20px, leaving a
  loose line hanging past the last column.

## [1.0.21] — 2026-07-13
- Light/dark mode added to the HTML. Toggle button in the toolbar; the
  page follows the OS colour-scheme preference on first load. Dark mode
  uses a hue-preserving inversion filter on the canvas, so the diagram
  re-themes without duplicating the SVG palette. The banner logo keeps
  its white pill and is unaffected.

## [1.0.20] — 2026-07-13
- Coeo logo relocated from the SVG canvas into the HTML banner, above
  the zoom toolbar, on a white pill so the navy artwork stays legible on
  the dark header. Diagram title/subtitle returned to the left margin.

## [1.0.19] — 2026-07-13
- Coeo logo moved to the upper-left; title and subtitle shifted right
  beside it.

## [1.0.18] — 2026-07-13
- Coeo logo added to the top-right of the title block (embedded, no
  external file needed).
- Version/date link removed from the diagram subtitle; the clickable
  version now lives only in the HTML banner.
- Part 3: "Configured as Selection 0…9 / ✱ / #" note re-wrapped onto
  three lines so it no longer runs into the destination-types panel.

## [1.0.17] — 2026-07-13
- Termination: red-drop label reworded from "VM off + no response" to
  "VM off / Call Hang up".

## [1.0.16] — 2026-07-13
- Voicemail box vertically centered on the "yes" arrow from the
  Is-Voicemail diamond; Extension rings and disconnect re-spaced below
  it with even gaps, the no-path, up-arrow, and red drop re-spanned to
  match.

## [1.0.15] — 2026-07-13
- Is-Voicemail branches set to conventional flowchart form: "yes" runs
  straight from the diamond's right-most tip into the Voicemail box;
  "no" drops from the bottom tip and elbows right into Extension rings.

## [1.0.14] — 2026-07-13
- Is-Voicemail branches made fully independent: "yes" now exits the
  diamond's upper-right edge, "no" the lower-right edge — previously
  both launched from the right vertex and overlapped for their first
  22px. Labels sit above their own arrows, clear of each other.

## [1.0.13] — 2026-07-13
- Termination: yes/no branches from the Is-Voicemail diamond redrawn as
  orthogonal elbows (the yes path had degraded into a flat squiggle);
  outcome labels moved from the far right to sit directly beside their
  arrows.

## [1.0.12] — 2026-07-13
- Termination column: yes-arrow into the Voicemail box smoothed to a
  single sweep; Extension box lowered for a proper gap, giving the
  upward "no answer" arrow (34px) and the red "VM off" drop (36px)
  standard lengths; no-curve and labels repositioned to match.

## [1.0.11] — 2026-07-13
- Part 3 termination re-spaced: header sits just below the section
  divider; main row (Greeting replayed → Timeout Destination →
  Is-Voicemail diamond) aligned on one axis; Voicemail / Extension /
  disconnect stacked with even 28px gaps; yes/no curves and the two
  vertical outcome arrows cleaned up with labels beside them.

## [1.0.10] — 2026-07-13
- Part 3 termination: disconnect box relocated directly below "Extension
  rings", aligned to the Voicemail/Extension column, fed by a straight
  red drop labeled "VM off + no response". Part 3 panels and Parts 4/5
  cascaded down to make room.

## [1.0.9] — 2026-07-13
- Part 3 termination logic corrected: Voicemail box is a true terminal
  (no path to disconnect). Is-Voicemail = No → extension rings → if
  unanswered, the call falls to that extension's voicemail box (new
  upward arrow). The call disconnects only when the extension's
  voicemail is turned off; disconnect node reworded accordingly.

## [1.0.8] — 2026-07-13
- Part 1 tooltips restyled: native browser titles replaced with custom
  scripted tooltips matching the OT gate panel (amber #fffdf7 fill,
  #e0a800 border), bold heading line, cursor-following placement clamped
  to the canvas edge. Requires script: works in the HTML and in the SVG
  opened directly; static image embeds fall back to no tooltip.

## [1.0.7] — 2026-07-13
- Hover tooltips added to all seven Part 1 stages (Provider, Trunk, DID
  Match, OT Gate, CLI, Range, Destination) using native SVG titles —
  each explains the stage with examples; cursor shows help pointer.

## [1.0.6] — 2026-07-13
- "Inherit" in the OT config panel is now clickable: it toggles an
  explanation callout (three OT states; Inherit re-applies the
  Server/Tenant rules; historical on/off-only note). Click the callout
  to close. First interactive element on the map.

## [1.0.5] — 2026-07-13
- "Where Operation Times is configured" panel: the Inherit arrow moved
  from the left edge to the horizontal center of the rows.

## [1.0.4] — 2026-07-13
- Arrowhead added to the Part 1 Destination → Part 2 pointer (stops just
  above the bus so the marker doesn't overlap it).

## [1.0.3] — 2026-07-13
- Part 1: the Destination → Part 2 feed line restyled to match the
  drill-down pointers (purple dashed) with an "expanded in Part 2 ↓"
  label, consistent with the Part 3/4/5 pointers.

## [1.0.2] — 2026-07-13
- Part 2: "Extension / Multi-User" node simplified to "Extension";
  legend and OT config panel wording updated to match.

## [1.0.1] — 2026-07-13
- Version stamp made clickable in both the SVG subtitle and the HTML
  header; it links to this CHANGELOG.md.
- Changelog completed with previously undocumented fixes (see additions
  under 0.6 and 0.9 below).

## [1.0] — 2026-07-13

First complete release. Version stamp added to the diagram subtitle and the
HTML header. README and CHANGELOG introduced.

### Content
- Removed all vendor-name references; restriction facts retained and
  reworded neutrally.

---

## Pre-release iterations (0.x)

### [0.9] — Layout polish
- Uniform ~28–30px arrows across all five parts (spine, columns, and all
  expansion flows); dependent elements reflowed.
- Part 1 spine rebuilt with even stage spacing; closed the oversized gap
  left by the fax-stage removal; stage labels vertically centered and
  renumbered 1–7 with no gaps (labels 4 and 6 lifted clear of the side
  boxes that were painting over them).
- Termination cluster in Part 3 shifted down so the section divider runs
  full width unobstructed (twice — the upward-fanning Voicemail box
  collides whenever Part 3 spacing changes).
- Drill-down arrow from the IVR digit map to Part 3 straightened and
  stopped at the divider; Part 2 legend moved right to clear its path.
- Divider added between the title block and Part 1; section framing
  (accent bars) added and subsequently removed by request.
- Inherit arrow inside the Part 1 "where OT is configured" panel
  lengthened; panel rows re-spaced to give the inheritance cascade a
  visible run.
- Overlap fixes: drill-down connector no longer routes through the Part 2
  legend; "nests"/"replay ×N"/invalid-loop annotations lifted off the
  curves they sat on; over-long legend line shortened to clear the gotcha
  column; Part 2 divider trimmed while the OT config panel extended past
  it (later restored to full width).
- Part 3 invalid-key loop-back rerouted as a clean orthogonal path into
  PLAY GREETING, and the disconnect-node curves replaced with labeled
  elbow arrows (both elements removed in a later revision — see 0.6).

### [0.8] — Part 5: Queue + ERG comparison
- Added Part 5: full queue flow (OT gate, max-callers check with
  full-redirect, waiting cycle with MOH and position announcements, Exit
  Digit, ring cycle, timeout/overflow).
- Queue vs ERG comparison panel: purpose, edition availability, members,
  callback, reporting, and shared features.
- Export checklist per Queue/ERG, including "record which type it is".

### [0.7] — Part 4: Ring / Dial Group
- Added Part 4: ring-all flow, no-answer path (Announce → Default
  Destination → Is-Voicemail fork), ERG extras panel, export checklist.
- Pointer labels added under the Part 2 Queue and Ring Group columns.

### [0.6] — Content corrections
- Removed all fax content (Auto Fax Detection stage in Part 1, Fax-to-Email
  destination chip in Part 3); spine stages renumbered.
- Removed the IVR-digit-map → Queue "nests" arrow in Part 2.
- Removed the invalid-key-press chain and its loop-back from the Part 3
  termination row (the Invalid Selection config field remains in the
  selections column).
- Removed the replay loop arrow (Play Greeting → Greeting plays); the
  replay behavior remains documented in the Play Greeting box text.
- Counted and removed all vendor-name occurrences on request (finalized
  in 1.0); Conference/Voicemail "no OT" placeholder removed in favor of a
  clean arrow.

### [0.5] — Operation Times accuracy pass
- Verified OT per object against documentation: present on DID, IVR,
  Queue, Ring/Dial Group, ERG; absent on Conference and Voicemail.
- Extension OT (opt-in via Enhanced Services) shown as a dashed
  conditional gate, then removed entirely per the deployment's convention;
  legend and config panel updated to match.
- Conference/Voicemail "no OT" placeholder later removed in favor of a
  clean arrow, with the fact retained in the legend.

### [0.4] — Format change
- Delivered as a single self-contained HTML file (inline SVG, offline,
  zoom in/out/reset toolbar); SVG retained alongside.

### [0.3] — Part 3: IVR expansion
- Added the IVR detail section: entry sequence, all selections 0–9 plus
  ✱ and # (each individually mapped), destination-type panel, Play
  Greeting replay counter, and the termination chain ending in disconnect.
- Clarified ✱/# as ordinary configurable selections; noted the distinct
  feature codes (✱304xxx greeting recording, ✱401/✱402 OT override).

### [0.2] — Operation Times as a repeating gate
- Reworked OT from a single spine step into a gate at every hop.
- Added the OT priority ladder panel (Open Days → Custom Destinations →
  Closed Dates → Default Destination, with the Is-Voicemail flag).
- Added the "where OT is configured" panel with Server/Tenant inheritance.
- Documented the double-divert gotcha (DID open, Queue closed).

### [0.1] — Initial map
- Generic DID → local destination flow: Provider → Trunk → DID match →
  processing checks → Destination + Value, fanning out to IVR, Queue,
  Ring Group, Extension/Multi-User, Conference, Voicemail.
- Billing distinction encoded: green "Local destinations" billed from the
  Service Plan vs blue Extension/Multi-User billed per the DID's E.164
  incoming price.

---

# Transcription panel (Deepgram Transcriber) — history

The transcription panel began as a standalone tool and was folded into this app in 2.10.0. Its standalone version history (1.0.0–1.23.0) is preserved here so the changes made to each component are visible in one place.

## [1.26.1] — 2026-07-17
### Changed
- Batch upload limit raised from 20 to 25 files.

## [1.26.0] — 2026-07-17
### Added
- "Attach to map" shows a pre-flight summary (counts to be attached vs
  left off) with Proceed / Cancel before writing anything to the map.

## [1.25.1] — 2026-07-17
### Changed
- After a batch attach, the panel signals the host app so it can confirm
  the attachment and offer a return to the map.

## [1.25.0] — 2026-07-17
### Changed
- Attaching a transcript to an IVR is now a per-file checkbox plus one
  "Attach to map" button at the top of the Transcripts panel: tick the
  files you want, pick each one's IVR, and attach them all at once.
  Auto-matched files are pre-ticked; unticked files are not attached.

## [1.24.1] — 2026-07-17
### Changed
- Raised the per-batch upload limit from 10 to 20 files (5 MB per file
  unchanged); the drop-zone hint and overflow message follow the new cap.

## [1.24.0] — 2026-07-17
### Changed
- Results panel redesign: each transcribed file is a distinct card with a titled
  header and a strong divider between files. The "Attach to IVR" control moved to
  the top of each card, above the transcript.
- The raw JSON response is hidden by default behind a "Show raw JSON" toggle
  button (flips to "Hide raw JSON").

## [1.23.0] — 2026-07-14

### Added
- Provider dropdown: choose Deepgram or OpenAI. Each provider has its own model list (gpt-4o-transcribe, gpt-4o-mini-transcribe, whisper-1 on OpenAI), language options (with Auto-detect on OpenAI), and API key — keys are held separately per provider for the session and swap with the dropdown.
- OpenAI transcription path: multipart upload to /v1/audio/transcriptions with friendly error mapping. Raw telephony files are wrapped in-browser into proper WAV containers for OpenAI (µ-law tag 7, A-law tag 6, linear PCM tag 1 — verified sample-identical via ffmpeg); .gsm reuses the WAV49 converter.
- Encrypted vault v2: one passphrase now protects both provider keys in a single AES-256-GCM blob. The blob is version-tagged and self-describing (stores its PBKDF2 iteration count), and iterations were raised 310,000 → 600,000 (current OWASP guidance). A legacy v1 blob still unlocks and is migrated to v2 on the next save. The vault remains disabled behind VAULT_ENABLED while security testing continues.

### Changed
- Smart formatting, Punctuate, and Diarize gray out under OpenAI (the first two are automatic there; diarization is not offered) and restore exactly when switching back to Deepgram.

## [1.22.0] — 2026-07-14

### Added
- GSM support: Asterisk/PBXware .gsm greetings (GSM 06.10 "toast" frames) are repacked in the browser into a standard WAV49 container Deepgram can decode — codec parameters are transcoded bit-exactly, verified sample-identical against an ffmpeg reference. No quality loss, no re-encoding.
- .gsm added to the file picker; the drop zone hint now lists GSM, µ-law, A-law, and SLN explicitly.

## [1.21.0] — 2026-07-14

### Added
- Raw telephony format support: headerless files (.ulaw/.ul, .alaw/.al, .sln/.sln16, .raw/.pcm) are detected by extension and sent with the explicit encoding and sample_rate parameters Deepgram needs to decode them — µ-law and A-law at 8 kHz, signed linear at 8 or 16 kHz. PBXware/Asterisk greetings and recordings now work as-is.
- The file picker now lists these extensions explicitly (they have no MIME type, so accept="audio/*" alone hid them; drag-and-drop always worked).

## [1.20.0] — 2026-07-14

### Changed
- Language is now a dropdown of supported codes instead of a free-text field, preventing typos from failing a batch. The options adapt to the selected model: nova-3 offers its ten languages plus Multilingual auto-detect; nova-2 lists 40+ languages and regional variants; nova-2-phonecall offers the English variants; general has the legacy set.
- Switching models keeps the chosen language when the new model supports it, otherwise falls back to English.

## [1.19.3] — 2026-07-14

### Changed
- Dot vertical travel deepened 45 px → 60 px (y 30–90); crossing is now ~7.2 s on the 779-unit path.

## [1.19.2] — 2026-07-14

### Changed
- Drop zone hint now lists more of Deepgram's supported formats: AAC, Opus, WebM, AMR, and PCM/µ-law alongside the original five.

## [1.19.1] — 2026-07-14

### Changed
- Dot vertical travel deepened again, 30 px → 45 px (y 37.5–82.5); crossing is now ~6.9 s on the 742-unit path.

## [1.19.0] — 2026-07-14

### Changed
- The traveling dot's ride deepened from 20 px to 30 px of vertical travel (undulating between y 45 and y 75 on the banner's midline) — a more pronounced wave motion on the way to the document.
- Stale speed comment corrected: the path is 714 units, crossing in ~6.6 s (~8.6 s full loop with the writing pause).

## [1.18.7] — 2026-07-14

### Changed
- Pre-morph wake tint lightened from full amber (#e0a800) to a light amber (#f5cf6b), matching the softness of the pale mint on the post-morph side. The dot itself stays full amber.

## [1.18.6] — 2026-07-14

### Changed
- Reverted 1.18.5: the traveling dot, its halo, and the pre-morph wake tint return to amber.

## [1.18.5] — 2026-07-14

### Changed
- The traveling dot (and its halo) recolored from amber to orange (#ff8c00); the pre-morph wake tint follows it so the glow still matches the traveler.

## [1.18.4] — 2026-07-14

### Changed
- Post-morph wake lightened further to a pale mint green (#a8e6c4).

## [1.18.3] — 2026-07-14

### Changed
- Post-morph wake color lightened from the word's dark green to a light green (#58d68d), so the glow reads as a soft halo around the darker green text.

## [1.18.2] — 2026-07-14

### Changed
- The proximity glow now changes color at the halfway morph: bars blend toward amber while the traveler is a dot, ramp through the same 45–55% crossfade zone as the dot-to-word morph, and blend toward the word's green for the rest of the journey.

## [1.18.1] — 2026-07-14

### Changed
- Dot-passage swell made dramatic: bars now surge toward 110 px (nearly full banner height) as the dot passes, overriding the cadence dip instead of multiplying into it — the 1.18.0 version was barely visible because tall bars had no headroom under the cap.
- Bars within the dot's proximity also shift color, blending from waveform blue to the dot's amber and back as it passes, and light to full opacity at the center.

## [1.18.0] — 2026-07-14

### Added
- Dot-passage swell: waveform bars near the traveling dot rise as it passes over them — a gaussian boost of up to 35% that follows the dot across the wave, like the voice physically pushing through toward the document. Clamped so no bar exceeds the banner.
- The swell rides through the existing CSS cadence (a per-bar `--sw` factor multiplied inside the keyframes), so burst rhythms and the passage effect compose instead of fighting.

## [1.17.1] — 2026-07-14

### Removed
- The live per-frame "talking" waveform engine from 1.17.0, reverting to the pure-CSS speech cadence of 1.16.0: fixed voice-profile silhouette with each syllable burst pulsing on its own rhythm and faintly shimmering pauses.

## [1.17.0] — 2026-07-14

### Added
- Live "talking" waveform: bar heights are now computed every frame (~30 fps) from randomly spawned syllable bursts with attack/decay envelopes, a drifting focus like moving formants, alternating speech phrases (1.5–4 s) and genuine silences (0.45–1.5 s), and a faint breath-noise floor. The wave never repeats.
- The loop idles while the banner is hidden (during a batch) or the tab is in the background, matching the traveling dot.

### Changed
- With reduced motion enabled — or if JavaScript fails — the static voice-profile silhouette from 1.15/1.16 remains as the fallback.

## [1.16.0] — 2026-07-14

### Changed
- Speech-cadence movement: the uniform gliding crest is replaced by per-burst rhythms — each syllable burst pulses on its own period (1.1–2.0 s) and offset, blooming outward from its center, so different parts of the wave "speak" at different moments.
- Pause bars between bursts now only shimmer faintly (shallow 3.6–4.6 s movement) instead of breathing fully, deepening the contrast between speech and silence.
- Still pure CSS animation — per-bar custom properties, no per-frame JavaScript.

## [1.15.0] — 2026-07-14

### Changed
- The idle waveform now has a voice-like profile instead of a smooth sine: eight syllable bursts of varying width and intensity, quiet pauses between them, and deterministic per-bar jitter — the silhouette of someone speaking. Peak stays at 100 px.

## [1.14.4] — 2026-07-14

### Changed
- Microphone and document glyphs moved out to the banner edges, and the wave-to-glyph margin set to a clean 30 px on both sides.
- Net result: the wave widened from ~636 px to ~660 px at full width; the traveling dot's path re-based to the new geometry.

## [1.14.3] — 2026-07-14

### Changed
- Wave peak raised to 100 px (bar heights now 56–100 px) and the banner grew to 120 px to hold it.
- Microphone, document, and the traveling dot's path re-centered on the taller banner's midline; the dot still lands on the document's first text line.
- Audio particles rise farther (110 px) to match the taller scene.

## [1.14.2] — 2026-07-14

### Changed
- Waveform bars slimmed to 3 px pills with fully round caps — a finer, more circular sine wave instead of rectangles.
- Spacing now distributes evenly across the span (space-between), so the wave still reaches edge to edge with thin bars.
- A clear margin added between the wave and the microphone/document glyphs on both sides.

## [1.14.1] — 2026-07-14

### Changed
- The idle waveform now spans nearly the full banner: bars flex to fill the space between the microphone and the document, ending just shy of each glyph instead of sitting in a narrow centered strip.
- Bar count raised from 48 to 64 so the wave stays fine-grained at the wider span.

## [1.14.0] — 2026-07-14

### Added
- Batch limits to keep API calls within reason: a maximum of 10 files per queue and 5 MB per file. Oversized files and overflow beyond the cap are skipped on add, with a status message naming what was skipped and why.
- "New batch" button in the Transcripts panel: clears the queue, results, and status in one click and scrolls back to the drop zone for the next run.

### Changed
- Drop zone hint now states the limits (10 files per batch, 5 MB each).
- "Clear queue" and "New batch" share one reset routine.

## [1.13.0] — 2026-07-14

### Changed
- The mic-to-document traverse slowed from ~1.9 s to 6 s (speed 340 → 108.2 px/s), making each phase of the journey easy to follow.
- The traveling word turns green (#1e8449) at the morph, so the amber dot visibly becomes green text; the full loop is now 8 s (6 s travel + 2 s writing).

## [1.12.0] — 2026-07-14

### Changed
- Waveform bars recolored from muted amber to soft blue, so the layers read as blue = audio, amber = recognized text.
- The breathing wave now travels left to right (microphone toward document), matching the direction of the traveling dot and word.

## [1.11.0] — 2026-07-14

### Changed
- The traveling dot now morphs mid-journey: it leaves the microphone as a dot and crossfades between 45% and 55% of the path into the word "hello", which rides the rest of the wave and lands on the document.
- `prefers-reduced-motion`: the static scene now shows the word "hello" resting at 65% of the path (the outcome) instead of the dot.

## [1.10.0] — 2026-07-14

### Added
- Audio particles on the idle banner: nine faint dots drift up from the bottom on staggered 5–8 s cycles and dissolve mid-rise into ghost letters (a, e, t, o, s…) that fade out before the top — speech turning into text in the background of the scene.
- Particle positions are spread across the middle of the banner, clear of the microphone and document glyphs; negative animation delays distribute them through their cycles from the first frame.
- `prefers-reduced-motion`: particles are hidden entirely; the static scene stays clean.

## [1.9.1] — 2026-07-14

### Changed
- Removed the "Listening" watermark from the idle banner; the animation now stands on its own.

## [1.9.0] — 2026-07-14

### Added
- Self-writing transcript on the idle banner: when the traveling dot lands on the document glyph, its text lines draw themselves in one by one while an amber cursor blinks; when the dot departs, the page wipes clean for the next arrival.
- Writing is synchronised to the dot in the animation loop rather than running on an independent timer, so the two can never drift apart.
- `prefers-reduced-motion`: the document renders fully written, cursor hidden, nothing moves.

### Changed
- The dot's rest at the document extended from 0.7 s to 2 s so the page finishes writing before the dot leaves.

## [1.8.0] — 2026-07-14

### Added
- Traveling dot trace on the idle banner: an amber dot leaves a microphone glyph, rides a wave path across the breathing bars at 340 px/s (matching the PBXware call-flow trace), fades in and out at the ends, and lands on a document glyph before looping.
- The trace pauses automatically while the banner is hidden (during a batch) or the tab is in the background, and resumes without drift.
- `prefers-reduced-motion`: the dot renders once, stationary at mid-path.

## [1.7.0] — 2026-07-14

### Added
- Idle-state "listening" banner above the API key panel: 48 waveform bars breathe slowly in a travelling wave while the tool waits, with a quiet "Listening" watermark.
- The banner hides while a batch is transcribing (the conversion animation takes over) and returns when the batch completes.
- `prefers-reduced-motion`: the banner renders as a static mid-height waveform with no movement.

## [1.6.0] — 2026-07-14

### Added
- Audio-to-text conversion animation while transcribing: a pulsing waveform flows through animated chevrons into a document whose text lines write themselves in, looping until the batch completes.
- Full `prefers-reduced-motion` support: with reduced motion enabled, all animation stops and the strip renders as a calm static graphic.

### Changed
- The small inline spinner in the status line was removed; the conversion animation now carries the in-progress state.

## [1.5.0] — 2026-07-14

### Changed
- Passphrase vault and lock screen temporarily disabled behind a feature flag (`VAULT_ENABLED`) while security testing continues. Key entry reverts to per-session paste.
- Any previously saved encrypted key remains in browser storage untouched and becomes usable again when the flag is re-enabled.

## [1.4.0] — 2026-07-14

### Added
- Multiple file support: drop or select several audio files at once; they collect in a queue with per-file size, status, and remove buttons.
- "Transcribe all" processes the queue one file at a time with live progress; failed files stay in the queue for one-click retry.
- Per-file transcript blocks, each with its own copy, download-as-.txt, metadata, and raw JSON inspector.
- "Copy all" and "Download all" combine every transcript into one text with filename headers.
- Clear queue button.

### Changed
- Results panel reworked from a single transcript to one block per file.

## [1.3.0] — 2026-07-14

### Added
- Clickable version badge in the header opening this changelog.

## [1.2.0] — 2026-07-14

### Added
- Password-first lock screen: when a saved key exists, the app asks for the password before anything else; the correct password fetches and loads the key automatically.
- Wrong-password shake feedback and a "Skip — enter the API key manually" escape hatch so a forgotten password never blocks use.

## [1.1.0] — 2026-07-14

### Added
- Encrypted key vault: optionally remember the API key on this device, protected by AES-GCM with a PBKDF2-derived key (310,000 iterations). The key is never stored in plaintext and never written into this file.
- Save key / Unlock / Forget controls in the API key panel.

### Changed
- Key panel hint text updated to describe encrypted storage.

## [1.0.0] — 2026-07-14

### Added
- Initial standalone test bench for the Deepgram pre-recorded transcription API.
- Runtime API key field (memory only), model and language options, smart formatting, punctuate, and diarize toggles.
- Drag-and-drop or click-to-choose audio upload with file details.
- Transcript view with speaker labels when diarization is on, copy and download-as-.txt buttons, confidence and duration metadata, and a raw JSON inspector.
- Friendly error messages for invalid keys (401), Deepgram errors, and network/CORS failures.
