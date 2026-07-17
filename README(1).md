# Phishing Email Detection & Awareness System

**Task 02 — Cyber Security Track**
**Author:** Brian Amani Mlenge
**Repository:** `FUTURE_CS_02`

A header and content analysis of real-world phishing emails, built to demonstrate practical detection techniques and produce awareness material that a non-technical employee could actually use.

---

## 📌 Overview

This project analyzes six real phishing emails pulled from a public honeypot dataset, breaking down the technical and behavioral indicators that expose each one. A seventh, clearly labeled reference sample is included to show what a legitimate email looks like by comparison — the honeypot corpus, by definition, contains only malicious captures and can't supply a genuine "safe" baseline on its own.

The full write-up lives in [`Phishing_Detection_Report.docx`](./Phishing_Detection_Report.docx).

## 🎯 Objective

- Extract and inspect email headers for authentication and routing red flags (SPF, Return-Path, Reply-To, Received chains).
- Identify identity-spoofing and social-engineering patterns in email content.
- Classify each sample by risk level with supporting evidence.
- Translate the technical findings into prevention guidance usable by non-technical staff.

## 🗂️ Methodology

Samples were sourced from **[Phishing Pot](https://github.com/rf-peixoto/phishing_pot)** (`rf-peixoto/phishing_pot`), a public GitHub repository that collects real phishing emails caught by honeypot mailboxes, with recipient addresses anonymized before publishing.

Six emails were selected to represent a range of attack styles — brand impersonation, account-compromise scams, and bulk spam. Headers were pulled directly from the raw `.eml` files and examined for sender authentication results, domain consistency, and routing information. Body content was reviewed separately for social-engineering language and link destinations. Each sample is paired with a header-analysis figure generated directly from the extracted header data.

**Tools used:** raw header inspection, the Phishing Pot dataset, and a text editor for documentation.

## ✅ Indicator Checklist

| Category | Indicator | Why it matters |
|---|---|---|
| **Sender authenticity** | SPF result in `Authentication-Results` | A pass means the sending server was authorized for that domain — it does **not** mean the message is safe, only that it wasn't spoofed at the domain level |
| **Sender authenticity** | `Return-Path` / `Reply-To` domain vs. `From` domain | If either points somewhere other than the `From` domain, replies or bounces are being routed away from the claimed origin |
| **Identity spoofing** | Display name vs. actual address | Display name says one thing ("Trust Wallet," "McAfee," "Facebook") while the address belongs to an unrelated domain |
| **Identity spoofing** | Lookalike character substitution | A capital "I" swapped for a lowercase "l" is nearly invisible in most fonts but changes the word entirely ("WaIIet," "Iog in") |
| **Infrastructure** | Domain–brand relationship | Sending domain has no business relationship to the claimed brand, or looks compromised/repurposed |
| **Infrastructure** | `Received:` chain / sending IP | Long or unusual routing chains, or a sending IP that doesn't match the claimed origin |
| **Content & behavior** | Urgency or fear-based language | Phrases like "your account will be suspended" or "verify immediately" are designed to trigger action before scrutiny |
| **Content & behavior** | Disproportionate offers | Rewards or returns that are unrealistically generous for an unsolicited email |
| **Content & behavior** | Link text vs. destination | Visible link text doesn't match where the link actually goes |

## 🔍 Samples Analyzed

| # | Claimed Sender | Subject Line | Actual Domain | SPF | Risk Level | Key Indicator |
|---|---|---|---|---|---|---|
| 1 | Facebook | *"Someone tried to Iog in To Your Account…"* | Unrelated domain | None | High — Phishing | Lookalike character substitution ("Iog," "ID") + failed SPF |
| 2 | Trust Wallet | *"Your Account KYC is unverified."* | Unrelated (real) ISP domain | Pass | High — Phishing | Passing SPF but zero relationship between sender and claimed brand |
| 3 | McAfee | *"Ihr McAfee-Abonnement läuft ab…"* | Unrelated business domain | Pass | High — Phishing | Three-domain layering (sender / brand / Reply-To) via bulk relay |
| 4 | Real employee (media org) | *"Successful Transfer"* | Legitimate, own domain | Pass | High — Phishing (compromised account) | Headers check out completely; account itself was compromised |
| 5 | Casino brand | *"Claim Your Free 400 FREE SPINS…"* | Bulk-marketing domain | Pass | Medium — Suspicious | No credential harvesting, but unsolicited too-good-to-be-true offer |
| 6 | Stellar Development Foundation | *"Stake your XLM for 25% yields"* | Unrelated (compromised) domain | None | High — Phishing | Guaranteed fixed-yield claim + failed SPF |
| 7 | *(reference)* legitimate service | *"Your monthly account statement is ready"* | Matches claimed domain | Pass | Safe | From / Return-Path / Reply-To all aligned, no urgency language |

**Notable case — Sample 4:** every header check passes cleanly (SPF pass, legitimate domain), yet the message is still phishing because the sending account itself was compromised. This is the report's central lesson: header analysis alone isn't sufficient — a message can pass every authentication check and still be malicious.

## 🛡️ Prevention & Awareness Guidelines

- Check the actual sending address, not just the display name.
- A passing SPF/DKIM check is not proof of legitimacy — only that the message wasn't spoofed at the domain level.
- Slow down on urgency; legitimate organizations rarely demand immediate action by email alone.
- Hover before clicking — compare visible link text to the actual destination.
- Be skeptical of guaranteed returns or free money.
- Report suspicious emails to IT/security instead of just deleting them.

## 📄 Repository Contents

```
FUTURE_CS_02/
├── Phishing_Detection_Report.docx   # Full report with header-analysis figures
└── README.md                        # This file
```

## 📚 References

- Phishing Pot dataset: https://github.com/rf-peixoto/phishing_pot

---

*Prepared as part of the Cyber Security track, Task 02 — Phishing Email Detection & Awareness System, July 2026.*
