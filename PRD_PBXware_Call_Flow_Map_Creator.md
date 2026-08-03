# PRD: PBXware Call Flow Map Creator — Product Requirements Document

**Version:** 1.0 · 2026-07-24
**Status:** Living document
**Reflects application build:** v2.43.0

---

## 1. Overview

The PBXware Call Flow Map Creator is a single-file, fully offline web application that turns a PBXware configuration export into a clear, traced diagram of how inbound calls route through a customer's phone system. A technician exports the relevant tabs from PBXware as an `.xlsx` workbook, loads it into the tool, picks a DID (or a queue/conference), and the tool traces and draws the complete path — from the dialed number through Operation Times gates, IVRs, every menu option, dial groups, ring groups, queues, extensions, voicemail and conference bridges — down to each terminating destination.

Everything runs in the browser. Nothing is uploaded to a server, so it can be used on customer data safely, deployed as a static page (GitHub Pages), embedded in Confluence, or simply opened from a local file.

A companion transcription workbench is built in: it transcribes IVR greeting audio (via a speech-to-text service) and attaches the resulting text to the matching IVRs in the map, so a map can document not just *where* calls go but *what each menu says*.

---

## 2. Goals

1. **Help new technicians get familiar with call routing.** Give a new tech a fast, self-service way to understand how any customer's PBXware system routes calls, shortening onboarding and reducing dependence on senior staff to "walk through" a config.
2. **Create a visual representation of call-flow routing.** Produce accurate, readable diagrams of end-to-end routing directly from real PBX data, suitable for review, documentation, hand-off and customer-facing explanation.
3. **Transfer current call flow and IVR recordings into voice-IVR prompting.** Capture the existing IVR audio as text transcripts so current prompts can be documented, reviewed and repurposed — a stepping stone toward standardizing and (re)generating voice IVR prompts.
4. **Ultimate goal — connect to PBXware via API and automate call-flow mapping.** Move from manual `.xlsx` exports to a direct API connection that pulls configuration automatically and generates or refreshes maps on demand, keeping documentation continuously in sync with the live system.

---

## 3. Background and problem

PBXware call routing is expressed across many interrelated configuration tables — DID routing, IVRs and their per-digit options, dial groups, ring groups, queues and agents, extensions, voicemail, caller ID, conference bridges and time-based (Operation Times) rules. Understanding "what happens when someone calls this number" means mentally joining all of those tables and following the chain by hand. That is slow, error-prone, and hard for a new technician to do reliably.

The result is that call-flow knowledge lives in people's heads, onboarding is slow, and documentation (when it exists) drifts out of date. A tool that reads the real configuration and draws the actual path removes the guesswork and makes the routing legible to anyone.

---

## 4. Target users

- **New technicians / trainees** — the primary audience: learning how routing works, per goal 1.
- **Support and provisioning staff** — quickly checking or explaining how a specific DID behaves.
- **Sales / solution engineers** — showing a customer their own call flow.
- **Documentation** — producing shareable diagrams (PDF/SVG/Visio) and prompt inventories.

---

## 5. Current capabilities (delivered in v2.43.0)

**Ingestion**
- Reads PBXware `.xlsx` exports directly in the browser (its own workbook reader; no external service).
- Understands the routing-relevant tabs: DID routing, IVR (with an optional greeting-transcript column), Dial Group, ERG (ring groups), Extension, Voicemail, Caller ID, Operation Hours, Queues, Agents and Conference.

**Tracing engine**
- Traces a DID to every terminating destination, following IVR options recursively.
- Resolves destination types: IVR, dial group, ring group, queue (with agents), extension, voicemail, conference bridge, deny/agents, and unassigned.
- **Operation Times** support, including per-day custom hours: a DID can fan out to a different destination for each weekday plus a closed/after-hours path, with the common hours shown on the gate and per-day exceptions labeled only where they differ.
- Loop detection and, on export, an "active routes only" mode that prunes paths that never reach a real endpoint.

**Visualization**
- Renders a traced call map as SVG, color-coded by destination type, in horizontal or vertical orientation.
- IVR greeting transcripts render as callouts on their IVR node.

**Navigation and guidance**
- "Map a DID," "Map a Queue" and "Map a Conference" pickers.
- Destination-type filter chips to narrow the DID list.
- An on-panel notification and tooltip prompting the user to select a call route.

**Transcription workbench (integrated)**
- Transcribes uploaded IVR greeting audio to text via a speech-to-text service.
- Attaches a transcript to one or more IVRs in the map (one transcript can be applied to several IVRs, e.g. per-day menus that share a greeting).
- A passphrase vault (AES-256-GCM / PBKDF2) exists for protecting stored credentials; it is currently disabled behind a flag pending re-enablement.

**Export**
- Download options dialog with additive format checkboxes: **PDF**, **SVG**, **Interactive HTML**, **Transcripts (.txt)** and **Visio (.vsdx)**.
- The **Visio export** produces an editable drawing — one shape per node (color-coded) with glued dynamic connectors — generated entirely in the browser from a bundled Visio template; it opens in Microsoft Visio for rearranging/restyling.
- Scope options: current map, all DIDs, or a hand-picked set; single file or a bundled `.zip`.

**Samples and onboarding aids**
- Two built-in, fully sanitized demo workbooks — "Property Management" (general demo: IVR, dial group, queue, conference, extension, agents) and "Office Dental" (per-day Operation Times) — with greeting transcripts pre-filled so the features are visible out of the box.
- Each sample can be **Attached** (loaded straight into the tool) or **Downloaded**.

**Deployment and delivery**
- Single self-contained HTML file; runs offline.
- Works standalone, as a static GitHub Pages site, or embedded in Confluence, with embed-aware download handling for the cross-origin iframe case.

**Engineering discipline**
- Semantic versioning, dual changelog (in-app and `CHANGELOG.md`), README version line, and syntax validation on every change.

---

## 6. Functional requirements

- The tool **must** parse a PBXware `.xlsx` export entirely client-side and never transmit customer data off the machine.
- It **must** trace a selected DID to all reachable terminating destinations and render the result as a diagram.
- It **must** represent time-based routing, including per-weekday destinations and a closed path.
- It **must** let a user export the current map (and optionally all maps) to at least PDF, SVG, Interactive HTML and Visio.
- It **must** allow IVR greeting audio to be transcribed and the transcript attached to the corresponding IVR node(s).
- It **should** ship with sanitized sample data that demonstrates each major feature.
- It **should** degrade gracefully when embedded in a restricted (iframe) context, still allowing the user to obtain exported files.

---

## 7. Non-functional requirements

- **Privacy:** no server round-trips for customer configuration; safe to run on live customer data.
- **Portability:** a single HTML file with no install and no build step for the end user.
- **Offline:** full core functionality (parse, trace, render, export) works with no network. (Audio transcription requires the speech-to-text service.)
- **Accuracy:** the rendered path must faithfully reflect the source configuration.
- **Maintainability:** versioned, changelogged, and validated on each change.

---

## 8. Architecture and constraints

- **Client-side pipeline:** in-browser workbook reader → model builder → tracer → SVG renderer → export pipeline. No backend.
- **Embedded transcriber:** delivered as an isolated in-page component; communicates with the host map via messaging to attach transcripts.
- **Export packaging:** SVG/HTML/Visio bundles and the Visio `.vsdx` package are assembled and zipped in the browser.
- **Known constraint — iframe/embed downloads:** inside a cross-origin iframe (e.g. Confluence) or the artifact preview, the browser blocks the native Save dialog and can block scripted downloads. The tool works around this with click-to-download links and a new-tab delivery path; the fullest download experience is in the standalone file.
- **Known constraint — Visio fidelity:** Visio shapes use flat fills (not the diagram's rounded/gradient styling) and Visio re-routes connectors, so the initial Visio layout can differ from the on-screen diagram until Visio re-flows it.

---

## 9. Deployment modes

1. **Standalone file** — opened directly in a browser; best for the full download/export experience.
2. **GitHub Pages** — hosted static page for easy sharing.
3. **Confluence embed** — embedded in team documentation, with embed-aware download handling.

---

## 10. Out of scope (current release)

- No live/direct connection to PBXware (all input is via manual `.xlsx` export today).
- No writing back to or editing of PBX configuration.
- No generation of actual voice audio (the transcriber captures text from existing audio; it does not synthesize new prompts yet).

---

## 11. Roadmap

**Near term**
- Re-enable the transcriber passphrase vault (increase key-derivation iterations; ensure the stored blob carries its version and parameters) before shipping further transcriber changes.
- Confirm Visio `.vsdx` output opens cleanly in Microsoft Visio across versions; refine shape styling/connector routing based on feedback.

**Mid term (supports goal 3 — voice IVR prompting)**
- Build out the transcription workbench into a first-class prompt inventory: capture every IVR's current audio as text, organize by IVR, and export a prompt sheet suitable for review and re-recording/standardization of voice prompts.
- Tighten the transcriber ↔ map integration so a fully documented map (routing + prompt text) is a single artifact.

**Long term (goal 4 — automation)**
- **PBXware API connection:** authenticate to a PBXware instance and pull the configuration directly, removing the manual `.xlsx` export step.
- **Automated mapping:** generate or refresh call-flow maps on demand (and potentially on a schedule), keeping documentation continuously in sync with the live system.
- **Change awareness:** surface differences between the last-known map and the current live configuration.

---

## 12. Success metrics

- **Onboarding time:** reduction in time for a new tech to correctly explain a customer's call flow.
- **Adoption:** number of techs/teams using the tool and maps produced.
- **Accuracy/trust:** low rate of "the diagram didn't match reality" corrections.
- **Documentation freshness:** proportion of customers with a current, tool-generated call-flow map (a lead indicator toward the automation goal).

---

## 13. Risks and open questions

- **API access (goal 4):** availability, scope and authentication model of the PBXware API, and whether it can be reached from the environments where the tool runs.
- **Embed/download limits:** cross-origin iframe restrictions cap the in-embed download experience; the standalone file remains the reference environment.
- **Visio compatibility:** needs validation across Visio versions.
- **Sample/customer data hygiene:** any bundled or shared data must remain fully sanitized (no real names, MACs, PINs, DIDs or domains).
- **Voice prompting scope (goal 3):** clarify how far the tool should go — documenting/exporting prompt text versus generating new voice audio.

---

## 14. Build snapshot

Current application build **v2.43.0**. Notable recent milestones: conference bridges as a full destination type; per-day custom Operation Times; greeting transcripts and multi-IVR transcript attach; two sanitized demo workbooks with attach-into-tool; embed-aware downloads; and one-click editable **Visio (.vsdx)** export. Change history is maintained in the in-app changelog and `CHANGELOG.md`.
