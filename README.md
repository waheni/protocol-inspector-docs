# Protocol Inspector — End User Guide

## 1) What this plugin does

Protocol Inspector is a tool window plugin for IntelliJ-based IDEs that helps you inspect SIP messages and SDP content.

You can:
- Paste one or many SIP messages from your clipboard.
- Review each message in a structured list.
- Inspect raw SIP text and parsed details.
- See protocol findings (errors/warnings/info) with remediation guidance.
- Export a Markdown report to clipboard.
- Use the Help tab to report bugs or request features.

---

## 2) Prerequisites

- IntelliJ Platform IDE with the plugin installed.
- SIP trace text available in your clipboard.

## 2.1 IDE compatibility

Protocol Inspector is built on the IntelliJ Platform and is not limited to IntelliJ IDEA only.

It can be installed in other JetBrains IDEs as well, including:
- GoLand
- RustRover
- Android Studio

Compatibility depends on each IDE's underlying IntelliJ Platform build version and Marketplace availability.

---

## 3) Open the plugin

1. Open your IDE.
2. Go to **View → Tool Windows → Protocol Inspector**.
3. The Protocol Inspector tool window appears.

---

## 4) UI overview

The tool window has 3 main parts:

## 4.1 Toolbar (top)
- **Paste from Clipboard**: Parses SIP text from your clipboard.
- **Export Report**: Generates a Markdown report and copies it to clipboard.
- **Clear**: Clears loaded messages and current analysis.

## 4.2 Message list (left)
Each row shows:
- SIP start line
- Call-ID
- CSeq

Click a message to inspect it.

## 4.3 Right tabs
- **Raw**: Original SIP message exactly as parsed.
- **Details**: Extracted fields (From/To/Via/Contact, Content-Type, SDP details).
- **Findings**: Rule-based issues and guidance.
- **Help**: Feedback buttons (Report Issue / Request Feature / Rate Plugin).

---

## 5) Quick start workflow

1. Copy SIP messages to your clipboard.
2. In Protocol Inspector, click **Paste from Clipboard**.
3. Wait for parsing to complete.
4. Select a message in the left list.
5. Review:
   - **Raw** tab for exact content.
   - **Details** tab for parsed fields.
   - **Findings** tab for issues.
6. Click **Export Report** to copy a full Markdown report.
7. Paste the report into a ticket, doc, or chat.

---

## 6) Findings explained

Findings are shown with severity icons:
- ❌ Error
- ⚠️ Warning
- ℹ️ Info

The top of the Findings tab includes a summary line with total counts.

Current rule checks include:

| Code | Severity | What it means |
|---|---|---|
| MISSING_CALL_ID | Error | `Call-ID` header missing |
| MISSING_CSEQ | Error | `CSeq` header missing |
| MISSING_FROM | Error | `From` header missing |
| MISSING_TO | Error | `To` header missing |
| MISSING_VIA | Error | `Via` header missing |
| VIA_NO_BRANCH | Error | `Via` exists but has no `branch=` parameter |
| CONTENT_LENGTH_MISMATCH | Warning | `Content-Length` differs from actual body size |
| CONTACT_PRIVATE_IP | Warning | `Contact` contains private IP |
| SDP_PAYLOAD_NO_RTPMAP | Warning | Dynamic SDP payload type has no `a=rtpmap` |
| SDP_NO_AUDIO | Error | SDP exists but no `m=audio` line |
| SDP_AUDIO_PORT_ZERO | Error | SDP `m=audio` port is `0` |
| SDP_NO_CONNECTION | Warning | SDP missing `c=` connection line |
| SDP_CONNECTION_ZERO | Warning | SDP connection IP is `0.0.0.0` |

---

## 7) Expected SIP input format

- Input should contain valid SIP start lines, for example:
  - `INVITE sip:bob@example.com SIP/2.0`
  - `SIP/2.0 200 OK`
- Multiple SIP messages can be pasted together.
- SDP is detected from:
  - `Content-Type: application/sdp`, or
  - body starting with `v=0`.

---

## 8) Exporting reports

When you click **Export Report**:
1. The plugin generates a Markdown report from loaded messages.
2. The report is copied to your system clipboard.
3. Paste it where needed.

If no messages are loaded, export is blocked.

---

## 9) Help tab (feedback)

The **Help** tab provides:
- **Report Issue**: opens the issue form in your browser.
- **Request Feature**: opens feature request form.
- **Rate Plugin**: opens marketplace page .


---

## 10) Large traces and performance

- Parsing and analysis run in background tasks.
- UI remains responsive while processing.
- For large inputs, selecting messages manually is recommended.

---

## 11) Troubleshooting

## 11.1 “No text found in clipboard”
Your clipboard does not contain plain text.
- Copy SIP text again.
- Retry **Paste from Clipboard**.

## 11.2 “Processing already in progress”
A background task is still running.
- Wait for completion, then retry.

## 11.3 “No messages loaded” on export
- Paste SIP text first.
- Confirm message rows appear on the left.

## 11.4 Buttons disabled
Buttons are disabled while parsing/report generation is active.
- Wait until the task ends.

## 11.5 Issue/Feature buttons do nothing or show config dialog
Feedback URL may be blank or placeholder.
- Contact plugin maintainer to configure feedback links.

---

## 12) Best practices

- Paste complete SIP transactions when possible.
- Preserve original line breaks and headers.
- Verify `Content-Length` and SDP fields during debugging.
- Use exported Markdown reports for incident tickets and peer review.

---

## 13) Frequently asked questions

## Q: Can I paste multiple SIP dialogs at once?
Yes. The parser splits messages by SIP start lines.

## Q: Does it modify my SIP data?
No. It parses and analyzes read-only content.

## Q: Can I save report output to a file directly?
The plugin currently copies report output to clipboard.

## Q: Why are some findings warnings and not errors?
Warnings indicate suspicious or risky protocol conditions, not always RFC-invalid payloads.

---

## 14) Summary

Protocol Inspector provides fast SIP/SDP inspection with actionable findings and one-click Markdown export. Use it as a daily troubleshooting companion for signaling and media negotiation issues.
