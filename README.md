# Protocol Inspector — End User Guide

## 1) What this plugin does

Protocol Inspector is a tool window plugin for IntelliJ-based IDEs that helps you inspect SIP messages and SDP content.

You can:
- Paste one or many SIP messages from your clipboard.
- Analyze the currently open `.log` file in the editor.
- Open a bundled sample SIP log for a quick demo.
- Review each message in sessions grouped by `Call-ID`.
- Use header-name search to quickly narrow messages (for example `Via`, `Contact`, `Call-ID`).
- Inspect raw SIP text and parsed details.
- See protocol findings (errors/warnings/info) with remediation guidance.
- Export a Markdown report to clipboard.
- Use the Help tab to report bugs, request features, and leave a Marketplace review.

---

## 2) Prerequisites

- IntelliJ Platform IDE with the plugin installed.
- SIP trace text available in your clipboard, **or** a `.log` file open in the editor.

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
- **Analyze Current File**: Reads the currently active editor file (must be `.log`) and extracts SIP messages from mixed log text.
- **Open Sample SIP Log**: Loads a bundled demo SIP trace so you can try the plugin immediately.
- **Export Report**: Generates a Markdown report and copies it to clipboard.
- **Clear**: Clears loaded messages and current analysis.

## 4.2 Message list (left)
Left panel includes:
- **Sessions** list (grouped by Call-ID, including `(none)` when missing)
- **Messages** list for the selected session
- **Search** field to filter messages by header name (for example `Via`, `Contact`, `Call-ID`)

Click a message to inspect it.

## 4.3 Right tabs
- **Raw**: Original SIP message exactly as parsed.
- **Details**: Extracted fields (From/To/Via/Contact, Content-Type, SDP details).
- **Findings**: Rule-based issues and guidance.
- **Help**: Feedback buttons (Report Issue / Request Feature / Rate Plugin).

---

## 5) Quick start workflow

Choose one input method:
1. **Paste from Clipboard** for raw SIP text.
2. **Analyze Current File** for an open `.log` file.
3. **Open Sample SIP Log** for the built-in demo.

Then:
4. Wait for parsing to complete.
5. Select a **Call-ID session**, then a message.
6. Optionally use **Search** to filter the current session by header name.
  - Example queries: `via`, `contact`, `call-id`, `cseq`.
7. Review:
   - **Raw** tab for exact content.
   - **Details** tab for parsed fields.
   - **Findings** tab for issues.
8. Click **Export Report** to copy a full Markdown report.
9. Paste the report into a ticket, doc, or chat.

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

## 7) Input formats and SIP detection

- Input should contain valid SIP start lines, for example:
  - `INVITE sip:bob@example.com SIP/2.0`
  - `SIP/2.0 200 OK`
- Multiple SIP messages can be pasted or loaded from `.log` files.
- For `.log` files, SIP can be extracted from mixed text that includes timestamps/log prefixes.
- SDP is detected from:
  - `Content-Type: application/sdp`, or
  - body starting with `v=0`.

### 7.1 `.log` file support details

- Only the **currently active editor file** is analyzed.
- The file must have a `.log` extension.
- SIP start lines are detected for requests and responses (for example `INVITE`, `REGISTER`, `OPTIONS`, `SIP/2.0 100 Trying`, `SIP/2.0 200 OK`).
- If no SIP messages are detected, the plugin shows: **“No SIP messages found in the log file.”**

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
- **Report Issue**: opens the bug issue template in the documentation repository.
- **Request Feature**: opens the feature request template in the documentation repository.
- **Rate Plugin**: opens the JetBrains Marketplace reviews page.

### 9.1 How to share effective feedback

To help us ship fixes faster and prioritize the right features:
- **For bugs**: include a short SIP snippet (sanitized), expected behavior, actual behavior, and IDE version.
- **For feature requests**: describe your workflow, why the feature matters, and what result you expect.
- **For reviews**: share what helped most (for example `Call-ID` grouping, search, findings quality, report export).

Even a short review and a small reproducible bug report are very helpful.

Current links:
- Issues: https://github.com/waheni/protocol-inspector-docs/issues/new?template=bug_report.md
- Feature requests: https://github.com/waheni/protocol-inspector-docs/issues/new?template=feature_request.md
- Reviews: https://plugins.jetbrains.com/plugin/29805-protocol-inspector/reviews

If a URL is not configured, the plugin shows:
**“Please configure feedback URL”**.

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
- Load SIP data first (paste text, analyze current `.log` file, or open sample).
- Confirm message rows appear on the left.

## 11.4 Buttons disabled
Buttons are disabled while parsing/report generation is active.
- Wait until the task ends.

## 11.5 “Open a .log file to analyze SIP messages.”
- Open a file with `.log` extension in the editor.
- Make sure that file tab is active.

## 11.6 “No SIP messages found in the log file.”
- Verify the log contains SIP start lines (`INVITE`, `BYE`, `SIP/2.0 ...`, etc.).
- Try **Open Sample SIP Log** to confirm plugin behavior.

## 11.7 “Sample SIP log could not be loaded.”
- This indicates a bundled resource loading problem.
- Restart IDE and retry.
- If issue persists, report it via the **Help** tab.

## 11.8 Issue/Feature buttons do nothing or show config dialog
Feedback URL may be blank or placeholder.
- Contact plugin maintainer to configure feedback links.

---

## 12) Best practices

- Paste complete SIP transactions when possible.
- For first-time usage, try **Open Sample SIP Log**.
- For production traces, keep logs in `.log` files and use **Analyze Current File**.
- Start with **Sessions** (Call-ID groups), then use **Search** to isolate the headers you care about.
- Preserve original line breaks and headers.
- Verify `Content-Length` and SDP fields during debugging.
- Use exported Markdown reports for incident tickets and peer review.
- If something is missing or confusing, please use **Request Feature** or **Report Issue** from the Help tab.
- If the plugin helps your workflow, please leave a **Rate Plugin** review in Marketplace.

---

## 13) Frequently asked questions

## Q: Can I paste multiple SIP dialogs at once?
Yes. The parser splits messages by SIP start lines.

## Q: Can I analyze a `.log` file directly from the editor?
Yes. Open the `.log` file, then click **Analyze Current File**.

## Q: Is there a built-in demo?
Yes. Click **Open Sample SIP Log**.

## Q: How do I find messages faster in big traces?
Select the relevant `Call-ID` session first, then use **Search** with header names such as `via`, `contact`, or `cseq`.

## Q: Does it modify my SIP data?
No. It parses and analyzes read-only content.

## Q: Can I save report output to a file directly?
The plugin currently copies report output to clipboard.

## Q: Why are some findings warnings and not errors?
Warnings indicate suspicious or risky protocol conditions, not always RFC-invalid payloads.

---

## 14) Summary

Protocol Inspector provides fast SIP/SDP inspection with actionable findings and one-click Markdown export. You can paste logs, analyze the current `.log` file, or start instantly with the bundled sample SIP demo.
