# Project Context — PBXware Call Flow Map Creator

**App version:** v2.43.4 · 2026-07-24 · single-file offline HTML (`pbxware_master_call_flow.html`)

## What this is
A fully offline, single-file web app that parses a PBXware `.xlsx` configuration export in the browser and renders traced call-flow diagrams: DID → Operation Times gates → IVRs (every option) → dial groups / ring groups / queues (with agents) / extensions / voicemail / conference bridges, down to each terminating destination. Nothing is uploaded anywhere, so it is safe on customer data. An embedded transcription workbench (currently v1.27.1) converts IVR greeting audio to text and attaches transcripts to one or more IVRs on the map.

## Goals
1. Help new techs get familiar with call routing. 2. Create a visual representation of call-flow routing. 3. Transfer current call flow and IVR recordings into voice-IVR prompting. 4. Ultimately connect to PBXware via API and automate call-flow mapping. (Full detail in the PRD.)

## Architecture (one file, no server)
Own in-browser XLSX reader (DOMParser over the unzipped workbook) → `buildModel` (tabs: DID routing, IVR incl. optional Greeting Transcript column, Dial Group, ERG, Extension, Voicemail, Caller ID, Operation Hours incl. per-day custom rows, Queues, Agents, Conference) → `traceDID` → `renderSVG`. Exports: PDF (jsPDF), SVG, interactive HTML, transcripts `.txt`, and editable Visio `.vsdx` (bundled template skeleton + generated `page1.xml`, zipped in-browser). Two sanitized demo workbooks — "Property Management" (single weekly Operation Times schedule) and "Office Dental" (per-day custom Operation Times) — are embedded base64 with Attach (loads straight into the tool) and Download template actions. The transcriber runs in a srcdoc iframe and posts transcripts to the map via messaging.

## Deployment modes and known constraints
Standalone file (full experience, native Save dialog), GitHub Pages, and Confluence iframe embed. Inside a cross-origin iframe the native Save dialog is blocked; downloads fall back to direct user-click links / new-tab delivery. Visio output uses flat fills and Visio re-routes connectors on open. The transcriber's passphrase vault is currently disabled (`VAULT_ENABLED=false`) — re-enable before further transcriber work (bump PBKDF2 iterations 310k → 600k; v2 blob already stores iteration count + version tag; disabling does not purge dormant `localStorage['dgVault.v1']` ciphertext).

## Engineering conventions (apply to every change)
Semantic versioning: patch = tweak/fix, minor = new feature. Dual changelog: in-app block plus `CHANGELOG.md`, entry format `## [X.Y.Z] — YYYY-MM-DD` with `### Added/Changed/Fixed/Removed`. README carries a `**Version:** X.Y.Z · date` line. Validation every change: `node --check` on all inline script blocks (block 0 — minified jsPDF/pako — always fails; blocks 1–3 must pass) and the SVG renderer region must remain byte-identical unless intentionally changed. Workflow: visual/UI changes get a mockup or preview before implementation; ambiguity is resolved with numbered options; changes ship complete in one pass (code + version bump + both changelogs + README).

## Related artifacts
`PRD_PBXware_Call_Flow_Map_Creator.md` (goals, requirements, roadmap), `CHANGELOG.md`, `README.md`, application-flow diagram (`call_flow_map_creator_flowchart.html` / `.mermaid`). Sibling tools outside this file: the PBXware Config Template workbook (v1.3.1) aligned to the parser's column positions, and the standalone Deepgram Transcriber test bench.
