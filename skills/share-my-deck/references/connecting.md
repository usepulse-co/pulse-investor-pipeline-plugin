# Connecting Pulse

What to tell a founder when the Pulse tools are not available in the conversation. Keep it to the line for their assistant.

| Assistant | What to say |
|---|---|
| Claude (web or desktop) | "When you want to share a deck, open Customize > Connectors and add the optional Pulse connector (https://api.usepulse.co/mcp). Sign in with Google. Then come back here." |
| Claude Code | "Run `claude mcp add --transport http pulse https://api.usepulse.co/mcp`, then `/mcp` to sign in." |
| ChatGPT | "Open Settings, Apps, find Pulse and connect it. Sign in with Google. Then come back here." |
| Cursor and other MCP clients | "Add `https://api.usepulse.co/mcp` as a remote MCP server and sign in when prompted." |

Matching uses the plugin’s anonymous connector. Add this separate authenticated connector only when deck sharing is requested. Pulse support is available at https://www.usepulse.co.

A founder with no Pulse account signs up on the way through: the sign-in page has a "No account?" path and returns them to the connection screen afterwards.

If connecting fails, say so in one line and stop. Do not explain what might be wrong on the Pulse side and do not mention plans or limits; the Pulse app shows the reason.

## The Pulse tools this skill uses

| Tool | Used for |
|---|---|
| `pulse_list_documents` | Is the deck already in Pulse? |
| `pulse_request_upload`, `pulse_complete_upload` | Upload from a client that can send file bytes |
| `pulse_confirm_action` | Complete or decline a returned pending confirmation |
| `pulse_create_link` | The one share link, no recipient, sends nothing |
| `pulse_list_links` | Accepted viewer roster and page-view counts |
| `pulse_get_contact_activity` | Room/contact-wide engagement; filter to the exact deck |
| `pulse_get_document_analytics` | Attention by page or over time |

Use the live discovered schemas if they differ from these examples. Missing tools are unavailable, not permission to invent substitutes. Link creation can return pending confirmation for sensitive documents, duplicate open item sets, recipients or watermark changes. Present the reason, wait for explicit approval, then call `pulse_confirm_action({"idempotency_token":"<returned token>","decision":"approve"})`. A decline uses `decision:"decline"`. Tokens expire; obtain a fresh pending action if expired, never claim success from an unconfirmed result.

## Upload protocol

Use only when the runtime has the exact file bytes and can issue outbound raw binary HTTP, and the founder authorized uploading this deck to Pulse.

1. `pulse_request_upload({"filename":"deck.pdf","size_bytes":12345})`, using exact file size.
2. PUT the raw bytes to returned `upload_url`, with `Content-Type: application/octet-stream` and the header named by `upload_header` whose value is `upload_ticket`. No JSON, base64 or multipart. This is Pulse-hosted ingress, not an S3 presigned URL.
3. After a verified successful PUT, `pulse_complete_upload({"upload_ticket":"<returned ticket>"})`. Retain the returned document ID and inspect processing state before reading pages or claiming readiness.

The upload ticket lasts 900 seconds and permits one ingress attempt. If transport outcome is ambiguous, do not blindly repeat the PUT or mint duplicate uploads; use returned recovery information or stop and report uncertainty. On refusal or processing failure, report the actual result and use the app handoff if appropriate.

## App handoff

Clients unable to send file bytes use `https://app.usepulse.co/drive` and its existing upload UI. Ask the founder to upload the PDF there and tell you when done, then resolve it in `pulse_list_documents`. Drive/Dropbox share URLs saved as web links do not import the PDF. Processing duration varies; do not promise a minute SLA.
