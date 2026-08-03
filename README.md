# PBXware — Call Flow Map Creator

**Version:** 2.44.1 · 2026-07-24 (version link lives in the HTML banner; Coeo logo in the HTML banner)

A single-file, self-contained visual reference for how inbound calls travel
through PBXware: from the provider trunk, through DID processing and
Operation Times gates, to every local destination type — with the branching
destinations (IVR, Ring/Dial Group, Queue/ERG) fully expanded.

## Files

| File | Purpose |
|---|---|
| `pbxware_master_call_flow.html` | Primary deliverable. Opens on a launcher: view the reference map, or upload exports to map your own system. Open in any browser. Works offline, includes zoom in/out/reset controls, a light/dark mode toggle (light by default) and a clickable "Inherit" explainer and hover tooltips across all five parts. |
| `pbxware_master_call_flow.svg` | The raw diagram. Use for embedding in wikis, printing, or further editing. |

| `README.md` | This file. |
| `CHANGELOG.md` | Revision history. |

## Embedding in Confluence (or any page)

The tool works inside an `<iframe>` (Confluence's iframe / HTML macro, a wiki,
a portal, etc.). It detects being framed automatically; you can also force it
with an `#embed` (or `?embed`) flag on the URL. In embed mode the layout goes
fluid and the banner shrinks.

The one thing an iframe can't do by itself is grow to fit its content, so the
page **reports its height to the host**. You have three options:

1. **Host uses iframe-resizer.** The MIT iframe-resizer v4 child is bundled and
   started automatically in embed mode. If the Confluence page already runs the
   iframe-resizer parent script, sizing just works — no extra code.

2. **Tiny listener snippet.** The page also posts a plain message you can act on.
   Add this once on the host page (set your iframe's `id`):

   ```html
   <iframe id="cfmap" src=".../pbxware_master_call_flow.html#embed"></iframe>
   <script>
     window.addEventListener('message', function (e) {
       if (e.data && e.data.type === 'cfmap:height') {
         var f = document.getElementById('cfmap');
         if (f) f.style.height = e.data.height + 'px';
       }
     });
   </script>
   ```

3. **Manual size.** When you can't add any host script, set the size yourself in
   the URL: `...#embed&h=1400` (and optionally `&w=1200`). The frame uses those
   pixel dimensions and reports the fixed height.

## Transcribe audio panel

The **Transcribe audio** button in the "map my system" view (next to Download /
Clear file) opens an embedded copy of the Deepgram Transcriber (v1.23.0). Use it
to convert call recordings and PBXware/Asterisk greetings to text — Deepgram or
OpenAI, with telephony formats (`.gsm`, `.ulaw`, `.alaw`, `.sln`, `.raw`)
handled directly.

The panel is loaded inside an isolated frame, so it shares none of the map's
styling or scripts. The map stays fully offline; the panel only reaches the
network (Deepgram/OpenAI) when you actually run a transcription, and you paste
your API key each session. The transcriber's encrypted key vault is disabled by
default (`VAULT_ENABLED=false`).

After a transcript is produced you can **attach it to an IVR**: the panel
auto-suggests the IVR by matching the audio filename to the greeting name
(keyed on the IVR number) and lets you confirm or override via a dropdown. The
attached greeting text then becomes part of that IVR's call-map diagram details
shown in full as a ❝ quote callout beside the IVR node (linked by a leader line) and included in the SVG/PDF/zip exports.
This applies to IVR nodes only, and transcripts persist locally and re-attach by
IVR number when you re-upload the workbook. The panel follows the map's
light/dark theme.

## Structure of the map

**Part 1 · DID processing spine** — the seven ordered steps every inbound
call passes through: Provider → Trunk → DID Match → Operation Times gate →
CLI Validation & Routing → DID Range rule → Destination + Value. Side
panels show the Operation Times evaluation priority (Open Days → Custom
Destinations → Closed Dates → Default Destination) and where OT is
configured, including the Server/Tenant inheritance chain.

**Part 2 · Downstream local destinations** — the five destination columns a
DID can hand a call to: IVR, Queue, Ring/Dial Group, Extension,
and Conference/Voicemail. The OT gate repeats in front of every object that
has one (DID, IVR, Queue, Dial Group, ERG); Extension/Multi-User,
Conference, and Voicemail have none. Color coding: green = "Local
destinations" billed from the Service Plan; blue = Extension billed per the DID's E.164 incoming price.

**Part 3 · The IVR node, expanded** — the entry sequence (OT gate, Ringing
Type/Preamble, greeting), all twelve selections (0–9, ✱, #) each with its
own Destination Type + value, the Play Greeting replay counter, and the
termination chain: Timeout Destination → Is-Voicemail fork → still no
response → call disconnected.

**Part 4 · The Ring / Dial Group node, expanded** — ring-all flow,
no-answer path (Announce → Default Destination → Is-Voicemail fork), plus
the Enhanced Ring Group (ERG) extras panel.

**Part 5 · The Queue node, expanded** — max-callers/full-redirect check,
the waiting cycle (MOH, position announcements, ring cycle), Exit Digit,
timeout/overflow — plus a Queue vs ERG comparison covering purpose, edition
availability, members, callback, and reporting.

## Key facts encoded in the map

- Operation Times is a **repeating gate**, not a single step. A call can
  clear the DID's OT during business hours and still be diverted by a
  Queue whose own OT is closed. Trace OT at every hop.
- OT exists on: DID, IVR, Queue, Ring/Dial Group, ERG. It does not exist
  on Extension, Conference, or Voicemail (per this deployment).
- `✱` and `#` are ordinary, fully configurable IVR selections — PBXware
  reserves nothing for them.
- The IVR's termination chain: greeting replayed N times → Timeout
  Destination. An unanswered extension falls to its own voicemail box;
  the call disconnects **only if that voicemail is turned off**.
- Ring Groups were renamed **Dial Groups** in v6.5. Business Edition (v6+)
  cannot create new Queues; ERG is used there instead. A destination
  number alone does not reveal whether it is a Queue or an ERG.

## Next phase: data-driven map

The current map is generic. Each part ends with an export checklist listing
the exact fields needed to replace generic nodes with the real system:

- **DIDs** — CSV export (`nr1`, `dest`, `ext`, `did_name` are the key columns)
- **IVRs** — per-selection destination map, Play Greeting N, timeout
  destination + Is-VM flag, OT state
- **Queues / ERGs** — members, strategy, max callers, exit digit,
  timeout/overflow destination, and *which type each object is*
- **Ring/Dial Groups** — members, Default Destination + Is-VM, OT state
- **Extensions** — number + name (labels for the leaves)

With those, the map upgrades to: fully traced multi-level paths per DID,
dead-end and orphan detection, reverse lookup by extension, and
departmental grouping.

## Open items

- Re-enable and re-verify the transcriber's encrypted key vault
  (`VAULT_ENABLED=true`) once security testing is complete.
- PBXware edition and version of the target system (Business /
  Multi-Tenant / Contact Center; v6/7/8) — affects Dial Group vs Ring
  Group naming, ERG availability, and Queue creation.
- Preferred trace depth for the data-driven version: first hop only, or
  full nested tree down to individual extensions.
