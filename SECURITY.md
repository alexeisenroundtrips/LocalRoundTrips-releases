<!--
  admin@roundtrips.io is the one address in this repository that a stranger is expected to find
  and use, so it has to stay a monitored mailbox rather than a person who may be on site. The same
  address is in legal/PrivacyPolicy.md §12 and docs/SECURITY-WHITEPAPER.md §9 — change all three
  in the same commit. This file is also published verbatim in the public releases repository so a
  reporter can reach it without access to the source; keep the two copies identical.
-->

# Security policy

RoundTrips moves a customer's project models between Autodesk Construction Cloud, their own
machine, and wherever they tell it to put them. It holds delegated tokens for their ACC, Microsoft
and Dropbox accounts. We would rather hear about a problem in it from you than from a customer.

## Reporting a vulnerability

Write to **admin@roundtrips.io**. If you prefer, open a
[private security advisory](https://docs.github.com/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability)
on this repository instead — both reach the same people.

Please do not open a public issue for something exploitable. Everything else — a crash, a wrong
result, a confusing message — is welcome as an ordinary issue.

Useful in a report, in rough order of usefulness:

- What an attacker gets, and what they need in order to get it.
- The version, from **Settings → About**, or the tag if you built it yourself.
- Steps that reproduce it, or the file, request or licence file that triggers it.
- Whether you have told anyone else.

You do not need a proof-of-concept exploit. A clear description of the flaw is enough to start.

## What we will do

| | |
|---|---|
| Acknowledge your report | Within **3 business days** |
| Tell you whether we can reproduce it, and our initial severity | Within **10 business days** |
| Ship a fix for a critical issue | Target **14 days** from confirmation |
| Ship a fix for a high-severity issue | Target **30 days** from confirmation |
| Tell you it is released, and credit you if you want it | On the day |

If we disagree that something is a vulnerability we will say so plainly and explain why, rather
than letting the thread go quiet. If a fix is going to take longer than the target above, we will
tell you that too, with a reason and a date.

## Disclosure

We ask for the usual coordinated disclosure: give us the window above before publishing. We will
not ask you to stay quiet indefinitely, we will not ask you to sign anything to report a bug, and
we will not take legal action against anyone who reports in good faith under this policy.

We publish fixes as a GitHub release on the public releases repository, with the security content
called out in the notes. Customers are emailed when an issue affects them.

## Scope

**In scope** — this repository, the released `RoundTrips` desktop application and its installer,
the licensing client, and the RoundTrips Live client and service.

**Out of scope** — Autodesk Construction Cloud, Microsoft 365, Dropbox and Keygen themselves;
report those to their own programmes. Also out of scope: findings that require an attacker to
already have administrator rights or physical access to the user's Windows profile, since at that
point they hold the credentials the application holds.

**Please do not** test against a customer's live ACC tenant, run automated scanners against
`api.keygen.sh` or our Live service, or do anything that would degrade a service for its users. If
you need an environment to test against, ask us and we will arrange one.

## Things we already know

Stated up front so nobody spends their time confirming them.

- **The audit log is tamper-evident, not tamper-proof.** Everything the hash chain needs is in the
  file, so whoever can edit the file can recompute it. The control that closes this is a collector
  shipping entries off the workstation, which
  [docs/LOGGING.md](docs/LOGGING.md) documents as the answer.
- **The assemblies decompile.** The build ships without an obfuscator or IL protector, by decision
  — the reasoning is in [docs/code-protection-options.md](docs/code-protection-options.md).
  Licence enforcement raises the effort required to bypass it; it does not prevent bypass, and we
  do not claim it does.
- **Local application data is not encrypted at rest.** Tokens go to Windows Credential Manager, but
  the task database, run logs and downloaded models are ordinary files protected by Windows file
  permissions. RoundTrips expects to run on a machine with full-disk encryption enabled.
- **The Client ID is public.** RoundTrips is an OAuth public client using PKCE and has no client
  secret. A committed Client ID is not a leaked credential.
