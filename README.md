# Chat Control Resistance Kit

A practical, technical toolkit for reducing your exposure to EU Chat Control-style scanning mandates and mass communications surveillance.

On 9 July 2026, the European Parliament revived the "temporary" Chat Control 1.0 derogation through a procedural maneuver, extending voluntary/mandated scanning of private communications for another cycle. End-to-end encrypted services received an explicit carve-out this round — but that carve-out is a policy choice, not an architectural guarantee, and it can be narrowed or removed in the next legislative cycle. The permanent Child Sexual Abuse Regulation (CSAR), which would mandate client-side scanning (CSS) on E2EE services regardless of encryption, remains in play and will return. Relying on a single vendor's E2EE promise is a bet on that vendor's jurisdiction, business model, and willingness to fight a legal order — not a technical guarantee against scanning. The tools below are chosen because their architecture, not their current policy or goodwill, makes mass scanning and single-point compulsion expensive or impossible. This is not about hiding wrongdoing — it's about making population-scale surveillance infrastructure structurally unworkable.

## Table of Contents

- [Messaging](#messaging)
- [Self-Hosted & Federated](#self-hosted--federated)
- [Email & Communication](#email--communication)
- [VPN & Transport](#vpn--transport)
- [Storage & Files](#storage--files)
- [Operating Systems & Browsers](#operating-systems--browsers)
- [Additional Hardening](#additional-hardening)
- [Quick Migration Paths](#quick-migration-paths)
- [Opsec Basics](#opsec-basics)
- [Contributing & Updates](#contributing--updates)

---

## Messaging

The core defense layer. Priority goes to architectures where there is no central server that can be legally compelled to scan, log, or backdoor traffic for everyone at once.

### Comparison Table

| Tool | Architecture | Server Required | Metadata Exposure | Client-Side Scanning Risk |
|---|---|---|---|---|
| SimpleX Chat | P2P relay, no user IDs | Yes (relays, swappable/self-hostable) | Near-zero — no persistent identifiers | Very low — no central account to target |
| Briar | P2P (Tor/Bluetooth/Wi-Fi Direct) | No (or Tor as transport) | Minimal — no server at all in local mode | Effectively none |
| Jami | Distributed hash table (DHT) | No central server | Low — P2P signaling | Very low |
| Session | Onion-routed over decentralized node network | Yes (Session Network nodes) | Low-medium — no phone number required | Low |
| Matrix + Element | Federated | Yes (many, self-hostable) | Medium — depends on homeserver, metadata visible to your homeserver | Medium — depends on server operator |
| Delta Chat | Uses existing email (IMAP/SMTP) + Autocrypt | Yes (any email server) | Medium — email metadata applies | Low |
| Threema | Centralized, privacy-focused | Yes (Swiss company-run) | Low, but centralized | Medium — single company, single jurisdiction |
| Signal | Centralized | Yes (Signal Foundation) | Low content, but centralized account structure | Medium-high — single org, single legal target |

### SimpleX Chat
The only major messenger with no user identifiers at all — not even a random UUID tied to your identity.

- **Why it helps against Chat Control / mass scanning:** SimpleX has no persistent user or device identifiers. Each contact connection uses independent, unlinkable queue pairs, so there is no account graph for an authority to subpoena or scan across. There's no central directory of users, so a scanning mandate has no index to iterate over. Relay servers only ever see encrypted envelopes for a specific queue, not a user's full contact graph.
- **Key practical notes:** Available on iOS, Android, desktop (Linux/Mac/Windows). Setup is as easy as any modern messenger — scan a QR/link to connect. You can self-host your own SMP (SimpleX Messaging Protocol) relays. No phone number or email needed to sign up.
- **Official links:** https://simplex.chat
- **Honest warning:** Younger project than Signal/Matrix, smaller audit history, and the group chat / large community experience is less polished. No usernames means recovery of a lost device is harder — there's no "account" to restore into.

<!-- Add new messaging tool here -->

### Briar
Peer-to-peer messenger designed to work over Tor, Bluetooth, or Wi-Fi Direct with no central server dependency at all.

- **Why it helps against Chat Control / mass scanning:** Briar syncs messages directly between devices, optionally over Tor hidden services. There is no central company, no central server, and often no internet infrastructure in the path at all (Bluetooth/Wi-Fi mesh mode). You cannot compel a company to scan traffic that never touches their infrastructure. This is the strongest architecture in this list against any form of centralized compelled scanning.
- **Key practical notes:** Android only (no iOS, due to Apple's background process restrictions incompatible with Briar's P2P sync model). Works offline between nearby devices. Designed originally for activists/journalists in high-risk, low-connectivity environments.
- **Official links:** https://briarproject.org
- **Honest warning:** No iOS client — this is a hard platform limitation, not a roadmap gap. Sync can be slow/battery-hungry in Tor mode. Small user base means fewer contacts will already be on it.

<!-- Add new messaging tool here -->

### Jami
Fully distributed, serverless communication platform (text, voice, video) built on a DHT (distributed hash table), originally a GNU project.

- **Why it helps against Chat Control / mass scanning:** No central server stores accounts or routes messages — peer discovery happens via DHT, similar in spirit to BitTorrent. There is no single company or server operator to serve a scanning order to, because there's no central point in the data path for one-to-one and small group communication.
- **Key practical notes:** Cross-platform (Linux, Windows, Mac, Android, iOS). Can also do SIP-style calling. Self-hosting an optional "account manager" (JAMS) is possible for organizations wanting managed accounts.
- **Official links:** https://jami.net
- **Honest warning:** UX is rougher than mainstream apps, DHT-based discovery can be slower to establish connections, and the project has smaller-scale independent security audits than Signal.

<!-- Add new messaging tool here -->

### Session
Messenger built on the Signal protocol fork but routed through a decentralized incentivized node network (Session Network, formerly Oxen), with no phone number requirement.

- **Why it helps against Chat Control / mass scanning:** Messages are onion-routed through a permissionless network of independently operated nodes rather than a single company's servers. No phone number or email is required to create an account, breaking the identity-to-account link that makes mass correlation easy. No single node operator has the full picture of sender/recipient/content.
- **Key practical notes:** Android, iOS, desktop. Onion routing adds latency vs. direct-connection apps. Voice/video calling exists but is less mature.
- **Official links:** https://getsession.org
- **Honest warning:** Session's security model has faced public scrutiny over cryptographic decisions distinct from mainline Signal protocol (e.g. session ID key handling); read the audits yourself before trusting it for high-stakes threat models. It is not officially affiliated with or endorsed by Signal.

<!-- Add new messaging tool here -->

### Matrix + Element
Open, federated communication protocol — think "email for chat" — with Element as the flagship client.

- **Why it helps against Chat Control / mass scanning:** Federation means there is no single homeserver an authority can compel to scan the entire network — a mandate against one homeserver operator affects only that server's users, not the protocol. **Self-hosting your own homeserver is the single highest-leverage move in this entire list**: you become your own data controller, subject to your own jurisdiction's rules and your own server's logs, not a mega-platform's. Combined with E2EE (on by default in private rooms), your homeserver operator sees only encrypted payloads for cross-server traffic.
- **Key practical notes:** Self-host with Synapse, Dendrite, or Conduit (Rust, lightweight, good for small/personal servers). Element is available on all major platforms. Bridges exist for WhatsApp/Telegram/Signal/etc. if you need interop during migration.
- **Official links:** https://matrix.org, https://element.io
- **Honest warning:** Federation means metadata (who's talking to whom, room membership) is visible to both parties' homeservers even with E2EE on content — federation is not the same as zero-metadata. Self-hosting requires real sysadmin competence and ongoing maintenance (updates, backups, abuse handling) to not become a liability.

<!-- Add new messaging tool here -->

### Delta Chat
A modern chat UI running entirely over standard email (IMAP/SMTP) with automatic E2EE via Autocrypt/OpenPGP.

- **Why it helps against Chat Control / mass scanning:** There is no Delta Chat server at all — you bring your own email account (including a self-hosted one), so there's no central company to compel. Because it rides on the existing, massively decentralized email ecosystem, there's no single "Delta Chat Inc." to serve an order against.
- **Key practical notes:** Works with any IMAP/SMTP provider, including self-hosted mail servers or Proton Mail (via Bridge). Cross-platform. Good option for users who already trust their email provider/self-hosted mail setup.
- **Official links:** https://delta.chat
- **Honest warning:** You inherit your email provider's metadata exposure (sender/recipient/timing headers are visible to mail servers in the path) unless you fully self-host and control both ends. Group chat UX is less smooth than purpose-built messengers.

<!-- Add new messaging tool here -->

### Threema
Centralized but privacy-first Swiss messenger, no phone number required, paid app.

- **Why it helps against Chat Control / mass scanning:** Swiss jurisdiction (outside EU/CSAR reach, subject to Swiss data protection law) and no phone number or email requirement means the account itself carries minimal linkable identity. Open-source clients allow independent verification of claims.
- **Key practical notes:** One-time paid app (no ad/data-monetization model). iOS, Android, desktop, and a work/enterprise (Threema Work) tier. Threema Gateway and on-prem (Threema OnPrem) options exist for organizations wanting full control.
- **Official links:** https://threema.ch
- **Honest warning:** Still centrally operated by a single Swiss company — Switzerland is not in the EU but is not immune to international legal pressure or mutual legal assistance treaties. Smaller user base than mainstream apps hampers network effects.

<!-- Add new messaging tool here -->

### Signal (with honest caveats)
The mainstream gold-standard for E2EE protocol design — but architecturally centralized.

- **Why it helps against Chat Control / mass scanning:** The Signal Protocol (double ratchet) is the best-audited E2EE messaging cryptography in mass use, and Signal has a public track record of shutting down operations in jurisdictions (e.g. leaving the UK market threat, past resistance to backdoor requests) rather than complying with backdoor mandates. Sealed sender reduces some metadata exposure.
- **Key practical notes:** iOS, Android, desktop. Extremely polished UX, huge existing user base makes migration friction low — the best "first step" app for non-technical contacts.
- **Official links:** https://signal.org
- **Honest warning:** Single centralized organization, single US jurisdiction, single legal entity to target with a court order or client-side scanning mandate. Requires a phone number to register (a real metadata/identity linkage, even if not visible to contacts). If CSAR-style mandates were ever imposed on E2EE apps operating in the EU and Signal chose compliance over withdrawal, every user would be affected simultaneously — that centralization is the core weakness this toolkit is designed around. Excellent app, wrong architecture for a scan-resistant future.

<!-- Add new messaging tool here -->

---

## Self-Hosted & Federated

Owning your own infrastructure is the highest-leverage move against any mass-mandate scanning regime — a court order against a platform with a billion users is a different animal than a court order against your personal VPS.

### Matrix Homeserver (Synapse / Dendrite / Conduit)
Run your own federation node for Matrix/Element.

- **Why it helps against Chat Control / mass scanning:** You become the data controller. Any legal demand must target you specifically, in your jurisdiction, for your server's logs — not a blanket mandate executed across a billion-user platform.
- **Key practical notes:** Conduit (Rust) is the lightest-weight option for small/personal servers; Synapse (Python) is the reference implementation with the most features but heavier resource use; Dendrite (Go) sits in between. Needs a domain, TLS cert, and basic Linux server administration.
- **Official links:** https://matrix.org/docs/older/servers/, https://conduit.rs, https://github.com/matrix-org/dendrite
- **Honest warning:** You are now responsible for uptime, backups, security patching, and potentially abuse/legal requests aimed at your server directly. Self-hosting badly (unpatched, unmonitored) is worse than using a well-run managed provider.

<!-- Add new self-hosted tool here -->

### XMPP (Prosody / ejabberd) + OMEMO
The original federated, decentralized chat protocol, still very much alive, with OMEMO for E2EE.

- **Why it helps against Chat Control / mass scanning:** Same federation logic as Matrix — no single operator, easy to self-host on minimal hardware. OMEMO provides Signal-protocol-grade E2EE on top.
- **Key practical notes:** Prosody is lightweight and beginner-friendly for self-hosting; ejabberd scales further for larger deployments. Clients: Conversations (Android), Monal (iOS/Mac), Gajim (desktop).
- **Official links:** https://prosody.im, https://xmpp.org
- **Honest warning:** Fragmented client ecosystem — feature support (OMEMO, file transfer, calls) varies significantly by client, which causes confusing inconsistent experiences for non-technical users.

<!-- Add new self-hosted tool here -->

### Nextcloud Talk
Self-hosted chat/video/files as part of a Nextcloud instance you already control.

- **Why it helps against Chat Control / mass scanning:** Bundles messaging with your own file storage and calendar under infrastructure you fully own — no third party in the data path at all if self-hosted on your own server.
- **Key practical notes:** Good option if you already run Nextcloud for file sync; adds chat/video without a new stack. E2EE for Talk is more limited than dedicated messengers — evaluate the current encryption status before relying on it for high-sensitivity comms.
- **Official links:** https://nextcloud.com/talk/
- **Honest warning:** Not primarily designed as a hardened E2EE messenger — treat it as a convenience layer for a self-hosted team/family, not a Signal/SimpleX replacement for sensitive conversations.

<!-- Add new self-hosted tool here -->

### Tailscale alternatives for self-hosted mesh (Headscale, Netmaker)
Self-hosted control plane for WireGuard-based mesh networks, replacing Tailscale's centralized coordination server.

- **Why it helps against Chat Control / mass scanning:** Tailscale itself is WireGuard under the hood (strong transport encryption) but relies on Tailscale's own coordination servers to broker connections and hold your device identity graph. Headscale (Go, open-source) and Netmaker let you run that coordination layer yourself, removing the one company that otherwise sees your mesh topology.
- **Key practical notes:** Headscale is a drop-in replacement compatible with official Tailscale clients — good balance of usability and control. Best for connecting your own devices/servers together (self-hosted mail, Matrix, Nextcloud, etc.), not for general messaging.
- **Official links:** https://headscale.net, https://netmaker.io
- **Honest warning:** Still relies on WireGuard's design (no traffic obfuscation — visible as WireGuard traffic to a network observer). Requires you to run and secure the coordination server yourself.

<!-- Add new self-hosted tool here -->

---

## Email & Communication

### Proton Mail
Swiss-based encrypted email provider with zero-access encryption for stored mail.

- **Why it helps against Chat Control / mass scanning:** Mailbox content is encrypted such that Proton itself cannot read it (zero-access encryption), and Proton operates under Swiss law, outside direct EU regulatory reach, with a public history of legal pushback on overbroad requests. Note this protects content at rest — email metadata (sender, recipient, subject in transit to non-Proton recipients) is inherently less protected than in closed-garden E2EE messengers.
- **Key practical notes:** Free tier available; paid tiers add custom domains, more storage, Proton VPN bundling. Proton Mail Bridge allows use with any desktop email client (including Delta Chat, Thunderbird) while keeping encryption.
- **Official links:** https://proton.me/mail
- **Honest warning:** Email as a protocol leaks metadata to any non-Proton recipient's mail server by design (SMTP headers travel in the clear between providers) — Proton encrypts what it controls, not the entire email ecosystem. Subject to Swiss legal process, which, while stronger than EU/US in some respects, is not immune to international requests.

<!-- Add new email tool here -->

### Self-Hosted Mail (Mailcow, Mail-in-a-Box)
Running your own full mail server stack.

- **Why it helps against Chat Control / mass scanning:** Removes any third-party provider from your mail's data path entirely — you hold the logs, you control retention, no company to serve a scanning or metadata-retention order to except you.
- **Key practical notes:** Mailcow (Docker-based) and Mail-in-a-Box are the most turnkey self-hosted mail stacks available. Requires managing deliverability (SPF/DKIM/DMARC, IP reputation) — the hardest part of self-hosted email in practice.
- **Official links:** https://mailcow.email, https://mailinabox.email
- **Honest warning:** Email deliverability for self-hosted servers is genuinely difficult in 2026 — major providers (Google, Microsoft) aggressively spam-filter or reject mail from small/unknown IP ranges. Budget real time for this, or expect delivery problems.

<!-- Add new email tool here -->

### Bitwarden (self-hosted / Vaultwarden)
Password manager with an open-source, self-hostable server (Vaultwarden is the popular lightweight reimplementation).

- **Why it helps against Chat Control / mass scanning:** Not a comms tool directly, but credential compromise is a common vector for de-anonymization and account takeover that undermines every other tool on this list. Self-hosting via Vaultwarden means your vault (even though it's already E2E encrypted client-side) never touches a third-party server at all.
- **Key practical notes:** Vaultwarden runs comfortably on a Raspberry Pi or small VPS via Docker. Official Bitwarden apps are compatible with a self-hosted Vaultwarden backend.
- **Official links:** https://bitwarden.com, https://github.com/dani-garcia/vaultwarden
- **Honest warning:** Vaultwarden is an unofficial, community-maintained reimplementation of the Bitwarden server — not officially supported by Bitwarden Inc. Self-hosting means you are responsible for backups; losing your Vaultwarden database with no backup means losing every credential in it.

<!-- Add new email tool here -->

---

## VPN & Transport

A VPN does not make you anonymous and does not protect message content — it shifts trust from your ISP to your VPN provider. Choose based on jurisdiction, audited no-logs claims, and payment anonymity options.

### Mullvad
Privacy-first VPN, no email/account-name required, accepts cash and Monero.

- **Why it helps against Chat Control / mass scanning:** Mullvad assigns a random account number instead of requiring identity, and has a long track record of externally audited no-logs claims and, notably, has physically demonstrated raided servers containing no useful logs in real incidents. Sweden-based, subject to EU jurisdiction, but architecture minimizes what could be handed over even under compulsion.
- **Key practical notes:** Flat pricing regardless of usage, supports WireGuard and OpenVPN, has its own hardened browser (Mullvad Browser, in partnership with the Tor Project) for fingerprint resistance.
- **Official links:** https://mullvad.net
- **Honest warning:** A VPN only protects your traffic from your ISP/local network to Mullvad's exit — it does not anonymize you against the destination service if you're logged into an identifying account, and it does not protect message content, which is the job of E2EE messengers, not VPNs.

<!-- Add new VPN tool here -->

### Self-Hosted WireGuard
Run your own VPN endpoint on a VPS you control, instead of trusting any commercial VPN provider.

- **Why it helps against Chat Control / mass scanning:** Removes the VPN provider as a third party entirely — the only entity that could be compelled is you and your VPS host, and you control what (if anything) is logged.
- **Key parameters:** WireGuard is fast, modern, minimal-attack-surface, and has straightforward config generation tools (e.g. `wg-easy` for a web UI, or plain `wg-quick`).
- **Key practical notes:** Doesn't give you the "many users sharing exit IPs" anonymity set that a commercial VPN provides — a self-hosted VPN on a VPS in your name is traceable to you far more easily than a shared commercial exit.
- **Official links:** https://www.wireguard.com, https://github.com/wg-easy/wg-easy
- **Honest warning:** A self-hosted single-user VPN provides encryption-in-transit and IP-hiding-from-local-network benefits, but does **not** provide anonymity — your VPS provider and, if subpoenaed, the VPS billing records, link directly back to you. Use Tor if the threat model requires anonymity, not just encryption.

<!-- Add new VPN tool here -->

### Tor
The original, still-relevant low-latency anonymity network, multi-hop by design.

- **Why it helps against Chat Control / mass scanning:** No single relay operator sees both origin and destination — the three-hop circuit design means compelling any one relay operator reveals nothing about the full path. This is the strongest anonymity (not just encryption) primitive in this list.
- **Key practical notes:** Use Tor Browser for browsing; Briar and some other tools listed here can route through Tor natively. Onion services let you self-host something reachable without exposing your server's real IP.
- **Official links:** https://www.torproject.org
- **Honest warning:** Meaningfully slower than direct connections or VPNs, and some services block or challenge Tor exit IPs aggressively, which can itself be a fingerprinting/friction vector.

<!-- Add new VPN tool here -->

---

## Storage & Files

### Cryptomator
Client-side encryption layer for files stored in any cloud provider (Dropbox, Google Drive, Proton Drive, etc.).

- **Why it helps against Chat Control / mass scanning:** Encrypts individual files and filenames client-side before they ever reach the cloud provider, so the storage provider — regardless of what it's later compelled to hand over — only ever holds ciphertext blobs it cannot read.
- **Key practical notes:** Open-source, works transparently as a virtual drive on desktop and mobile, layers on top of any existing cloud storage you already use — no need to migrate providers.
- **Official links:** https://cryptomator.org
- **Honest warning:** Metadata like file sizes and modification timestamps at the storage provider are not hidden — only content and filenames are encrypted, so some patterns of activity remain visible to the storage provider.

<!-- Add new storage tool here -->

### VeraCrypt
Full-disk and container-based encryption, successor to TrueCrypt.

- **Why it helps against Chat Control / mass scanning:** Protects data at rest against seizure — a compelled disclosure or device seizure yields nothing usable without the passphrase (and hidden volumes provide plausible deniability against compelled disclosure of a password).
- **Key practical notes:** Cross-platform (Windows/Mac/Linux). Use for encrypted containers, full-disk encryption, or hidden volumes. Combine with a strong, memorized (not written down) passphrase.
- **Official links:** https://veracrypt.fr
- **Honest warning:** No cloud sync built in — it's a local encryption tool, not a sharing/collaboration tool. A forgotten passphrase means genuinely unrecoverable data — there is no backdoor, including for you.

<!-- Add new storage tool here -->

### Self-Hosted Nextcloud
Full self-hosted cloud storage/sync/office suite replacing Google Drive/Dropbox/O365.

- **Why it helps against Chat Control / mass scanning:** Removes the third-party cloud provider from the picture for file storage the same way self-hosting Matrix does for messaging — you are the data controller.
- **Key practical notes:** Combine with Cryptomator for client-side encryption on top, since Nextcloud's own server-side encryption is not zero-knowledge by default.
- **Official links:** https://nextcloud.com
- **Honest warning:** Self-hosted storage needs real backup discipline — a self-hosted single point of failure with no offsite backup is a good way to lose everything to a hardware failure, not just to legal compulsion.

<!-- Add new storage tool here -->

---

## Operating Systems & Browsers

### GrapheneOS
Hardened, privacy/security-focused Android-based mobile OS with no Google services by default.

- **Why it helps against Chat Control / mass scanning:** Removes Google Play Services (a major telemetry and potential compelled-access surface) by default, includes hardened memory allocator, sandboxing improvements, and network/sensor permission controls beyond stock Android. A cleaner OS baseline matters because messenger app-layer protections mean less if the underlying OS is compromised or leaking metadata to a platform vendor.
- **Key practical notes:** Only supported on Google Pixel hardware (due to Pixel's strong verified-boot and firmware update support). Sandboxed Google Play compatibility layer available if you need specific Google-dependent apps.
- **Official links:** https://grapheneos.org
- **Honest warning:** Hardware-locked to Pixel devices — buying Pixel hardware means still funding Google, even though you're not running Google's OS on it. Some banking/anti-fraud apps flag or refuse to run on de-Googled devices.

<!-- Add new OS tool here -->

### Mullvad Browser
Hardened Firefox-based browser built with the Tor Project, focused on eliminating fingerprinting rather than just blocking trackers.

- **Why it helps against Chat Control / mass scanning:** Uniform fingerprint across all users (same window size, fonts, canvas behavior) makes individual browser-fingerprint-based tracking far less effective — most tracking today isn't cookies, it's fingerprinting.
- **Key practical notes:** Works well paired with Mullvad VPN (though not required) or Tor. No account or telemetry.
- **Official links:** https://mullvad.net/en/browser
- **Honest warning:** Deliberately breaks some fingerprinting-resistant behaviors that trade off convenience (e.g., no persistent extensions, resized windows can reduce your anonymity set) — read the included documentation on how to use it correctly, since using it wrong (maximizing the window, installing extensions) undermines the entire point.

<!-- Add new browser tool here -->

### Tor Browser
Firefox-based browser pre-configured to route all traffic through Tor with anti-fingerprinting defaults.

- **Why it helps against Chat Control / mass scanning:** Combines network-level anonymity (Tor circuit) with browser fingerprinting resistance, the closest thing to an off-the-shelf anonymity solution for general web use.
- **Key practical notes:** Use the default window size and don't install extensions — both reduce your anonymity set by making your browser instance more unique.
- **Official links:** https://www.torproject.org/download/
- **Honest warning:** Meaningfully slower browsing experience; some sites actively block Tor exit nodes.

<!-- Add new browser tool here -->

---

## Additional Hardening

- **2FA via hardware keys (YubiKey, Nitrokey):** Use FIDO2/WebAuthn hardware keys instead of SMS or app-based 2FA where possible — SMS 2FA is a SIM-swap and metadata-linkage risk.
- **De-Google / de-Apple your phone where feasible:** Even alongside good messaging apps, a stock OS with full vendor telemetry undermines your other efforts. GrapheneOS (above) is the most mature option; /e/OS is a lower-friction alternative for non-Pixel hardware with weaker hardening guarantees.
- **Metadata minimization discipline:** The best tool is useless if you also use a mainstream, centralized app for the same contacts "just in case" — parallel weak channels leak the same information the strong channel was protecting.
- **DNS-over-HTTPS/TLS with a privacy-respecting resolver:** e.g., Mullvad's own DNS, or self-hosted Pi-hole + Unbound, to stop your ISP or default resolver from logging every domain you resolve.
- **Firmware and update hygiene:** Full-disk encryption is worthless if the device boots an untrusted or out-of-date bootloader — keep OS and firmware updates current on all hardened devices.

<!-- Add new hardening tip here -->

---

## Quick Migration Paths

### WhatsApp → SimpleX Chat or Matrix/Element
1. Install SimpleX Chat on your phone; it needs no phone number, so you can run it alongside WhatsApp during transition.
2. Create your first connection link and share it with your closest contacts via an existing trusted channel.
3. For group-heavy use cases (family groups, communities), stand up a small Matrix homeserver (Conduit is the lightest to self-host) and invite your group into an Element space instead — it more closely mirrors WhatsApp's group UX.
4. Once your core contact graph has migrated, stop posting to WhatsApp groups and archive the app rather than deleting history immediately, in case of stragglers.

### Telegram → Session or Delta Chat
1. Telegram is **not E2EE by default** (only in opt-in "Secret Chats", and never for groups) — treat any migration off Telegram as urgent for group communications specifically.
2. For 1:1 replacement with minimal friction and no phone number requirement, move contacts to Session.
3. For community/broadcast-style groups currently run on Telegram channels, evaluate a self-hosted Matrix space (better moderation tooling, federation) or Delta Chat mailing-list-style groups if your community already trusts a shared email domain.
4. Export any needed chat history from Telegram before migrating (Settings → Advanced → Export Telegram Data) since group history won't transfer automatically.

### Google Drive/Dropbox → Cryptomator + Any Cloud, or Self-Hosted Nextcloud
1. Quick path: keep your existing cloud storage account, install Cryptomator, and create an encrypted vault inside your existing synced folder — this requires zero changes to your provider or sharing workflows, only that files are now ciphertext to that provider.
2. Long path: stand up a self-hosted Nextcloud instance on a VPS or home server, migrate files over, and layer Cryptomator inside it for content the provider itself (even though it's you) shouldn't be able to read in plaintext (e.g. if the VPS itself could be compelled/seized).
3. Migrate password manager to self-hosted Vaultwarden in parallel, since it's the credential store gating everything else.

<!-- Add new migration path here -->

---

## Opsec Basics

These matter more than any single tool choice — bad operational security defeats good cryptography every time.

- **Compartmentalize identities.** Don't reuse the same username/avatar/writing style across a "clean" identity and a sensitive one — stylometric and metadata correlation is a real deanonymization vector, not a theoretical one.
- **Assume metadata is the real target.** Content encryption is table stakes in 2026; who-talked-to-whom-when is what most mass surveillance regimes actually optimize for. Favor tools that minimize this (SimpleX, Briar, Session) for your most sensitive contacts.
- **Minimize the parallel weak channel.** If you migrate a contact to a strong tool but keep messaging them "sometimes" on WhatsApp/Telegram/SMS, that weak channel is now your actual security level — attackers target the weakest link, not the one you're proudest of.
- **Verify software integrity.** Download apps from official sites/F-Droid/verified app stores, verify PGP signatures or checksums for anything sensitive (VeraCrypt, self-hosted server binaries), and keep everything patched.
- **Physical security still matters.** Full-disk encryption is defeated by an unlocked, unattended device. Use short auto-lock timers and strong device passcodes, not just app-level security.
- **Backups need the same threat model as the original.** An encrypted phone backed up in plaintext to an unencrypted cloud account defeats the entire exercise — encrypt backups with the same rigor as the primary device.
- **Don't over-engineer your threat model.** Most people don't need Tails + Tor + Briar for daily life — match tool choice to actual risk. Over-complicated opsec that you abandon after two weeks protects you less than a simpler setup you actually maintain.

---

## Contributing & Updates

This repo tracks a moving legislative and technical target — Chat Control derogations, CSAR negotiations, and the underlying tools all change. Contributions are welcome:

- **New tools:** Open a PR adding a tool in the appropriate section, following the existing format (name, one-liner, "why it helps," practical notes, official link, honest limitation). Use the `<!-- Add new tool here -->` anchors as insertion points.
- **Corrections:** If a tool's architecture, jurisdiction, or audit status has changed, open an issue or PR — accuracy here matters more than completeness.
- **Legislative updates:** If you're tracking the EU Council/Parliament process on Chat Control/CSAR, PRs updating the situational summary at the top are especially welcome — cite the specific document or vote.
- **No affiliate links, no unverified claims.** Every tool listed here should be independently verifiable — link to official sources only.

```bash
# Fork, branch, and submit
git checkout -b add-tool-name
# edit the relevant section
git commit -m "Add <tool> to <section>"
git push origin add-tool-name
# open a PR
```

---

Reach out on X @ricardgardella
