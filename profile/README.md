# Email8 - Email Platform

**Secure Post-Quantum-Safe Email Platform That Heals and Scales Itself.**

Email8 is the email vertical of the Sentin8 platform - a complete mail infrastructure: SMTP/IMAP/POP3/SIEVE protocols, message storage, access control, and webmail. Every new Sentin8:Auth user with an email address automatically gets a mail account with INBOX/Trash/Junk/Drafts/Sent/Archive mailboxes provisioned within seconds of sign-up.

## Services

| Service | Domain | What it does |
|---|---|---|
| **Email8:Gate** | Protocol Gateway | SMTP (ports 25/587/465), IMAP (143/993), POP3 (110/995), SIEVE (4190) - protocol termination, proxy to MTA/access |
| **Email8:MTA** | Mail Transfer Agent | Inbound/outbound mail routing, DNS lookups, delivery, bounce handling |
| **Email8:Store** | Message Storage | Email message storage, indexing, retrieval, search |
| **Email8:Access** | IMAP/POP3 Access Service | IMAP/POP3 protocol handlers, mailbox management, message access control |

## Flutter Apps

| Repository | Description |
|---|---|
| `email8-webmail` | Web-based email client |
| `email8-access` | API access layer for mobile/third-party clients |
| `email8-admin` | Vertical admin console (RFW) |

## Platform Dependencies

Email8 depends on these Sentin8 platform services for cross-cutting concerns:

| Platform Service | What Email8 gets from it | Dependency direction |
|---|---|---|
| **Sentin8:Auth** | User identity, sign-in, automatic mailbox provisioning | email8 → Auth (user provisioning via `sentin8:auth:user:created`) |
| **Sentin8:BFF** | Proxy, delegation gateway, client session custody | email8 ← BFF (user tokens proxied to services) |
| **Sentin8:Bus** | Durable event transport, producer outbox relay, consumer DLQ, SSE push | email8 → Bus (produce + consume domain events) |
| **Sentin8:Cron** | Recurring job scheduling (cleanup, retention policies) | email8 → Cron (schedule registration) |
| **Sentin8:Func** | Tenant-authored executable hooks (mail filters, processing rules) | email8 ← Func (proxy-back handlers for hooks) |
| **Sentin8:Storage** | Object/file storage (attachments, message bodies) | email8 → Storage (document PUT/GET) |
| **Sentin8:Stat** | Business metrics aggregation and telemetry | email8 → Stat (event ingestion) |
| **Sentin8:Sys** | Organization/environment lifecycle, service enablement | email8 → Sys (env-init events) |

## Key Invariants

- **Automatic provisioning** - every new Sentin8:Auth user with an email address automatically gets a mail account: the `sentin8:auth:user:created` event provisions a principal + INBOX/Trash/Junk/Drafts/Sent/Archive mailboxes within seconds of sign-up
- **Idempotent provisioning** - a redelivered event never duplicates mailboxes
- **Protocol compliance** - full SMTP, IMAP, POP3, and SIEVE protocol support with tenant isolation on every mailbox operation
- **No infrastructure operation** - the vertical never operates infrastructure; the platform group does
- **Composable enrollment** - any subset of services enrolls independently via `vertical enroll`

---

**Architecture & Design:** email8 and its underlying concepts were designed by [Hamid Jahanzad](https://www.linkedin.com/in/hamidjahanzad/).
