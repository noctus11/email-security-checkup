# Email Security Checkup

A small n8n workflow that checks whether a business domain is protected against email spoofing, and explains the result in plain English.

## What it does
1. A web form asks for a domain (for example `example.com`).
2. The workflow looks up the domain's public SPF and DMARC DNS records using Google's public DNS service.
3. An AI model (Gemini) reads the records and writes a short report with a grade from A to F, what the records mean, the risk, and recommended fixes.

## Why it matters
Small businesses are often impersonated in phishing and invoice scams because their domains have no DMARC record or a weak one. This tool turns raw DNS data into something a non-technical owner can understand.

## Built with
- n8n (self-hosted in Docker)
- Google Gemini
- Google public DNS (DNS over HTTPS)

## How to use
1. Run n8n and import `domain-email-checkup.json`.
2. Add your own Gemini API key as a credential and select it in the Gemini model node.
3. Run the workflow and submit a domain in the form.

## Limitations
- Checks only SPF and DMARC. DKIM is not checked because it needs a known selector.
- Uses public DNS information only. It does not scan or touch the target's servers.
- The report is AI-generated and should be reviewed by a person before being sent to a client.

## What I learned
- Wrote prompts that treat lookup data as untrusted input.
- Tested the output and corrected a weak recommendation about SPF hard fail when DMARC was already set to reject.
