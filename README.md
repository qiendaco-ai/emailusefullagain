# emailusefullagain
AI Draft Autopilot is an open-source Thunderbird 115+ extension that creates rule-driven AI reply drafts without ever sending emails automatically and filter spams. 


# AI Draft Autopilot for Thunderbird

**Less spam. Less repetitive typing. More time for the emails that matter.**

AI Draft Autopilot is a Thunderbird 115+ MailExtension that screens incoming messages and prepares AI-assisted reply drafts using your own rules, prompts and preferred AI providers.

Our main goal is to reduce the burden of spam in the mailbox and save time by preparing answers to routine emails directly inside **Thunderbird**. You stay in your email workflow, with a draft ready to review instead of a blank reply window.


## Open-source release and future licensing

**The current V2.8.3 release is fully open source. Anyone can download the project and use it personally.**

**Any further version will be commercially licensed.** The commercial licensing terms will be published with those future releases. The licensing policy described here distinguishes the current open-source release from future commercial versions.

AI provider charges, where applicable, are separate from the extension. You configure your own provider or local model endpoint.

## Main features

### AI reply drafts with human control

- Prepare replies directly in Thunderbird using account-specific prompts and ordered rules.
- Preserve Thunderbird’s native quoted reply history, selected identity and signature.
- Review and edit each draft before deciding whether to send it.
- Generate or regenerate a draft manually from the message’s right-click menu.

### Spam and phishing screening

The extension works alongside your existing server and Thunderbird junk filters. Its spam/phishing gate runs on messages that remain in the Inbox.

Local pre-analysis checks signals including sender and Reply-To mismatches, misleading links, punycode domains, IP-address URLs, risky attachment extensions, and credential or payment-redirection language. An optional, independent AI classifier can assess unresolved messages.

Suspicious messages can be held for manual review. Local heuristics never delete, move or directly classify messages as spam. The project aims to reduce spam-related work; it does not promise to eliminate every unwanted message.

SPF, DKIM and DMARC evidence is used only from `Authentication-Results` headers matching receiving-server `authserv-id` values you explicitly trust. These headers are ignored for trust decisions until trusted servers are configured.

### Native Thunderbird control tags

| Tag | Behaviour |
| --- | --- |
| **AI: Trusted** | Explicitly bypasses this message’s spam/phishing gate, then applies normal drafting rules. |
| **AI: Never Draft** | Prevents automatic drafting for the message. |
| **AI: Always Draft** | Overrides ordinary drafting skips and ignore rules, while retaining the spam/phishing gate. |
| **AI: Review** | Holds the message from automatic drafting for manual review. |
| **AI: Stale Draft** | Identifies an older tracked AI draft after the conversation has moved on. |

### Sender reputation

Trust specific sender addresses or domains explicitly. Optional address-book matching and your actual sent replies can also establish trust.

Learned trust applies to the **exact sender address**, with a default threshold of three real replies. Replying to one address does not automatically trust every address on its domain. Reply history is stored locally, bounded and independently clearable.

Reputation-based trust does not erase severe phishing indicators. An explicit **AI: Trusted** message tag remains a deliberate override of the gate.

### Flexible account rules

Match sender, domain, subject, body, recipients, Reply-To, List-ID, tags, attachments, mailing-list status, automatic/bulk status or junk status.

Rules can draft or ignore, append or replace prompts, select identity/reply/signature behaviour, and override model, temperature and token settings.

### Configurable conversation context

Choose what the drafting AI receives:

- **Full thread:** all available extracted conversation text.
- **Recent exchanges:** the newest message plus a configurable number of quoted exchanges.
- **Latest message only:** the newest unquoted message text.

The body-character limit still applies. These settings control AI context and API usage; they do not remove Thunderbird’s native quoted history from the saved draft.

### Reliable processing

A persistent queue coordinates processing, with rapid-follow-up coalescing, transient-error retries, separate spam/draft concurrency limits, circuit breakers and hourly/daily API ceilings.

Startup and historical IMAP-sync protection help avoid unintended historical processing. Cancellation and supersession checks help prevent obsolete work from saving drafts. Diagnostics are bounded, with manual inspection and flush controls.

### Stale-draft tracking

When a newer message arrives, you regenerate a draft, or Thunderbird reports a real reply was sent, older tracked AI drafts can be marked **AI: Stale Draft**. They are tagged rather than automatically deleted. Real replies also update reputation and cancel obsolete queued thread work.

## Supported AI providers

Drafting supports:

- OmniRouter / OpenAI-compatible Chat Completions
- OpenAI-compatible Responses API
- Anthropic
- Gemini
- Ollama
- Generic REST

The optional spam classifier supports these provider families plus **native Jev / TypeSafe SystemOne**. Spam classification and drafting can use different providers.

Messages selected for AI processing are sent to the corresponding configured provider. Local security analysis and reputation storage do not mean all AI processing happens on your device.

## How it works

1. Server and Thunderbird filters process the message.
2. The persistent queue schedules eligible work.
3. The Inbox-only spam gate, local analysis and reputation checks run.
4. The optional spam AI evaluates the message when needed.
5. Account rules select drafting behaviour and prompts.
6. The drafting AI prepares a reply.
7. Thunderbird saves a native draft for your review.

## Installation

1. Download `ai-draft-autopilot-v2.8.3.xpi` from the project’s published release files.
2. In Thunderbird, open **Add-ons and Themes**.
3. Choose the **gear icon → Install Add-on From File…**.
4. Select the XPI and confirm installation.
5. Configure your AI provider, account prompts, drafting rules and optional spam/phishing settings.

**Requirement:** Thunderbird 115 or newer. This is a Thunderbird MailExtension, not a Firefox browser extension.

V2.8.3 uses the same extension ID as earlier releases, allowing in-place upgrades from V2.8, V2.8.1 and V2.8.2 while migrating existing settings and logs.

## Current release: V2.8.3

The third audit hotfix preserves V2.8.2 behaviour and addresses queue state, cross-tab settings writes, tag-update races, classifier parsing, native signature policy, request limits and templating, attached-message isolation, compose retries, error-file downloads, secret redaction and imported-rule safety.
