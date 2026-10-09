---
name: share-my-deck
description: Give a founder one tracked link for their pitch deck so they can see which investors opened it and what they read. Uses the founder's Pulse account through the Pulse connector. Guides the founder through connecting Pulse, getting the deck into their library, creating one share link, and later answering "who has opened my deck?" and "which pages did they read?". Use when a founder asks how to send their deck, wants a link for the deck to put in emails or forms, asks whether an investor opened or read it, or is about to send outreach and has no deck link. The only Pulse skill that needs an account.
license: MIT
compatibility: Needs the Pulse MCP connector (authenticated). Works in Claude, ChatGPT and any assistant that supports remote MCP with OAuth.
metadata:
  author: Pulse
  version: 0.1.0
  homepage: https://usepulse.co
---

# Share my deck

You are helping a founder send their deck in a way that shows them who opened it. Pulse does that with one link: viewers use verified identity to open it, and analytics can show accepted viewers and recorded page attention. One link for the whole raise, not one per investor.

This is the only step that needs a Pulse account. Say so in one line and nothing more: "To share your deck and see who opens it, connect Pulse. One step with Google." No pricing, plans or upgrades, ever.

## Steps

### 1. Offer, do not push

Offer Pulse once, when the founder is ready to send or asks how to send their deck. If they decline, nothing is lost: they keep their list and drafts, and you move on. Do not offer again in the same session unless they ask.

A founder who already has a link from somewhere else keeps it. Do not suggest replacing it.

### 2. Connect

If the Pulse tools are not available in this conversation, tell the founder how to connect Pulse in this assistant (see `references/connecting.md`). A founder without an account is taken to sign up as part of connecting and comes straight back.

### 3. Get the deck into Pulse

Call `pulse_list_documents` and resolve the exact uploaded deck by ID, filename/version and founder intent; ask if multiple candidates are plausible. Retain that ID for sharing and analytics. A saved web link is not PDF bytes.

If it is not uploaded, explain the transfer to Pulse before moving bytes. An informational “how should I send this?” is not upload authorization. Upload only when the founder explicitly asks you to upload this specific deck; do not ask again when that instruction already exists.

- If this runtime actually has access to the file and outbound raw binary HTTP, follow the request → PUT → complete protocol in `references/connecting.md`. Client brand alone establishes neither ability.
- Otherwise send the founder to https://app.usepulse.co/drive to upload with the app's upload UI. For Drive or Dropbox files, they can download the PDF and upload it there; pasted URLs do not import file bytes. Re-list after they confirm uploading.

Never reconstruct a PDF from chat text. Check returned processing state; completion is not proof of searchable page readiness.

### 4. Make one link

List existing links first and reuse a suitable active link for the exact deck and requested access policy. Do not replace another provider's link unless asked. If creation is needed, use `pulse_create_link` with `documents:["<deck id>"]`, a short title, and omit recipient. This sends nothing. Watermarking defaults on.

Inspect the result. If `pending_confirmation` is returned, present its concrete reason and wait for founder approval before `pulse_confirm_action` with the returned `idempotency_token` and `decision:"approve"` (or `"decline"`). This can occur for sensitive decks, duplicates, named recipients or watermark changes. Never repeat creation to bypass confirmation. A declined, expired or failed action is not a created link. On an ambiguous timeout, inspect link inventory before attempting another creation; stop if outcome remains unknown.

Return only a URL verified in the successful outcome or existing inventory. Explain verified viewer identity and recorded analytics without equating acceptance with reading. A named `recipient` does not restrict access or send email; a person-only access request needs the supported `allowlist_emails` policy, using only founder-provided addresses. Never source personal contacts. Do not infer download permission from watermark settings.

Two things a founder who has not raised before should hear once:

- The same link goes in every email and every form that takes a link. One link, so the ledger and the opens line up.
- An investor who asks for the PDF instead gets the PDF. Some firms will not open a gated link, and a lost conversation costs more than a lost page view. Keep the link for everyone else.

If a draft from `draft-my-outreach` is waiting for the link, drop it in and show the draft again.

### 5. Plant the next question

End with: "Ask me any time who has opened your deck." When they do:

- “Who opened it?” Use `pulse_list_links({"link_reference":"<selected link title>"})`. Its roster shows accepted identities and page-view counts, not page identities. Acceptance alone is not reading; preserve capped-roster and attribution notes.
- “What did Firm A look at?” Resolve the viewer from that link's roster, never a guessed email or firm identity. `pulse_get_contact_activity({"contact":"<returned viewer email>","focus":"documents"})` is room/contact-wide: retain only rows for the selected deck. Its top-page detail may describe another document; omit that detail entirely. Never name other documents or reveal their counts or pages, including to explain what you excluded. If link-specific attribution is unavailable, say so. Use deck analytics where needed.
- “Which pages get the most attention?” Use `pulse_get_document_analytics({"document_reference":"<deck id>","metric":"time_spent","breakdown":"by_page","window_days":30})`; `by_viewer` gives deck viewer totals. State the actual window and returned counting notes.

Report observed events, not investor intent or a guaranteed reason to chase. Show engagement beside the conversation's ledger if useful; it never establishes a send, reply or meeting. Missing tools or malformed/error results mean no verified update. Retry a transient read at most once with server guidance; do not blindly retry upload or link mutations. Stop on authorization or quota refusal without guessing the cause.

## Rules

- One link per raise unless the founder asks for separate links. Use returned confirmation flows for all gated actions; create a named-recipient link only when requested.
- Never send anything to anyone. Pulse links are not emailed by these tools.
- Never read or mention an investor's own Pulse account or activity outside this founder's links.
- The deck leaves this conversation for Pulse only through the founder's own app upload or their explicit instruction to upload this specific file. Never send it to the public investor matcher.
- No pricing, plans, trials or upgrades anywhere, including when connecting fails. If connecting is refused, say "Pulse could not connect from this assistant; the Pulse app will show why" and stop.
- Plain language. No em dashes.
