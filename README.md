# macOS Setup, Deliberately

> Every setting from first boot to a backed-up baseline, with what you gain and what it costs. GUI-first, any Apple silicon Mac laptop.
> No SIP changes · No Gatekeeper bypasses · No debloat scripts · Every change reversible

---

## Table of Contents

1. [Overview](#1-overview)
2. [Phase 1: Pre-Flight & Setup Assistant](#2-phase-1-pre-flight--setup-assistant)
3. [Phase 2: macOS Update Pass](#3-phase-2-macos-update-pass)
4. [Phase 3: Travel Security Baseline](#4-phase-3-travel-security-baseline)
5. [Phase 4: iCloud on a Small Plan](#5-phase-4-icloud-on-a-small-plan)
6. [Phase 5: Apple Intelligence & Siri Decision](#6-phase-5-apple-intelligence--siri-decision)
7. [Phase 6: Notifications, Spotlight & Nag Cleanup](#7-phase-6-notifications-spotlight--nag-cleanup)
8. [Phase 7: Remove Unnecessary Apps](#8-phase-7-remove-unnecessary-apps)
9. [Phase 8: Login Items & Background Items](#9-phase-8-login-items--background-items)
10. [Phase 9: Privacy & Security Permissions Audit](#10-phase-9-privacy--security-permissions-audit)
11. [Phase 10: Battery Settings](#11-phase-10-battery-settings)
12. [Phase 11: Continuity Setup & Verification](#12-phase-11-continuity-setup--verification)
13. [Phase 12: Browser & Web Apps](#13-phase-12-browser--web-apps)
14. [Phase 13: Minimal Dev Setup](#14-phase-13-minimal-dev-setup)
15. [Phase 14: iPad File Transfer Workflow](#15-phase-14-ipad-file-transfer-workflow)
16. [Phase 15: Final Checks & Clean Baseline Backup](#16-phase-15-final-checks--clean-baseline-backup)
17. [Phase 16: Erase & Reset for Trade-In](#17-phase-16-erase--reset-for-trade-in)
18. [Quick Reference Card](#18-quick-reference-card)
19. [Appendix: Troubleshooting](#19-appendix-troubleshooting)
20. [Appendix: Sources](#20-appendix-sources)

---

## 1. Overview

> 📌 **The idea.** macOS doesn't ship with much bloat. Most of setup is a series of choices: each one buys you something (battery, privacy, security, convenience) and costs you something else. This guide puts both sides next to every change, so you decide on purpose instead of clicking Continue.

This document takes an Apple silicon Mac laptop from the first Setup Assistant screen to a hardened, low-nag, backed-up baseline. It's tuned for a common use case: a laptop that travels, handles browsing, admin, and light development, and works as part of an Apple ecosystem (iPhone, iPad, Apple Watch, AirPods). If your Mac is a desk-bound workhorse, everything still applies — you'll just care less about the travel-hardening and battery phases.

Almost every step goes through **System Settings**, an app's own **Settings** window, or **Finder**. Terminal appears only where there's no GUI equivalent, and every such command is marked **Optional**, is reversible, and comes with its undo command. Nothing here touches SIP, Gatekeeper, the sealed system volume, or the update mechanism — so resale, Erase All Content and Settings, and future OS upgrades all keep working.

**Which macOS this guide targets:**

| | Status as of Sept 30, 2026 |
|---|---|
| **In the box** | Macs launched before September 2026 (e.g. the M5 MacBook Air, March 2026) shipped on **macOS Tahoe 26**. Apple doesn't publish the factory build per unit; expect a 26.x build depending on when yours was boxed. Newer stock may arrive on 27. |
| **Current release** | **macOS 27 Golden Gate** — released Sept 14, 2026; **27.0.1** shipped Sept 28, 2026. Runs on every Apple silicon Mac (M1 and later, plus MacBook Neo); Intel Macs are not supported. |
| **Still supported** | macOS Tahoe 26.7.1 (security update, Sept 28, 2026). |
| **This guide** | Written against **macOS 27.0.1**. Where Tahoe 26 differs (mostly the Siri/Apple Intelligence pane), it's called out. |

**This guide covers:**

| ✅ In scope | ❌ Out of scope |
|---|---|
| Setup Assistant choices, with honest trade-offs | Disabling SIP, Gatekeeper, or the Signed System Volume |
| macOS 27 update pass and auto-update policy | "Debloat" scripts, MDM profiles, or `launchctl` surgery on system daemons |
| FileVault, Find My, firewall, lock screen, AirDrop, accessory hardening | Third-party antivirus / "cleaner" apps (CleanMyMac etc.) |
| iCloud selective sync on a small (e.g. 50GB) plan | Using iCloud as bulk storage |
| Apple Intelligence / Siri AI decision | Local LLM or AI dev tooling |
| Notification, Spotlight, Safari suggestion cleanup | Boot Camp / Linux (not supported on Apple silicon Macs) |
| Login items, background items, permissions audit | Heavy dev environments (Docker, Xcode.app, simulators) |
| Battery settings including Charge Limit | Third-party battery managers (AlDente etc.) — not needed on 26.4+ |
| Continuity setup and verification | Gaming / Arcade setup |
| Third-party browser (Brave as the example), Safari web apps, minimal git/SSH setup | |
| iPad file workflow, Time Machine baseline, sale/trade-in reset | |

> 📌 **Device-specific vs. universal.** This guide was written on a 13" MacBook Air M5 (16GB / 512GB). Everything applies to any Apple silicon MacBook Air or MacBook Pro on macOS 27. Where hardware matters, it's called out: Apple Intelligence's enhanced dictation/voices need **M3 or later with 12GB+ RAM**; MacBook Pro models show an **Energy Mode** menu with a High Power option that the fanless Air doesn't have; and storage advice assumes a 512GB drive — tighten it on 256GB.

> 📌 **How to read confidence in this guide.** Anything stated flatly is documented — by Apple Support / the Mac User Guide for macOS 27, or the vendor's own docs (GitHub, Homebrew). Anything tagged ***(inference)*** is my reasoning, not a documented guarantee. Anything tagged ***(verify on 27)*** is a label that's been stable for several releases but that I couldn't confirm word-for-word against Apple's 27 docs — if the name on screen differs slightly, it's that. Sources are listed in the last appendix.

> ℹ️ **Callout legend** — 📌 key context · ⚠️ warning / easy to get wrong · ℹ️ background info · 💡 tip · 🔒 security/privacy note.

---

## 2. Phase 1: Pre-Flight & Setup Assistant

Estimated time: 15–25 minutes, not counting the OS update.

**Before you open the lid:**
- Have your Apple Account password and a trusted device (for 2FA and "Set up with iPhone") in hand.
- Know which Wi-Fi you're using — use a trusted network, not a hotel/café one, for first setup.
- Decide now: **set up as new.** Don't migrate from another machine; the whole point is a clean baseline.

Move through Setup Assistant in order. Screen order shifts a little between builds; the choices don't.

1. **Language / Country or Region** — set your actual country.

> ⚠️ **Don't fake an EU region for EU-only perks.** iPhone Mirroring is documented as unavailable in the European Union. If you live outside the EU, keep the Mac, iPhone, and Apple Account on your real region or you risk losing Continuity features. (If you're in the EU, iPhone Mirroring simply isn't available to you — skip that row in Phase 11.) *(Exactly which signal Apple keys the EU block to — Mac region, Apple Account country, or physical location — isn't documented. Don't test it.)*

2. **Choose how to set up your Mac** — Apple now offers: set up **with iPhone or iPad**, transfer from another Mac / Time Machine / startup disk, transfer from a Windows PC, or set up as new.
   - Using **Set up with iPhone** is fine — it's a convenience for sign-in and Wi-Fi, not a data migration. Otherwise pick **Set up as new** / **Not Now** on transfer.
3. **Accessibility** — **Not Now** unless you need something.
4. **Wi-Fi** — connect.
5. **Apple Account** — sign in. Required for Find My, Continuity, iCloud Keychain/Passwords, and Apple subscriptions. Approve 2FA on your trusted device.
6. **Terms and Conditions** — accept.
7. **Create a Computer Account** — this is your local macOS login.
   - **Full name / account name:** the *account name* becomes your home folder (`/Users/<name>`) and is painful to change later. Pick something short and lowercase, e.g. `alex`.
   - **Password:** use a real passphrase. On a travel laptop this is what stands between a thief and your data after a restart (Touch ID doesn't work until the password has been entered once after boot).
   - If offered, **don't** tick "Allow my Apple Account to reset this password" on a travel machine *(inference: it adds a second path to your local login that depends on account security rather than the device itself; your recovery plan is the FileVault recovery key below)*.
8. **Location Services** — **Enable.** Find My needs it, and so do time zone and Wi-Fi-based location. You'll trim per-app access in Phase 9.
9. **Time Zone** — leave on automatic.
10. **Analytics** — **uncheck everything** (Share Mac Analytics, share with app developers, and any Siri/iCloud analytics boxes on this screen). None are required for anything to work.
11. **Screen Time** — **Set Up Later.** It's for usage limits and parental controls. Useful mainly as a lever for restricting Apple Intelligence (Phase 5) — you don't need it on day one.
12. **Siri / Apple Intelligence** — see Phase 5 before deciding. If you want to decide later: **skip / Not Now**. You can turn Siri AI on in System Settings at any time; the reverse is also true.
13. **FileVault** — **turn it ON.** When asked how to recover:
    - **Create a recovery key and do not use my iCloud account** ← recommended for this machine.
    - Store that key in **two** places that don't travel with the laptop: e.g. your phone's password manager *and* on paper at home.
    - Full reasoning and trade-offs in Phase 3.
14. **Touch ID** — enroll at least two fingers (e.g. both index fingers). Add a third later for the hand that's usually on the mouse.
15. **Apple Pay** — optional. Enable if you buy things in Safari; skip otherwise. It's a card on the Mac's Secure Enclave, not a data-sharing toggle.
16. **Choose Your Look** — your call. Liquid Glass transparency is now adjustable later at `System Settings > Appearance`.
17. **Update Mac Automatically / Software Updates** — accept the defaults; you'll tune the policy in Phase 2.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Clean account with no migrated junk, launch agents, or old preferences | Everything gets set up by hand — that's the point, but it takes longer |
| Analytics off from minute one — nothing collected before you can reach Settings | None functional; Apple uses these for crash/bug triage |
| FileVault recovery key held by you, not recoverable via your Apple Account | Lose the password *and* the key and the data is gone — Apple can't recover it |
| Location on keeps Find My and auto time zone working for travel | Location Services is broad by default — audited in Phase 9 |

---

## 3. Phase 2: macOS Update Pass

Estimated time: 30–60 minutes including download and restart, mostly unattended. Plug into power.

1. `System Settings > General > Software Update`.
2. If the Mac shipped on Tahoe 26.x, you'll see both a Tahoe update and an **Upgrade to macOS Golden Gate** option.
   - **Recommended:** upgrade to **macOS 27.0.1** now. You're setting up from scratch, so there's nothing to break, and doing the major-version jump before configuration means you won't redo settings that move.
   - **Conservative alternative:** take the latest Tahoe 26.7.x update and wait for 27.1. Apple is still shipping Tahoe security updates. Everything in this guide works on Tahoe 26.4+ except where noted.
3. After restart, check again — a follow-up (Security Response, app updates) often appears.
4. Click the **ⓘ** next to **Automatic Updates** and set:

| Option | Setting | Why |
|---|---|---|
| Download new updates when available | **On** | Downloads in the background; doesn't install without you |
| Install macOS updates | **Off** (or On, your call) | Off means you pick when a 30-minute install happens — not mid-trip on hotel Wi-Fi |
| Install Security Responses and system files | **On** | Small, rapid security fixes. Leave this on for a travel machine. |

5. **App Store updates:** open **App Store > Settings** and turn on automatic updates for App Store apps *(verify on 27 — this moved out of Software Update in recent releases)*.

> 🔒 **Travel note.** Don't sit on security updates while traveling. If you turned off *Install macOS updates*, make a habit of `Software Update` on every trusted network before a trip.

> 💡 **Rosetta.** macOS 27 is documented as the final major release with general-purpose Rosetta support for Intel-only apps. Don't install Rosetta unless an app asks for it — and treat any app that needs it as a candidate for replacement before your trade-in window.

---

## 4. Phase 3: Travel Security Baseline

Estimated time: 10 minutes. This is the phase that matters most for a laptop that goes everywhere.

### 4.1 FileVault — ON (verify)

`System Settings > Privacy & Security > FileVault` → confirm **On**.

> ℹ️ **What FileVault actually adds on Apple silicon.** The internal SSD is always hardware-encrypted by the Secure Enclave. FileVault ties the decryption key to your login password, so a powered-off or freshly booted Mac is unreadable without it. Performance cost on Apple silicon is effectively zero — the encryption is hardware either way.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Stolen laptop = brick of ciphertext until someone knows your password | Forget password + lose recovery key = data permanently gone |
| Required baseline for any real travel threat model | Recovery key must be stored somewhere safe and **not** in the bag |
| Zero measurable performance cost on Apple silicon | iCloud-based recovery is more convenient, but routes recovery through your Apple Account instead of a key you hold |

> 🔒 **Border crossings / high-risk moments.** Shut the Mac **down** rather than sleeping it. After a restart, Touch ID and Apple Watch unlock won't work until the password is entered, and the FileVault key isn't in memory. *(Inference on the practical threat model — the restart behavior itself is documented.)*

### 4.2 Find My Mac — ON (with trade-offs)

`System Settings > [your name] > iCloud > See All > Find My Mac` → **Turn On**, and turn on the **Find My network**.

If Find My isn't offered: `System Settings > Privacy & Security > Location Services` → make sure Location Services is on → **System Services > Details** → **Find My Mac** on.

Turning on Find My Mac **automatically enables Activation Lock**.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Locate, play sound, lock, or erase remotely from iPhone or iCloud.com | Device location is available to anyone who gets into your Apple Account — your account security *is* the Mac's location privacy |
| Activation Lock makes a stolen Mac nearly worthless to resell | Must be removed before trade-in (Phase 16) or the buyer/trade-in program can't use it |
| Find My network can locate it while offline/asleep via nearby Apple devices | Small ongoing Bluetooth beaconing *(inference: no measurable battery impact documented)* |

> 🔒 **Your Apple Account is now the master key to this laptop's location and remote wipe.** Use a strong unique password, keep 2FA trusted numbers current, and consider **Advanced Data Protection** (`System Settings > [your name] > iCloud > Advanced Data Protection`) with a recovery key or recovery contact set first. ADP end-to-end encrypts most iCloud data categories, including backups and Notes.

### 4.3 Lock Screen & Password on Wake

`System Settings > Lock Screen`:

| Setting | Value |
|---|---|
| Turn display off on battery when inactive | **2 minutes** (battery + security) |
| Turn display off on power adapter when inactive | 10 minutes |
| Require password after screen saver begins or display is turned off | **Immediately** |
| Show password hints | **Off** |
| Show message when locked | Optional — a "if found, contact …" email *(not your phone number)* |
| Login window shows | **Name and password** (doesn't reveal the account name) |

> 💡 **Lock on demand:** `Control + Command + Q`. Or set a Hot Corner: `System Settings > Desktop & Dock > Hot Corners…` → **Lock Screen**.

### 4.4 Firewall

`System Settings > Network > Firewall` → **On**. Then **Options…**:

- **Enable stealth mode** → **On**. The Mac doesn't answer pings or probe attempts on hostile networks.
- **Block all incoming connections** → leave **Off** normally. Turn it **On** at airports/hotels if you want, but it blocks File Sharing (your SMB-to-iPad path in Phase 14) while it's on *(inference from Apple's description of the option; test on your network)*.
- Leave "automatically allow built-in / downloaded signed software" at defaults.

### 4.5 AirDrop — Contacts Only

`System Settings > General > AirDrop & Continuity` → **AirDrop: Contacts Only**.

> 📌 **Renamed pane.** In Ventura–Sequoia this was *AirDrop & Handoff*. On current Tahoe builds and macOS 27 it's **AirDrop & Continuity**.

Your own iPhone/iPad (same Apple Account) always see this Mac under Contacts Only. **Everyone for 10 Minutes** is there when you need a one-off from a stranger, and it reverts on its own.

On the same page: **AirPlay Receiver → Off** on a travel machine (it advertises the Mac as a target on every network you join). Turn it on when you need it.

### 4.6 Sharing services & hostname

`System Settings > General > Sharing`:
- Everything **Off** by default (Screen Sharing, Remote Login, Remote Management, Media Sharing, Printer Sharing, Internet Sharing, Bluetooth Sharing). File Sharing gets turned on deliberately in Phase 14 if you want the SMB path.
- **Local hostname → Edit** → set something generic, e.g. `mba-13`. By default it's "Alexs-MacBook-Air" (your first name), which is broadcast over Bonjour on every network you join *(inference: minor, but free to fix)*. Also update `System Settings > General > About > Name` to match.

### 4.7 USB/Thunderbolt accessories

`System Settings > Privacy & Security > Allow accessories to connect` → keep **Ask for new accessories** (default) or tighten to **Always ask** for travel. This blocks data connections from unknown USB-C devices (charging kiosks, rogue cables) until you approve them on an unlocked Mac.

### 4.8 Wi-Fi hygiene

`System Settings > Wi-Fi` → **Details…** next to each network → **Private Wi-Fi address: Rotating** for public networks; **Fixed** for home/trusted networks where the router tracks you by MAC.

Also on the Wi-Fi page, turn **Ask to join networks** / **Ask to join hotspots** to your preference *(verify on 27)*, and **forget** hotel networks after checkout.

### 4.9 Optional: Lockdown Mode

`System Settings > Privacy & Security > Lockdown Mode`. Designed for people targeted by mercenary spyware; it disables a lot (some web tech, message attachments, shared albums, incoming FaceTime from unknown callers, wired connections while locked). **Not recommended as a default** — it will break normal ecosystem stuff. Worth knowing it exists if a specific trip's threat model changes.

| ✅ Benefits (Phase 3 overall) | ⚠️ Possible drawback |
|---|---|
| Lost or stolen Mac is unreadable, locatable, and unsellable | Account compromise = location exposure + remote-wipe risk; account security is load-bearing |
| Mac is quiet on hostile networks (stealth firewall, no AirPlay, generic hostname) | Block-all firewall breaks SMB to the iPad while on |
| AirDrop spam from strangers is off | Immediate password-on-wake adds friction — Apple Watch unlock (Phase 11) removes most of it |

---

## 5. Phase 4: iCloud on a Small Plan

Estimated time: 5 minutes. This phase assumes a small iCloud plan (the free 5GB, 50GB, or 200GB) shared across several devices. In that case iCloud is for **syncing small, high-value data**, not file storage. If you pay for 2TB+ and want iCloud as your file store, keep Desktop & Documents on and skip the Off column.

`System Settings > [your name] > iCloud`. Under **Saved to iCloud**, click **See All** and set:

| Keep ON | Turn OFF (or keep off) |
|---|---|
| **Passwords & Keychain** — if Apple Passwords is your password manager | **Photos** — the iPhone/iPad library shouldn't be mirrored onto a 512GB travel laptop. Access via iCloud.com or AirDrop when needed. *(If you want Photos on the Mac, turn on **Optimize Mac Storage** in Photos settings.)* |
| **Find My Mac** | **iCloud Drive → Desktop & Documents Folders** — this uploads everything you drop on the Desktop. Big quota eater. |
| Contacts, Calendars, Reminders, Notes, Safari | Mail (only if you use iCloud Mail) |
| Messages in iCloud | Anything you don't use on this Mac (Home, Stocks, News, Freeform, Journal) |
| **iCloud Drive → Sync this Mac** — on, for small shared files | |

**iCloud Drive options** — `System Settings > [your name] > iCloud > Drive`:
- **Sync this Mac:** On.
- **Desktop & Documents Folders:** **Off.**
- **Optimize Mac Storage:** **On** — older files are evicted to the cloud when local space is tight.

> 📌 **Keep an eye on the quota.** `System Settings > [your name] > iCloud` shows the storage bar. iCloud **Backup** of iPhones and iPads is usually what eats the quota, not the Mac. Also note: in macOS 27, **Shared Albums now store full-resolution media and count against your iCloud storage** — a new way to lose space you didn't use to lose.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Passwords, notes, calendars stay in sync across all your devices | Files saved to Desktop/Documents stay local only — back them up via Time Machine |
| No silent upload of Desktop clutter burning your quota | Photos not on the Mac means AirDrop/iCloud.com to grab one |
| Optimize Mac Storage keeps the SSD from filling with iCloud Drive files | Optimized files need a network to open if they've been evicted |

---

## 6. Phase 5: Apple Intelligence & Siri Decision

Estimated time: 5 minutes to decide; model downloads happen in the background if you opt in.

### 6.1 What changed in macOS 27

- **Siri AI** replaces the old Siri. It's merged into Spotlight as a single **Search or Ask** bar (`Command + Space`), keeps conversations in a new **Siri app** synced via iCloud, and adds **Visual Intelligence** on Mac (`Shift + Command + Space`) and **Write with Siri** system-wide.
- The old global Apple Intelligence toggle is gone. The control is now **`System Settings > Siri`** with **Turn Off Siri**, and a fallback option **Use Siri Classic** (the pre-AI Siri).
- On macOS **Tahoe 26**, the pane is **`System Settings > Apple Intelligence & Siri`** with a master Apple Intelligence toggle.
- Siri AI is rolling out via a **waitlist**, English first, and isn't available in every region/language.
- Improved dictation and custom voices require **M3 or later with 12GB+ RAM**. M1/M2 Macs get Siri AI without those two extras.

### 6.2 Privacy model — what's documented vs reported

| Claim | Status |
|---|---|
| Simple requests run **on-device** | Documented by Apple |
| Harder requests go to **Private Cloud Compute (PCC)**; Apple says the data isn't accessible to Apple or third parties and isn't retained | Documented by Apple (it's Apple's claim; PCC's design is publicly inspectable, which is more than most vendors offer) |
| Apple's next-gen foundation models are built on a **Google Gemini** base, running inside Apple's PCC with no data going to Google | **Reported** (widely, not detailed in Apple's own consumer docs). Treat as likely, not verified. |
| **ChatGPT** is an optional, separate extension that sends requests to OpenAI when you explicitly choose it | Documented; off unless you enable it |

### 6.3 Honest trade-offs for *this* machine

| ✅ Turn Siri AI on | ⚠️ Turn it off / Siri Classic |
|---|---|
| Search or Ask is genuinely better than old Spotlight at "find that PDF from the landlord" queries | Model downloads eat SSD. Apple hasn't published a 27 figure; community reports range from roughly 8GB to 20GB+ depending on features *(unverified — check your own Storage settings)* |
| Visual Intelligence and Write with Siri are useful on a travel admin machine | Background model downloads and indexing cost some battery in the first days *(inference)* |
| On-device + PCC is the most privacy-respecting mainstream AI stack right now | Some AI features (notification summaries, mail categories) add noise — you can kill those individually |
| Your iPhone/iPad Siri history syncs in the Siri app | Turning Siri off doesn't remove Spotlight — `Command + Space` still searches your Mac |

**Recommendation (inference):** For most people, **Siri AI on, with the noisy bits trimmed** (6.4) is the best balance. Choose **Siri Classic** if you're tight on storage (especially on a 256GB Mac) or want no AI features at all. Both are one click to reverse. Don't try to strip models out by switching regions or using MDM profiles — that's the "sketchy debloat" territory this guide avoids, and the region trick can break iPhone Mirroring.

### 6.4 If you keep Siri AI — trim the noise

- **Notification summaries:** `System Settings > Notifications > Summarize notifications` → off, or on only for chatty apps.
- **Mail:** Mail > Settings → turn off summaries / categories / smart-reply style matching if you don't want them.
- **Messages:** Messages > Settings → message summaries off.
- **Learn from apps:** `System Settings > Siri > App Access` → turn off **Learn from this application** for anything you don't want indexed for suggestions (banking apps, Terminal).
- **Siri history:** `System Settings > Siri` → delete Siri & dictation history when you want a clean slate *(verify on 27 — exact button label)*.
- **Audit:** `System Settings > Privacy & Security > Apple Intelligence Report` — exports a log of requests sent to Private Cloud Compute. Use it to confirm what's leaving the machine.

### 6.5 If you turn it off

`System Settings > Siri` → **Turn Off Siri** → confirm. Or **Turn On Siri → Use Siri Classic** to keep basic voice Siri without Siri AI. Check `System Settings > General > Storage` a day later to see whether space came back.

---

## 7. Phase 6: Notifications, Spotlight & Nag Cleanup

Estimated time: 10 minutes. Apple's nags are gentler than Microsoft's, but they exist — mostly as notification permission prompts, badges, and "suggestions".

### 7.1 Notifications

`System Settings > Notifications`:

| Setting | Value |
|---|---|
| Show previews | **When Unlocked** (travel: nothing readable on a locked screen) |
| Show Notifications: when display is sleeping | Off |
| Show Notifications: when screen is locked | **Off** |
| Show Notifications: when mirroring or sharing the display | **Notifications Off** (protects you during screen share and iPhone Mirroring demos) |
| Summarize notifications | Your call (Phase 5) |

Then go app by app in the list below: **Allow notifications → Off** for anything that shouldn't talk to you. Typical kills on a fresh Mac: **Tips**, **News**, **Stocks**, **Game Center**, **Home**, **Books**, **Podcasts**, **TV** (Apple TV+ promos land here), **Freeform**, **Journal**.

> 💡 **iPhone notifications on the Mac.** macOS can mirror notifications from your iPhone (a Continuity feature). If you get double pings, look in `System Settings > Notifications` for **Allow notifications from iPhone** and either turn it off or pare down which iPhone apps can forward *(verify on 27 — label)*.

### 7.2 Spotlight

`System Settings > Spotlight`:
- Under **Results from Apps / Results from System / Results from Clipboard**, turn off categories you don't want surfacing (e.g. **Siri Suggestions / Suggestions**, **Websites**, **Movies & TV**, **Music**, **Tips**) *(exact category names vary; turn off anything that isn't local data you care about)*.
- **Help Apple Improve Search** → **Off**. This stops sharing search/Look Up query data with Apple.
- **Clear Spotlight history** when you want a clean slate.
- **Search Privacy / exclusions:** add folders you never want indexed (e.g. a scratch folder full of node_modules) *(verify on 27 — button label)*.

> 📌 **In macOS 27 Spotlight and Siri share one bar.** Turning Siri off in Phase 5 reverts `Command + Space` to classic Spotlight behavior. Turning off Spotlight categories doesn't touch Siri's knowledge; it only affects what the search results list shows.

### 7.3 Safari suggestions

Even if you use another browser day to day, Safari hosts your web apps (Phase 12) and opens links from some system surfaces. **Safari > Settings > Search**:
- **Include search engine suggestions** → Off
- **Include Safari Suggestions** → Off
- **Preload Top Hit in the background** → Off (saves a little bandwidth/battery)

*(verify on 27 — these labels have been stable for years.)*

**Safari > Settings > Privacy**: confirm **Prevent cross-site tracking** on and **Hide IP address** → *from Trackers and Websites* (the Websites option uses iCloud Private Relay, which needs a paid iCloud+ plan).

### 7.4 Dock, menu bar & desktop nags

- `System Settings > Desktop & Dock` → **Show suggested and recent apps in Dock → Off**.
- `System Settings > Desktop & Dock` → **Click wallpaper to reveal desktop → Only in Stage Manager** (stops the "where did my windows go" surprise) *(verify on 27)*.
- `System Settings > Menu Bar` (new top-level pane since Tahoe) → hide items you don't use; since Tahoe, rarely-used items can live in Control Center instead of the menu bar.
- `System Settings > Appearance` → **Liquid Glass** slider toward *tinted* if the transparency hurts legibility (new in 27).

### 7.5 Store & ads nags

- `System Settings > Privacy & Security > Apple Advertising` → **Personalized Ads → Off**. Affects App Store, Apple News, and Stocks ads.
- **App Store > Settings** → **In-App Ratings & Reviews → Off** (stops apps from prompting "Enjoying this app?") and **Video Autoplay → Off** *(verify on 27)*.
- **Music > Settings** — if you don't subscribe to Apple Music, turn off **Show Apple Music** to hide the store/subscription surfaces *(verify on 27)*. If you do subscribe, just trim notifications.

> 🔒 **Personalized Ads note.** Apple doesn't use a cross-app advertising ID like Windows' — this toggle controls whether Apple's own ad platform uses your account info (e.g. App Store downloads) to target ads in Apple's apps. Off has zero functional cost.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Lock screen shows nothing readable; screen shares don't leak notifications | You'll miss notifications while the Mac is locked — your iPhone and Watch cover that |
| Spotlight shows your files, not web/media suggestions | You lose "type a movie name, get showtimes" style results |
| No more Dock clutter or review prompts | Some settings drift back after major OS upgrades — recheck after 28.0 |

---

## 8. Phase 7: Remove Unnecessary Apps

Estimated time: 5 minutes. macOS ships far less junk than an OEM Windows install, and the core system apps (Stocks, News, Chess, Tips, etc.) live on the **sealed system volume** — they **cannot** be deleted without breaking the seal, and you shouldn't try. What you *can* remove are the App Store–distributed apps Apple preinstalls.

`Finder > Applications`, sort by **Size**. Remove by dragging to Trash (or right-click → **Move to Trash**).

| Generally safe to remove | Keep these |
|---|---|
| **GarageBand** — large; also delete its sound library if prompted *(inference: several GB)* | Safari — hosts your web apps and is the fallback for anything your default browser handles badly |
| **iMovie** — heavy video editing belongs on the iPad/desktop | Passwords — your password/passkey manager |
| **Keynote / Numbers / Pages** — if you live in Google Docs/Office; free to reinstall from the App Store any time | Find My, Shortcuts, Preview, Screenshot, Terminal, Disk Utility, System Information |
| Any trial or third-party app you installed and didn't keep | Music, Messages, FaceTime, iPhone Mirroring |

> 💡 **Hide what you can't delete.** For system apps you'll never use: right-click the Dock icon → **Options → Remove from Dock**, and turn their notifications off (Phase 6). That's the full extent of what's safe for sealed apps.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Recovers several GB on a 512GB drive | Reinstalling iWork/iMovie/GarageBand is a re-download from the App Store |
| Nothing removed here affects the OS, updates, or reset | Deleting a sealed system app isn't possible and attempts (via scripts) break updates — not covered here on purpose |

---

## 9. Phase 8: Login Items & Background Items

Estimated time: 5 minutes now, 2 minutes every time you install something new.

`System Settings > General > Login Items & Extensions`. Two things live here:

1. **Open at Login** — apps that launch when you log in. Remove with the **−** button.
2. **App Background Activity** (older builds: **Allow in the Background**) — helpers, updaters, and launch agents that run even when the app isn't open. Toggle off per item.
3. **Extensions** — Finder, Share, Spotlight, etc. extensions by category. Turn off anything you don't use.

| Safe to turn off in the background | Leave on |
|---|---|
| Browser updaters you don't rely on (most browsers update themselves when opened) *(inference)* | Anything from **Apple** that's listed |
| Microsoft AutoUpdate, Adobe/Creative Cloud helpers, Zoom helpers — if installed | Your VPN client, if you use one on untrusted networks |
| Discord/Spotify-type helpers (if you use web apps instead — Phase 12) | Any backup or sync agent you actively depend on |
| Game launchers | |

> 💡 **New in macOS 27: the Dock shows background processes.** A light gray indicator beneath an app icon means the app is still running in the background after you quit it. **Right-click → Stop Running in Background.** It's the closest thing macOS has to Task Manager's "this is still eating resources" signal, and it's per-session and harmless.

> ℹ️ **Same idea as Windows startup apps.** Turning off a login or background item doesn't uninstall or break anything — the app still works when you open it; it just stops running at login or idling in the background. Some apps (VPNs, sync clients) will stop doing their job when closed.

For resource checks: **Activity Monitor** (Applications > Utilities) → **Energy** tab → sort by **12 hr Power**. That's the real "what's killing my battery" list.

---

## 10. Phase 9: Privacy & Security Permissions Audit

Estimated time: 10 minutes on a fresh Mac; faster each time you repeat it.

`System Settings > Privacy & Security`. Every item below is a per-app allow-list. On a fresh machine most will be empty — the point is to **know where they are** and re-audit after installing tools.

**High-value categories (check these first):**

| Category | What it grants | What should be in it |
|---|---|---|
| **Full Disk Access** | Read almost everything, including Mail and Messages databases | Terminal only if you need it; backup tools. Nothing else. |
| **Accessibility** | Control the Mac — keystrokes, clicks, window control | Only utilities you trust and use (window managers, etc.) |
| **Input Monitoring** | Read keystrokes system-wide | Almost nothing. Anything here that isn't a keyboard utility you installed → remove |
| **Screen & System Audio Recording** | Record your screen/audio | Video call apps / browser only while you use them |
| **Camera / Microphone** | As named | Your browser, FaceTime, call apps |
| **App Management** | Lets an app update or delete other apps | Updaters you trust (e.g. Homebrew's terminal, if you install it) |
| **Developer Tools** | Lets apps run unsigned dev binaries without Gatekeeper checks each launch | Terminal *only if* you do dev work that needs it; otherwise empty |
| **Local Network** | Discover/connect to devices on your LAN | Your browser (for casting), apps that talk to LAN devices; revoke otherwise |
| **Passkeys Access for Web Browsers** | Lets Chromium browsers (Brave, Chrome, Edge) use your iCloud Keychain passkeys | Your non-Safari browser, if you use Apple Passwords (Phase 12) |
| **Files & Folders** | Per-folder access (Desktop, Documents, Downloads, removable volumes) | Review after installing each new app |
| **Automation** | Lets one app script another | Usually empty |

**Location Services** — `Privacy & Security > Location Services`:
- Per-app: **your browser** → While Using, **Weather/Maps** → While Using, everything else → off unless it has a reason.
- **System Services → Details**: keep **Find My Mac**, **Setting time zone**, **Networking and wireless** on. Turn off **Location-based suggestions** / **Suggestions & Search**, **Significant Locations** (a history of places you frequent), and consider **Show location icon in menu bar when System Services request your location** → **On** so you can see when it's used *(verify on 27 — exact labels in System Services)*.

**Analytics & Improvements** — `Privacy & Security > Analytics & Improvements` → turn **everything off**: **Share Mac Analytics**, **Share with app developers**, and any Siri/dictation, assistive-voice, or iCloud analytics toggles shown. (The exact set varies by build; none are required.)

**Apple Advertising** — **Personalized Ads → Off** (if not done in Phase 6).

**Allow applications from** — leave at **App Store & Known Developers**. That's Gatekeeper; don't loosen it. When a legit notarized app is blocked the first time, approve it here with **Open Anyway** — that's the supported per-app path, not a bypass.

> 🔒 **Audit cadence.** Re-run this phase after installing any new app, and before each long trip. The categories that matter most on a stolen or compromised laptop are Full Disk Access, Accessibility, Input Monitoring, and Screen Recording — anything in those is trusted with basically everything.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| You know exactly which apps can read disk, keystrokes, or screen | Revoking a permission an app needs breaks that feature silently until you re-grant |
| Significant Locations off removes a location history you probably never look at | Location-based suggestions off = fewer "leave now for…" style prompts |
| Analytics off costs nothing functionally | Apple gets less crash data from your machine |

---

## 11. Phase 10: Battery Settings

Estimated time: 3 minutes. Skip to Charging if your Mac lives on a desk.

`System Settings > Battery`:

**Low Power Mode / Energy Mode** — set **On battery → Low Power** (or **Only on Battery** if your build shows a single Low Power Mode menu). On a fanless Air the cost is mild performance throttling under load — irrelevant for browsing/admin/light dev. Leave power adapter on **Automatic**. *(The control is labeled differently across builds and models: Apple's docs describe both a Low Power Mode menu and an Energy Mode menu with separate On battery / On power adapter choices. Use whichever your screen shows.)*

**Charging (ⓘ next to Charging):**

| Setting | Recommendation |
|---|---|
| **Optimized Battery Charging** | **On.** Learns your routine and holds at ~80% until you usually unplug. |
| **Charge Limit** (macOS Tahoe 26.4+, Apple silicon) | **Yes, this exists now.** Range 80–100% (80/85/90/95/100). For a *travel* laptop, leave at **100%** most of the time and let Optimized Charging handle it. Set **80%** during long stretches parked at a desk. |

> 📌 **Charge Limit trade-off, honestly.** A hard 80% limit costs you ~20% of your travel runtime every single time. Optimized Charging gets most of the longevity benefit without that cost because it tops off before you usually leave. Both have **Charge to Full Now** in the battery menu for the "I'm leaving in an hour" override. The Mac also occasionally charges to 100% on purpose to keep the percentage calibrated — that's documented, not a bug.

**Battery Health (ⓘ)** — shows **Maximum Capacity** and condition. Worth checking every few months, and before you sell.

**Options…** *(verify on 27 — the list varies by model)*:

| Option | Setting |
|---|---|
| Slightly dim the display on battery | **On** |
| Optimize video streaming while on battery | **On** |
| Wake for network access | **Only on Power Adapter** (or Never) |
| Prevent automatic sleeping on power adapter when the display is off | Off |

> 💡 **Cycle count without Terminal.** Hold **Option** → Apple menu → **System Information** → **Power** → *Cycle Count*.

**Optional (Terminal) — battery snapshot in one line.** Read-only; changes nothing, so there's nothing to undo.

```bash
system_profiler SPPowerDataType | grep -E "Cycle Count|Condition|Maximum Capacity"
```

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Low Power on battery = noticeably longer runtime on a fanless machine | Occasional sluggishness on heavy pages or builds — switch to Automatic temporarily |
| Charge Limit/Optimized Charging slow long-term capacity loss — helps resale value | Hard 80% limit shortens every trip; use it deliberately |
| Dim-on-battery and streaming optimization are free runtime | Slightly dimmer screen on battery |

---

## 12. Phase 11: Continuity Setup & Verification

Estimated time: 10–15 minutes. Do this with your other Apple devices next to you. Skip rows for devices you don't own.

### 12.1 Shared prerequisites (all features)

On **every** device: same Apple Account, **two-factor authentication** on, **Bluetooth** and **Wi-Fi** on, **Handoff** on.

| Device | Handoff location |
|---|---|
| Mac | `System Settings > General > AirDrop & Continuity` → **Allow Handoff between this Mac and your iCloud devices** |
| iPhone / iPad | `Settings > General > AirPlay & Continuity` → **Handoff** *(verify on your iOS/iPadOS version — older builds named this "AirPlay & Handoff")* |

### 12.2 Feature-by-feature setup and verification

| Feature | Setup | How to verify it works |
|---|---|---|
| **AirDrop** | Phase 3 (Contacts Only) | AirDrop a photo iPhone → Mac and a file Mac → iPad |
| **Handoff** | Above | Open a Safari page on iPhone → a Safari icon appears at the right end of the Mac's Dock |
| **Universal Clipboard** | Automatic with Handoff | Copy text on iPhone, `Command + V` on the Mac within a minute or two |
| **Sidecar** | `System Settings > Displays` → **Add Display** pop-up → your iPad → **Use as: Extended display**. Quick path: **Control Center > Screen Mirroring** → iPad | iPad shows the Mac desktop; in macOS 27 you can tap and scroll directly on it |
| **Universal Control** | `System Settings > Displays > Advanced` → **Allow your pointer and keyboard to move between any nearby Mac or iPad** → On. Also **Push through the edge of a display…** and **Automatically reconnect…** | Push the cursor off the screen edge toward the iPad; it should cross over. Drag a file across to test file drop |
| **Apple Watch unlock** | `System Settings > Touch ID & Password` → **Apple Watch** → toggle on next to your watch. Watch needs a passcode and must be on your wrist | Lock (`Control + Command + Q`), then press a key — should unlock with a Watch tap. Also approves admin prompts via double-click of the side button |
| **iPhone Mirroring** | Open the **iPhone Mirroring** app on the Mac → follow the prompts; approve on iPhone. Requires iOS 18+ with a passcode, iPhone **locked** and nearby | iPhone's home screen appears in a window on the Mac. In macOS 27 the window is resizable |
| **iPhone calls / SMS** | iPhone: `Settings > Apps > Phone > Calls on Other Devices` and `Messages > Text Message Forwarding` → allow the Mac *(verify on your iOS version)* | Call your iPhone from another phone; the Mac should ring |
| **Instant Hotspot** | Automatic | Mac Wi-Fi menu lists your iPhone under Personal Hotspot — join with no password |
| **Continuity Camera** | Automatic | In FaceTime's (or your browser's) camera picker, your iPhone appears as a camera |

> ⚠️ **Mutually exclusive combos (documented).** iPhone Mirroring won't run while the Mac is using **Sidecar**, **AirPlay**, or **sharing its internet connection**. Universal Control and Sidecar both want the iPad — the iPad is either a second display (Sidecar) or a second computer (Universal Control), not both at once *(inference from how they're presented; switch via Control Center)*.

> 🔒 **iPhone Mirroring logs access.** The iPhone keeps a record of which Mac mirrored it and when. If you ever lend this Mac out or suspect access, check the iPhone. Also: anyone at an unlocked Mac can use your mirrored iPhone — your **immediate** password-on-wake (Phase 3) matters more because of this.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| iPad, iPhone, and Watch act like one system with the Mac | Continuity uses Bluetooth LE beaconing constantly *(inference: small but nonzero idle battery cost)* |
| Watch unlock removes the friction of "immediately" password on wake | Everything collapses if Apple Account/2FA is misconfigured — see Appendix A.1 |
| Universal Control drag-and-drop is a legit file transfer path to the iPad | iPhone Mirroring unavailable in the EU |

---

## 13. Phase 12: Browser & Web Apps

Estimated time: 10 minutes.

### 13.1 A non-Safari default browser (Brave as the example)

Safari is a perfectly good default and the most battery-efficient browser on a Mac *(inference — widely reported, consistent with it using the system's WebKit and media stack)*. If you prefer a Chromium or Firefox browser, the steps are the same; Brave is used as the example here.

1. Download the browser from its official site only (e.g. **brave.com**).
2. Sign in to its sync, confirm extensions and bookmarks.
3. `System Settings > Desktop & Dock` → **Default web browser** → pick it.
4. **Brave-specific cleanup:** **Brave > Settings** → turn off **Brave Rewards**, **Brave News**, **Brave Wallet** (set "Default Ethereum/Solana wallet" to *None* and hide the icon), **Leo** (if you don't use it), and **Brave VPN** promos in the toolbar. *(Brave's menu layout shifts between releases — search "wallet", "leo", "rewards" in Brave's settings search box.)*

### 13.2 Passwords & passkeys with Apple Passwords

If you use Apple Passwords (iCloud Keychain) as your password manager and a non-Safari browser, three things to know. If you use a third-party manager (Bitwarden, 1Password, etc.), install its browser extension and app, set it as the provider in `System Settings > General > AutoFill & Passwords`, and turn off Passwords there to avoid two vaults offering to save.

- **Passkeys in Chromium browsers.** Chromium on macOS can use your **iCloud Keychain passkeys** through the macOS system passkey UI. Grant it: `System Settings > Privacy & Security > Passkeys Access for Web Browsers` → your browser on. Google documents this for Chrome; other Chromium browsers (Brave, Edge, Arc) generally inherit it *(inference — test once with a passkey login before relying on it on a trip)*. Firefox handles passkeys through the same system UI on recent versions *(verify)*.
- **Passwords in other browsers.** Apple publishes an **iCloud Passwords** extension for Chrome/Edge on macOS. Other Chromium browsers (Brave, Arc) aren't officially supported; it installs from the Chrome Web Store and commonly works, but that's unsupported *(inference)*. The fallback is fine: open the **Passwords** app, or use Safari for the logins where it matters.
- `System Settings > General > AutoFill & Passwords` → make sure **Passwords** is the enabled AutoFill provider and that your browser's own password manager is off (in Brave/Chrome: **Settings > Autofill and passwords > Offer to save passwords → Off**) so you don't end up with two vaults *(verify on 27 — pane name)*.

> ⚠️ **Passkeys here are synced, not device-bound.** If you're used to device-bound passkeys that never leave one machine (e.g. Windows Hello on a PC), note that Apple Passwords passkeys sync end-to-end encrypted across your Apple devices via iCloud Keychain (and with ADP on, Apple can't read them) — but they **will exist on every device signed into your Apple Account**. There's no device-bound-only option in Apple Passwords. If a specific account (banking) must stay device-bound, the realistic option is a hardware security key (FIDO2/YubiKey) for that account rather than a platform passkey *(inference — recommendation, not a documented Apple setting)*.

### 13.3 Web apps: the macOS answer to Edge/Chrome PWAs

If you've used Edge or Chrome PWAs on Windows to replace heavy desktop apps (Discord, Spotify, Slack), there's a direct equivalent — and if the goal is fewer background processes, **Safari web apps are the better choice on macOS**.

| | **Safari web app** (`Safari > File > Add to Dock`) | **Chromium PWA (Brave/Chrome/Edge "Install app")** |
|---|---|---|
| Runs as | Its own app in `~/Applications`, own Dock icon, own window | A Chromium "app shim" that opens a browser window |
| Needs the browser running? | **No** — independent of Safari *(inference from Apple's description + WebKit architecture)* | **Yes** — launching it starts/keeps the browser running *(inference from Chromium's macOS app-shim design)* |
| Cookies / logins | **Separate** from Safari — Apple documents it shares no history, cookies, or website data | Shares the browser profile and extensions (so your ad blocker applies) |
| Notifications | Yes, with a Dock badge, once you allow it inside the web app | Yes, through the browser |
| Settings | Open the web app → *app name* menu → **Settings** (name, URL, icon, privacy, extensions) | The browser's app settings |
| Remove | Drag it from `~/Applications` to Trash | Browser's apps page / right-click → uninstall |

**Recommendations (inference):**
- **Discord / Slack → Safari web app.** Avoids the Electron client's background helpers and doesn't keep your main browser alive. Trade-off: some features the native client has (e.g. richer screen share, noise suppression, push-to-talk while unfocused) may be limited or missing in a browser — if you do voice calls regularly, the native app may be worth its weight.
- **Spotify → either.** The web player works as a Safari web app; the native app has better offline downloads and media-key handling. Apple Music subscribers should just use the built-in **Music** app.
- **Anything else web-first** (Proton, Notion, dashboards, admin portals) → Safari web app per site, each with isolated cookies. That isolation is a small privacy win too.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Web apps cut Electron/native helper processes and login items | Safari web apps don't get your other browser's ad blocker — Safari content blockers are separate |
| Per-web-app cookie isolation | Some web clients are feature-reduced vs native |
| One password vault across all Apple devices | Passkeys sync everywhere your Apple Account is signed in |

---

## 14. Phase 13: Minimal Dev Setup

Estimated time: 10–15 minutes. **Skip this phase if you don't write code.** Target: `git` + SSH to GitHub for light projects, nothing more.

### 14.1 Xcode Command Line Tools (no full Xcode)

Terminal is unavoidable here — there's no GUI entry point. **Optional, reversible.**

```bash
xcode-select --install
```

A GUI dialog appears → **Install**. This gives you `git`, `clang`, `make`, etc. (~a few GB). You do **not** need the full Xcode app from the App Store (tens of GB).

**Undo:**
```bash
sudo rm -rf /Library/Developer/CommandLineTools
```

### 14.2 git identity

```bash
git config --global user.name "Your Name"
git config --global user.email "<your-id>+<username>@users.noreply.github.com"
git config --global init.defaultBranch main
```

Use your GitHub **noreply** address (GitHub > Settings > Emails) to keep your real email out of public commit history.

**Undo:** `git config --global --unset user.name` (same for the others), or delete `~/.gitconfig`.

### 14.3 SSH key for GitHub (per GitHub's macOS docs)

```bash
ssh-keygen -t ed25519 -C "<your noreply email>"
# Enter = default path; set a passphrase
eval "$(ssh-agent -s)"
```

Create/edit `~/.ssh/config` (TextEdit works: `open -e ~/.ssh/config` after `touch ~/.ssh/config`):

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Then:
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
pbcopy < ~/.ssh/id_ed25519.pub
```

Paste into **GitHub > Settings > SSH and GPG keys > New SSH key**. Test:
```bash
ssh -T git@github.com
```

`--apple-use-keychain` stores the key's passphrase in your **login keychain** (local to this Mac), so you type it once. Unlike your passkeys, this SSH key is **device-bound** — it never leaves the Mac. One key per machine; revoke it on GitHub when you trade the Mac in.

**Undo:** delete the key on GitHub; `rm ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub`; remove the `Host github.com` block from `~/.ssh/config`.

### 14.4 Homebrew — only if justified

**Default: skip it.** CLT gives you git and SSH, which is all this workload needs.

Install Homebrew only if you actually want a CLI tool Apple doesn't ship. The most likely trigger is **GitHub CLI (`gh`)**: GitHub's docs recommend `brew install gh`, and their macOS `.pkg` installers are currently **unsigned**, which means opening them requires a Gatekeeper override — the thing this guide doesn't do. So "I want `gh`" = "install Homebrew".

If you do:
- Install from **brew.sh** only (it also offers a `.pkg` installer on its GitHub releases). On Apple silicon it lives in `/opt/homebrew` — outside the system volume, so it doesn't interfere with updates or reset.
- Immediately: `brew analytics off` (Homebrew collects anonymized analytics by default).
- Keep it lean: `brew install gh` and stop. Don't turn it into a second App Store.
- **Undo:** Homebrew's official uninstall script (linked from docs.brew.sh), then `sudo rm -rf /opt/homebrew` if anything remains.

> 💡 **Editor.** Pick one, lightweight. TextEdit/nano for quick edits; one real editor if you want (VS Code, Zed, or Sublime). Don't install a full IDE unless the work calls for it.

---

## 15. Phase 14: iPad File Transfer Workflow

Estimated time: 10 minutes to set up. Skip if you don't have an iPad. If you offload files from the Mac to an iPad (or pull files from it), here's how each method behaves day to day.

| Method | Best for | Needs a network? | Notes |
|---|---|---|---|
| **AirDrop** | Ad-hoc files, screenshots, a folder of photos | **No** — peer-to-peer Wi-Fi + Bluetooth | Fastest to start; macOS 27 claims faster transfers. Your own devices always see each other under Contacts Only |
| **Universal Control drag-and-drop** | Moving a file while you're already using the iPad as a second screen/computer | Same as Continuity | Drag from Finder across the screen edge into Files on the iPad |
| **Files > Connect to Server (SMB)** | Browsing/pulling from a Mac folder at home without cables | **Yes** — same LAN | Requires File Sharing on the Mac (below). Blocked on hotel Wi-Fi with client isolation *(inference: very common)* |
| **Shuttle SSD** | Big batches, video, anything > a few GB | No | Plug into the iPad's USB-C port; shows in Files |

### 15.1 SMB: Mac as a file server for the iPad

On the **Mac**:
1. Make a dedicated folder, e.g. `~/Transfer`. Share **only that** — not your whole home folder.
2. `System Settings > General > Sharing > File Sharing` → **On** → **ⓘ** → **Shared Folders: +** → add `~/Transfer`. Remove any default shares you don't want.
3. **Options…** → **Share files and folders using SMB** → On. Leave any "Windows File Sharing" per-user password checkbox **off** — Apple notes those passwords may be stored less securely; it's only needed for Windows clients.
4. Note the address shown (e.g. `smb://mba-13.local`).

On the **iPad**:
1. **Files** → **⋯** (top of sidebar) → **Connect to Server**.
2. Enter `smb://mba-13.local` (or the Mac's IP).
3. **Registered User** → your Mac account name and password.

> 🔒 **Turn File Sharing off when you leave home.** It's a listening service on every network you join. The firewall's *Block all incoming connections* (Phase 3) also blocks it. Treat SMB as a home-network tool; use AirDrop or the SSD on the road.

> 💡 **Optional (Terminal) — stop `.DS_Store` litter on SMB shares and USB drives.** Finder writes hidden `.DS_Store` files into every folder it browses; on iPads and Windows PCs they show up as junk. Apple documents the network-store setting for SMB performance. Reversible:
> ```bash
> defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
> defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true
> ```
> Log out and back in. **Undo:**
> ```bash
> defaults delete com.apple.desktopservices DSDontWriteNetworkStores
> defaults delete com.apple.desktopservices DSDontWriteUSBStores
> ```
> *(The USB variant is widely used but not in Apple's SMB doc — inference that it behaves the same.)*

### 15.2 Shuttle SSD format

Use **separate drives** for Time Machine (Phase 15) and file shuttling if you can. If you only have one drive, Apple supports adding a second APFS volume to the Time Machine disk in Disk Utility — but the iPad can't read a Time Machine volume and a lost drive takes both, so separate drives are better. Format the shuttle in **Disk Utility** (`Applications > Utilities`) → **View > Show All Devices** → select the physical drive → **Erase**:

| Format | Mac | iPad (Files) | Windows PC | Encrypted |
|---|---|---|---|---|
| **APFS (Encrypted)** | ✅ | ✅ (iPadOS supports APFS encrypted — prompts for password) | ❌ | ✅ |
| **exFAT** | ✅ | ✅ | ✅ | ❌ |

**Recommendation (inference):** if the shuttle only goes Mac ↔ iPad → **APFS (Encrypted)**, since a drive that travels can be lost. If it also has to work with a Windows PC, use **exFAT** and accept that it's unencrypted — or keep a second small exFAT stick for Windows.

---

## 16. Phase 15: Final Checks & Clean Baseline Backup

Estimated time: 10 minutes plus the first backup.

**Confirm before finishing:**
- `Software Update` shows nothing pending
- FileVault **On**, Find My Mac **On**, firewall **On** + stealth
- Lock → Watch unlock works; lock → Touch ID works; restart → password required
- AirDrop, Handoff, Universal Clipboard, Sidecar, Universal Control, iPhone Mirroring each verified once (Phase 11 table)
- Default browser set, passkey login tested once
- `System Settings > General > Storage` — note the baseline used space (useful if AI models or caches balloon later)
- Recovery key stored in two places that don't travel with the laptop

### 16.1 Time Machine to the dedicated SSD

1. Plug in the Time Machine SSD.
2. `System Settings > General > Time Machine` → **Add Backup Disk** (or **+**) → select the drive → **Set Up Disk**.
3. Turn on **Encrypt Backup** → set a backup password (store it with the FileVault key, not on the laptop).
4. Allow it to erase the drive — Time Machine formats it **APFS**. Use this disk **only** for Time Machine; Apple explicitly says not to store other files on the backup volume.
5. Run the first backup now: Time Machine menu bar icon (or the settings pane) → **Back Up Now**. This is your **clean baseline**.
6. **Options…** → **Back up frequency**: **Automatically Every Hour** if you plug the drive in at a desk; **Manually** if the drive lives at home and you back up weekly.

> ℹ️ **Time Machine is your only "restore point."** macOS has no Windows-style system restore — the system volume is sealed and restored by reinstall. Time Machine covers your data, apps, and settings; if the OS itself misbehaves, you reinstall macOS from Recovery and restore from Time Machine with Migration Assistant.

> 💾 **Tip:** Keep the Time Machine SSD at home, not in the laptop bag. A stolen bag shouldn't take both the Mac and its backup.

| ✅ Benefits | ⚠️ Possible drawback |
|---|---|
| Encrypted, versioned backup of a known-good baseline | Only as current as your last plug-in — iCloud doesn't cover Desktop/Documents here (Phase 4) |
| Full-machine restore via Migration Assistant | Drive is dedicated; can't double as the shuttle |
| APFS snapshots make incremental backups fast | Encrypted backup password is one more secret to keep |

---

## 17. Phase 16: Erase & Reset for Trade-In

Do this before you sell, trade in, or give away the Mac. (For a repair, Apple's repair flow handles Find My differently — follow Apple's instructions for that instead of erasing.) Estimated time: 15 minutes.

1. **Final Time Machine backup.**
2. **Revoke the SSH key** on GitHub (Settings > SSH and GPG keys), if you set one up in Phase 13.
3. **Messages:** Messages > Settings > iMessage → **Sign Out**.
4. **Unpair Bluetooth accessories** you're keeping (Magic Keyboard/Mouse, etc.): `System Settings > Bluetooth` → **ⓘ** → **Forget This Device**. *(AirPods and Watch pairing is account-based and follows your account — nothing to do.)*
5. **Erase All Content and Settings:** `System Settings > General > Transfer or Reset > Erase All Content and Settings`. This securely erases data, settings, and apps while keeping the installed macOS, and signs you out of Apple services.
6. **Confirm it's off your account.** In the **Find My** app on another device (or iCloud.com/find) → **Devices** — the Mac should be gone. Also check **account.apple.com > Devices**; remove it if it's still listed. *(Apple's page says erase signs you out; that signing out removes Activation Lock is the expected consequence — verify it rather than assume it.)*
7. When the Mac restarts to the Setup Assistant "Hello" screen, **shut it down**. Don't set it up.

> ⚠️ **Activation Lock is what kills trade-in value.** If the Mac still shows in Find My after erasing, remove it from **iCloud.com/find** before handing it over. Trade-in programs will reject or re-price an Activation-Locked Mac.

---

## 18. Quick Reference Card

| Task | Where |
|---|---|
| Software Update / auto-update policy | `System Settings > General > Software Update` → ⓘ Automatic Updates |
| FileVault | `System Settings > Privacy & Security > FileVault` |
| Find My Mac | `System Settings > [your name] > iCloud > See All > Find My Mac` |
| Lock screen / password on wake | `System Settings > Lock Screen` |
| Firewall + stealth mode | `System Settings > Network > Firewall` → Options |
| AirDrop, Handoff, AirPlay Receiver | `System Settings > General > AirDrop & Continuity` |
| Sharing services, hostname, File Sharing | `System Settings > General > Sharing` |
| USB/Thunderbolt accessory approval | `System Settings > Privacy & Security > Allow accessories to connect` |
| Private Wi-Fi address | `System Settings > Wi-Fi` → Details |
| iCloud sync / iCloud Drive options | `System Settings > [your name] > iCloud` (→ Drive) |
| Siri AI / Siri Classic / turn off (macOS 27) | `System Settings > Siri` |
| Apple Intelligence (macOS 26) | `System Settings > Apple Intelligence & Siri` |
| AI request log | `System Settings > Privacy & Security > Apple Intelligence Report` |
| Notifications / previews / summaries | `System Settings > Notifications` |
| Spotlight categories & Help Apple Improve Search | `System Settings > Spotlight` |
| Safari suggestions | Safari > Settings > Search |
| Dock suggestions / default browser / hot corners | `System Settings > Desktop & Dock` |
| Menu bar items | `System Settings > Menu Bar` |
| Liquid Glass transparency | `System Settings > Appearance` |
| Login items / background activity / extensions | `System Settings > General > Login Items & Extensions` |
| Stop a background app (27) | Right-click Dock icon → Stop Running in Background |
| Permissions audit | `System Settings > Privacy & Security` |
| Analytics / Personalized Ads | `Privacy & Security > Analytics & Improvements` / `> Apple Advertising` |
| Browser passkey access (non-Safari) | `Privacy & Security > Passkeys Access for Web Browsers` |
| Low Power / Charge Limit / Battery Health | `System Settings > Battery` (ⓘ next to Charging / Battery Health) |
| Cycle count | Option + Apple menu → System Information → Power |
| Watch unlock | `System Settings > Touch ID & Password` → Apple Watch |
| Universal Control | `System Settings > Displays > Advanced` |
| Sidecar | `System Settings > Displays` → Add Display, or Control Center > Screen Mirroring |
| Safari web app | Safari > File > Add to Dock |
| Time Machine | `System Settings > General > Time Machine` |
| Storage overview | `System Settings > General > Storage` |
| Erase for trade-in | `System Settings > General > Transfer or Reset > Erase All Content and Settings` |

---

## 19. Appendix: Troubleshooting

### A.1 A Continuity feature doesn't work (Handoff, Universal Control, Sidecar, Clipboard)

Check one variable at a time, in this order, on **both** devices:
1. Same Apple Account (`System Settings > [your name]` on Mac; `Settings > [your name]` on iPhone/iPad).
2. Bluetooth and Wi-Fi both on (Wi-Fi on even if you're wired or on hotspot).
3. Handoff on (Phase 11 → section 12.1 table).
4. Within ~10 m / 30 ft.
5. Mac isn't sharing its internet connection; iPad isn't sharing cellular (blocks Universal Control).
6. Software updates on both, then restart both.

Only after all six pass: sign out and back into iCloud on the misbehaving device.

### A.2 iPhone Mirroring won't start

- The iPhone must be **locked** and nearby, running iOS 18+ with a passcode.
- Sidecar, AirPlay, or Internet Sharing active on the Mac blocks it — disconnect the iPad from Sidecar first.
- Region: unavailable in the EU. If you changed region settings on the Mac, iPhone, or Apple Account, set them back to your real country.

### A.3 Apple Watch unlock toggle is missing or greyed out

Needs: same Apple Account with 2FA, Wi-Fi + Bluetooth on the Mac, a **passcode on the Watch**, and wrist detection on. After a restart, the first unlock always requires the password — that's by design, not a fault.

### A.4 My non-Safari browser doesn't offer my iCloud passkeys

`System Settings > Privacy & Security > Passkeys Access for Web Browsers` → make sure your browser is on. Restart it. If it still doesn't show, use Safari for that login — Chromium-based browsers' support for the macOS iCloud Keychain passkey bridge follows upstream Chromium *(inference)*.

### A.5 Storage dropped by 10–20GB overnight after setup

Almost certainly Siri AI / Apple Intelligence model downloads (and Spotlight re-indexing after the 27 upgrade). Check `System Settings > General > Storage`. If you don't want Siri AI, turn it off (Phase 5 → section 6.5) and give it a day. Don't delete files from `/System` or asset folders by hand — that's what breaks updates.

### A.6 iPad can't connect to the Mac over SMB

1. File Sharing is on and `~/Transfer` is listed under Shared Folders.
2. Firewall **Block all incoming connections** is off.
3. Both are on the same network — hotel/café Wi-Fi often isolates clients; use AirDrop or the SSD.
4. Try the Mac's IP address instead of `.local` (shown in `System Settings > Wi-Fi > Details`).

### A.7 The Mac stopped charging at 80% (or 85/90/95)

That's **Charge Limit** or **Optimized Battery Charging**. Click the battery icon in the menu bar → **Charge to Full Now**, or change the limit in `System Settings > Battery` → ⓘ next to Charging.

### A.8 A USB-C device or dock "doesn't work"

On Apple silicon laptops, new accessories must be approved on an **unlocked** Mac. Unlock, replug, click **Allow**. If you set **Always ask**, you'll see this every time — that's intentional.

### A.9 An app says it's damaged or can't be opened

If it's from a known developer and you downloaded it from their official site: `System Settings > Privacy & Security` → scroll to Security → **Open Anyway** (appears for an hour after a blocked launch). That's the supported per-app approval. If an app wants you to disable Gatekeeper wholesale or run `xattr`/`spctl` commands, don't use that app on this machine.

### A.10 Notifications stopped for an app I care about

Check `System Settings > Notifications > [app]` and whether the app is a Safari web app — web app notification permission is granted **inside the web app**, not in Safari. Also check whether you turned off its background activity in Phase 8.

---

## 20. Appendix: Sources

Apple:
- [Apple introduces the new MacBook Air with M5 (Newsroom, Mar 2026)](https://www.apple.com/newsroom/2026/03/apple-introduces-the-new-macbook-air-with-m5/)
- [macOS 27 Golden Gate compatible computers](https://support.apple.com/en-us/127255)
- [What's new for enterprise in macOS Golden Gate 27](https://support.apple.com/en-us/148830)
- [Set up your MacBook Air (Setup Assistant)](https://support.apple.com/guide/macbook-air/set-up-your-mac-apd831707cb3/mac)
- [Get macOS updates](https://support.apple.com/guide/mac-help/get-macos-updates-mchlpx1065/mac)
- [Protect data with FileVault](https://support.apple.com/guide/mac-help/protect-data-on-your-mac-with-filevault-mh11785/mac)
- [Set up Find My on Mac](https://support.apple.com/guide/findmy-mac/set-up-fmm53101237/mac)
- [Lock Screen settings](https://support.apple.com/guide/mac-help/change-lock-screen-settings-mh11784/mac)
- [Firewall](https://support.apple.com/guide/mac-help/block-connections-to-your-mac-with-a-firewall-mh34041/mac)
- [AirDrop & Continuity settings](https://support.apple.com/guide/mac-help/change-airdrop-continuity-settings-mchl6a407f99/mac)
- [Hand off tasks between devices](https://support.apple.com/guide/mac-help/hand-off-tasks-between-devices-mchl732d3c0a/mac)
- [Allow accessories to connect](https://support.apple.com/en-us/102282)
- [Private Wi-Fi address](https://support.apple.com/en-us/102509)
- [Store files in iCloud Drive](https://support.apple.com/guide/mac-help/store-files-in-icloud-drive-mchle5a61431/mac)
- [Turn off and restrict Apple Intelligence features on Mac](https://support.apple.com/guide/mac-help/turn-restrict-access-apple-intelligence-mchlb2e44f94/mac)
- [Notifications settings](https://support.apple.com/guide/mac-help/change-notifications-settings-mh40583/mac)
- [Spotlight settings](https://support.apple.com/guide/mac-help/mchl3e00eae9/mac)
- [Login Items & Extensions settings](https://support.apple.com/guide/mac-help/change-login-items-extensions-settings-mtusr003/mac)
- [Privacy & Security settings](https://support.apple.com/guide/mac-help/change-privacy-security-settings-on-mac-mchl211c911f/mac)
- [Analytics & Improvements](https://support.apple.com/guide/mac-help/mh27990/mac)
- [Location Services](https://support.apple.com/guide/mac-help/control-access-to-location-services-mh35873/mac)
- [Battery settings](https://support.apple.com/guide/mac-help/change-battery-settings-mchlfc3b7879/mac)
- [Optimized Battery Charging and Charge Limit](https://support.apple.com/en-au/102338)
- [Low Power Mode on Mac](https://support.apple.com/en-us/101613)
- [Continuity requirements](https://support.apple.com/en-us/108046)
- [Universal Control](https://support.apple.com/en-us/102459)
- [iPhone Mirroring](https://support.apple.com/en-us/120421)
- [Unlock your Mac with Apple Watch](https://support.apple.com/guide/mac-help/unlock-your-mac-with-apple-watch-mchl4f800a42/mac)
- [Sidecar](https://support.apple.com/guide/mac-help/use-your-ipad-as-a-second-display-mchlf3c6f7ae/mac)
- [Safari web apps](https://support.apple.com/en-us/104996)
- [File Sharing / SMB](https://support.apple.com/guide/mac-help/share-mac-files-with-windows-users-mchlp1657/mac)
- [iPad: transfer files to storage devices and servers](https://support.apple.com/guide/ipad/ipad8139864c/ipados)
- [Back up with Time Machine](https://support.apple.com/en-us/104984)
- [Disks you can use with Time Machine](https://support.apple.com/guide/mac-help/disks-you-can-use-with-time-machine-mh15139/mac)
- [Erase All Content and Settings](https://support.apple.com/en-us/102664)
- [What to do before selling or trading in your Mac](https://support.apple.com/en-us/102773)

Vendor docs:
- [GitHub: Generating a new SSH key (macOS)](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [GitHub CLI: macOS install](https://github.com/cli/cli/blob/trunk/docs/install_macos.md)
- [Homebrew](https://brew.sh)
- [Apple: Adjust SMB browsing behavior (.DS_Store on network shares)](https://support.apple.com/en-us/102064)
- [Chrome support for passkeys in iCloud Keychain (ImperialViolet)](https://www.imperialviolet.org/2023/10/18/icloudkeychain.html)

Reporting (release facts & feature details):
- [MacRumors: macOS Golden Gate now available (Sept 14, 2026)](https://www.macrumors.com/2026/09/14/apple-releases-macos-golden-gate/)
- [9to5Mac: macOS 27.0.1 and Tahoe 26.7.1 (Sept 28, 2026)](https://9to5mac.com/2026/09/28/apple-releases-macos-27-0-1-golden-gate-plus-macos-tahoe-26-7-1-and-macos-sequoia-15-8-1/)
- [MacRumors: 50 new macOS Golden Gate features](https://www.macrumors.com/2026/09/17/new-features-changes-macos-27/)
- [MacRumors: macOS 27 roundup](https://www.macrumors.com/roundup/macos-27/)
- [AppleInsider: Golden Gate review — Siri AI](https://appleinsider.com/articles/26/09/14/macos-golden-gate-is-here-and-siri-ai-is-its-remarkable-best)
- [MacMost: combined Spotlight and Siri AI menu](https://macmost.com/how-to-use-the-combined-spotlight-and-siri-ai-menu-in-macos-golden-gate.html)
- [iDownloadBlog: Charge Limit in macOS Tahoe](https://www.idownloadblog.com/2026/03/26/battery-charge-limit-macbook/)
- [iMore: encrypted drive support in iPadOS Files](https://www.imore.com/apple-adds-encrypted-drive-support-files-ios-14-and-ipados-14)

---

*macOS Setup, Deliberately · Apple silicon Mac laptops · macOS 27 Golden Gate · GUI-first*
