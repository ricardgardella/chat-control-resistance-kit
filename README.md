# Chat Control Resistance Kit

A practical toolkit for reducing your exposure to EU Chat Control-style scanning mandates and mass communications surveillance.

On 9 July 2026, the European Parliament revived the "temporary" Chat Control 1.0 derogation through a procedural maneuver. E2EE services got an explicit carve-out this round, but that's a policy choice, not an architectural guarantee, and it can be narrowed or removed next cycle. The permanent CSAR (which would mandate client-side scanning regardless of encryption) is still in play. Every tool below is picked for its architecture, not its current policy or goodwill, so mass scanning and single-point compulsion stay expensive or impossible no matter which way future votes go.

## Table of Contents

- [Messaging](#messaging)
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

Priority goes to architectures with no central server that can be legally compelled to scan, log, or backdoor traffic for everyone at once.

- **SimpleX Chat** - No phone number, no username, no account graph to subpoena, self-hostable relays. Younger project, smaller audit history than Signal/Matrix. https://simplex.chat
- **Briar** - True peer-to-peer over Tor, Bluetooth, or Wi-Fi Direct, no server needed at all. Android only, no iOS build is possible. https://briarproject.org
- **Matrix + Element** - Federated protocol; self-hosting your own homeserver makes you your own data controller. Metadata (who's talking to whom) is still visible to your homeserver. https://matrix.org, https://element.io
- **Delta Chat** - Runs entirely over your own email (IMAP/SMTP), so there's no Delta Chat server to compel. Inherits your email provider's metadata exposure. https://delta.chat
- **Threema** - Swiss jurisdiction, no phone number required, open-source clients. Still a single centralized company, paid app. https://threema.ch
- **Signal** - The best-audited E2EE protocol in mass use, easiest migration path for non-technical contacts. Single company, single US jurisdiction, requires a phone number to register. https://signal.org

<!-- Add new messaging tool here -->

---

## Email & Communication

- **Proton Mail** - Zero-access encryption, Swiss jurisdiction, free tier available. Email metadata still leaks to non-Proton recipients by design. https://proton.me/mail
- **Self-Hosted Mail (Mailcow, Mail-in-a-Box)** - Full control of your own mail server, no third party to compel. Deliverability (SPF/DKIM/DMARC, IP reputation) is genuinely hard to get right. Can https://mailcow.email, https://mailinabox.email
- **Bitwarden (self-hosted / Vaultwarden)** - Self-hosted password vault that never touches a third-party server. Vaultwarden is an unofficial community reimplementation; backups are on you. https://bitwarden.com, https://github.com/dani-garcia/vaultwarden

<!-- Add new email tool here -->

---

## VPN & Transport

A VPN hides your traffic from your ISP and shifts trust to the provider. It does not make you anonymous, does not protect message content, and is architecturally different from Tor.

- **Mullvad** - Random account number, no email required, accepts cash and Monero, long audited no-logs track record. Sweden-based, subject to EU jurisdiction. https://mullvad.net
- **Proton VPN (really good option)** - Five straight years of independently audited no-logs claims, tested in 400+ real legal requests. https://protonvpn.com
- **Self-Hosted WireGuard** - Run your own VPN endpoint, cutting any commercial provider out of the picture. Traceable directly to you via VPS billing records; doesn't provide anonymity. https://www.wireguard.com
- **Tor (not a VPN)** - Three-hop anonymity network; no single relay sees both who you are and where you're going. Can be combined with a VPN to have a double security and privacy protection.https://www.torproject.org

> **Exit-country tip:** the country your exit VPN server sits in matters on its own, separate from where the VPN company is based, because it decides whose courts and data-retention laws apply. as per 10-07-2026, **Iceland** is the strongest current pick (no VPN-specific data retention, not in any Eyes alliance, no US MLAT). **Switzerland** is good but watch it closely: a proposed 2026 surveillance law (VÜPF) could force mandatory logging and ID verification on Swiss-based providers, including Proton VPN.

<!-- Add new VPN tool here -->

---

## Storage & Files

- **Cryptomator** - Encrypts files and filenames client-side before any cloud provider (Dropbox, Google Drive, Proton Drive) ever sees them. File sizes and timestamps stay visible to the provider. https://cryptomator.org
- **VeraCrypt** - Full-disk and container encryption; a forgotten passphrase means genuinely unrecoverable data, no backdoor, including for you. Local only, no cloud sync. https://veracrypt.fr
- **Self-Hosted Nextcloud** - Full cloud storage/sync/office suite you control entirely. Needs real backup discipline to avoid becoming its own single point of failure. https://nextcloud.com

<!-- Add new storage tool here -->

---

## Operating Systems & Browsers

- **GrapheneOS** - Hardened Android, no Google Play Services by default. Pixel-only hardware for now. https://grapheneos.org
- **Mullvad Browser** - Uniform fingerprint across all users, built with the Tor Project. Deliberately breaks some convenience features (persistent extensions, custom window size) to stay fingerprint-resistant. https://mullvad.net/en/browser
- **Tor Browser** - Combines Tor's network anonymity with fingerprinting resistance. https://www.torproject.org/download/
- **Brave** - An open-source privacy focused browser. https://brave.com

<!-- Add new browser tool here -->

---

## Additional Hardening

- **2FA via hardware keys (YubiKey, Nitrokey)** - FIDO2/WebAuthn beats SMS 2FA, which is a SIM-swap and metadata-linkage risk.
- **De-Google / de-Apple if feasible** - GrapheneOS is the most mature option; works on non-Pixel hardware with weaker hardening. Apple is considered more private than Android.
- **Firmware and update hygiene** - Full-disk encryption is worthless on a device booting an untrusted or out-of-date bootloader.

<!-- Add new hardening tip here -->

---

## Quick Migration Paths

**WhatsApp → SimpleX Chat or Matrix/Element**
Install SimpleX Chat (no phone number needed, run it alongside WhatsApp during transition), migrate close contacts first, then stand up a small Matrix homeserver for group-heavy use cases before archiving WhatsApp.

**Telegram → SimpleX Chat or Delta Chat**
Telegram isn't E2EE by default and never for groups, so treat this as urgent. Move 1:1 contacts to SimpleX Chat; move community/broadcast groups to a self-hosted Matrix space or Delta Chat. Export chat history first (Settings → Advanced → Export Telegram Data).

**Google Drive/Dropbox → Cryptomator, or Self-Hosted Nextcloud**
Quick path: install Cryptomator on your existing cloud account, zero migration needed. Long path: stand up self-hosted Nextcloud and layer Cryptomator on top. Migrate your password manager to self-hosted Vaultwarden in parallel.

<!-- Add new migration path here -->

---

## Opsec Basics

Bad operational security defeats good cryptography every time.

- **Compartmentalize identities.** Don't reuse the same username, avatar, or writing style across a clean identity and a sensitive one.
- **Assume metadata is the real target.** Who talked to whom, and when, is what mass surveillance actually optimizes for. Favor SimpleX or Briar for your most sensitive contacts.
- **Minimize the parallel weak channel.** A contact you also message "sometimes" on WhatsApp/SMS makes that weak channel your real security level.
- **Verify software integrity.** Download from **official sites*8 or F-Droid, verify checksums for anything sensitive, keep everything patched.
- **Physical security still matters.** Full-disk encryption is defeated by an unlocked, unattended device.
- **Backups need the same threat model as the original.** An encrypted phone backed up in plaintext to an unencrypted cloud account defeats the exercise.
- **Don't over-engineer your threat model.** Match tool choice to actual risk; a simple setup you maintain beats a complex one you abandon after two weeks.

---

## Contributing & Updates

This repo tracks a moving legislative and technical target. See [CONTRIBUTE.md](CONTRIBUTE.md) for how to submit a tool or a fix.

---

Reach out on X @ricardgardella
