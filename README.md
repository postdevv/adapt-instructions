========================================================
 ADAPT - USER GUIDE
========================================================

+------------------------------------------------------+
| NOTE: Make sure to follow all steps and do not skip  |
| any steps. Every step in this guide is strictly      |
| mandatory for a clean unban.                        |
+------------------------------------------------------+

--------------------------------------------------------
 SUPPORTED GAMES & STATUS
--------------------------------------------------------
 Game Title                   Anti-Cheat           Status
--------------------------------------------------------
 Fortnite                     BE + EasyAntiCheat   UNDETECTED
 Apex Legends                 EasyAntiCheat        UNDETECTED
 Rust                         EAC (Cerberus)       UNDETECTED
 Rainbow Six Siege            BattlEye             UNDETECTED
 Call of Duty (WZ / BO6)      Ricochet             UNDETECTED
 FiveM                        FiveM AC / servers   SERVER DEPENDENT
 Valorant                     Riot Vanguard        WORK IN PROGRESS
--------------------------------------------------------


========================================================
 SPOOFER SETUP [DO THIS BEFORE SPOOF]
 Windows Reinstall (Mandatory)
========================================================

A fresh Windows re-installation with a clean drive format is
mandatory to ensure 100% unban success. Modern anti-cheats
(EasyAntiCheat, BattlEye, Vanguard, Ricochet) embed permanent
telemetry traces, registry identifiers and volume tracker
markers across your system drive.

+------------------------------------------------------+
| MANDATORY REQUIREMENT: Do NOT use Windows "Reset This |
| PC" (built-in reset leaves tracking logs behind). You |
| must perform a clean reinstall using an official USB  |
| Flash Drive (8 GB+).                                  |
+------------------------------------------------------+

STEP-BY-STEP QUICK REINSTALL GUIDE

 1. CREATE BOOTABLE WINDOWS USB
    Prepare an empty USB flash drive (minimum 8 GB):
    - Download the official Microsoft Media Creation Tool
      (Windows 10 or Windows 11).
    - Run the tool, select "Create installation media
      (USB flash drive)".
    - Select your USB drive and wait for the process to
      complete (5-10 minutes).

 2. BOOT FROM USB & FORMAT DRIVE

    Reboot your PC and tap your motherboard's boot menu
    key immediately:

      Motherboard      Boot Menu Key
      ---------------- -------------
      ASUS             F8
      MSI / ASRock     F11
      Gigabyte         F12

    - Select your USB drive (prefixed with UEFI:).
    - Click Install Now > Select Custom: Install Windows
      only (advanced).
    - Select each partition of your primary Windows drive
      (Drive 0) and click Delete until only Unallocated
      Space remains.
    - Select the Unallocated Space and click Next.
      Windows will install fresh.

 3. POST-INSTALLATION RULES
    +--------------------------------------------------+
    | DO NOT INSTALL OR OPEN GAMES YET: Complete the    |
    | initial Windows setup. Do not link any old banned |
    | Microsoft/Epic/Riot accounts. Proceed directly to |
    | the next steps (BIOS Configurations & Spoofing)   |
    | first.                                            |
    +--------------------------------------------------+


========================================================
 SPOOFER SETUP [DO THIS BEFORE SPOOF]
 BIOS Configurations (Secure Boot & TPM)
========================================================

Before spoofing, you must configure your motherboard BIOS
to permit kernel driver communication and prevent
anti-cheats from reading banned hardware certificates.

 1. DISABLE SECURE BOOT (MANDATORY)

Adapt requires direct low-level kernel access to rewrite
your SMBIOS tables and serials. If Secure Boot is on, the
Windows kernel blocks all unsigned hardware flasher drivers.

  1.1 ENTER MOTHERBOARD BIOS
      Restart your PC and repeatedly tap DEL or F2 as soon
      as the screen turns on. Press F7 if needed to enter
      Advanced Mode.

  1.2 DISABLE SECURE BOOT
      - ASUS: Boot > Secure Boot > Change OS Type from
        Windows UEFI to Other OS (or Key Management >
        Clear Secure Boot Keys).
      - MSI: Settings > Advanced > Windows OS
        Configuration > Secure Boot > Disabled.
      - Gigabyte / AORUS: BIOS tab > Secure Boot > Disabled.
      - ASRock: Security > Secure Boot > Disabled.

 2. DISABLE TPM / fTPM (IF TPM BANNED)

    +--------------------------------------------------+
    | TPM BAN NOTICE: If you are hardware banned on TPM  |
    | (e.g. Valorant / Vanguard VAN 152 / VAN 1067) and |
    | you are NOT using the TPM Clean feature, you must |
    | disable TPM so anti-cheats cannot read your banned |
    | endorsement key!                                  |
    +--------------------------------------------------+

    AMD motherboards (AMD fTPM):
    Navigate to Advanced > CPU Configuration or AMD fTPM
    Configuration > Set AMD fTPM switch (or TPM Device
    Selection) to Disabled.

    Intel motherboards (Intel PTT):
    Navigate to Advanced / Security > PCH-FW Configuration
    or Trusted Computing > Set Intel PTT (Platform Trust
    Technology) to Disabled.

 3. DISABLE FAST STARTUP

Fast Startup causes Windows to save a hibernation file of
old SMBIOS hardware tables instead of polling newly
spoofed tables on reboot.

  3.1 OPEN POWER OPTIONS
      Press Win + R, type powercfg.cpl and press Enter.
  3.2 CHANGE SETTINGS
      Click "Choose what the power buttons do" on the left
      sidebar.
  3.3 UNCHECK FAST STARTUP
      Click "Change settings that are currently unavailable",
      then uncheck "Turn on fast startup (recommended)" and
      click Save changes.


========================================================
 TPM & SECURITY ARCHITECTURE
 TPM 2.0 & Bypass Reality
========================================================

A comprehensive technical breakdown of how Trusted Platform
Modules (TPM 2.0) work, how anti-cheats (like Riot Vanguard)
enforce hardware attestation, and the technical reality
behind software "TPM Spoofers".

+------------------------------------------------------+
| CORE CONCEPT: A TPM is a cryptographic security        |
| processor (integrated into your CPU as fTPM / Intel    |
| PTT, or a standalone dTPM module). It contains an      |
| immutable, factory-burned Endorsement Key (EK) that    |
| cannot be permanently rewritten by any software or     |
| BIOS flasher.                                          |
+------------------------------------------------------+

HOW ANTI-CHEATS USE TPM (HARDWARE ATTESTATION)
Modern anti-cheats (especially Riot Vanguard on Windows 11)
don't just read serial numbers; they use TPM 2.0 for
cryptographic Remote Attestation:

 1. THE ENDORSEMENT KEY (EK) ROOT OF TRUST
    Every TPM chip has a unique Endorsement Key pair burned
    at the factory. The public key is signed by a
    certificate issued directly by the manufacturer (AMD,
    Intel, Infineon, Nuvoton). The private key never leaves
    the cryptographic silicon.

 2. HARDWARE BINDING BANS
    When an anti-cheat flags or bans your machine, it binds
    the penalty cryptographically to your TPM's Endorsement
    Key certificate. The anti-cheat sends a cryptographic
    nonce (challenge) that only your genuine TPM private key
    can sign.

 3. WINDOWS 10 VS WINDOWS 11 ENFORCEMENT
    - Windows 11: Mandates TPM 2.0 and Secure Boot at the
      OS level. Vanguard immediately blocks launch with
      error VAN 1067 or VAN 9003 if TPM is disabled.
    - Windows 10: Vanguard does not strictly require TPM 2.0
      at the kernel level on Windows 10! Disabling
      fTPM/PTT in BIOS is a completely viable workaround to
      play without a TPM ban on Windows 10.

THE TRUTH ABOUT SOFTWARE "TPM SPOOFERS"
Almost all software tools marketed as "TPM Spoofers" fall
into two technical categories:

 CATEGORY 1: FAKE "TPM SPOOFERS" (Registry / WMI / Clear-Tpm)
 - They edit cosmetic registry keys (e.g. HKLM:\SOFTWARE\
   Microsoft\Tpm\TpmInstanceId) or flush cached NGC
   credentials (AppData\Local\Microsoft\NGC).
 - They intercept basic user-mode WMI queries or modify
   SMBIOS tables in memory.
 - They automate a simple Clear-Tpm command in PowerShell
   or run a BIOS NVRAM wipe.
 Why they fail: the moment a kernel-level anti-cheat driver
 directly queries the official Windows TPM driver (tpm.sys)
 or issues a hardware challenge, these superficial
 modifications are bypassed and your original hardware
 Endorsement Key is retrieved.

 CATEGORY 2: DRIVER-BASED SPOOFERS (Ring 0 / Hypervisor)
 - Memory Interception: they load an unsigned driver in
   kernel mode (Ring 0) to hook IRP dispatch routines of
   tpm.sys and alter responses on the fly.
 - BYOVD Loading: because Windows blocks unsigned kernel
   drivers, they exploit known vulnerabilities in legitimate
   signed drivers (Bring Your Own Vulnerable Driver) to
   inject their hook into kernel memory.
 - Volatile & Temporary: these hooks only exist in volatile
   RAM. Upon restarting your computer, all changes disappear
   because the physical silicon was never altered.

+------------------------------------------------------+
| SUMMARY: Purely software-based "permanent TPM spoofing" |
| without changing your processor or installing an        |
| external hardware crypto-module does not exist. It is   |
| either surface-level registry cosmetics or temporary    |
| RAM interception.                                       |
+------------------------------------------------------+

PERMANENT & VERIFIED SOLUTIONS FOR TPM BANS

 METHOD A: WINDOWS 10 + DISABLE fTPM IN BIOS
   (Free & 100% working)
   - AMD: BIOS > Advanced > AMD fTPM Configuration > AMD
     fTPM switch = Disabled.
   - Intel: BIOS > Advanced / Security > PCH-FW
     Configuration > Intel PTT = Disabled.
   - Install Windows 10 (21H2 or 22H2). Vanguard will
     launch cleanly without requesting TPM attestation.

 METHOD B: DISCRETE TPM 2.0 MODULE (~12-15 EUR)
   (Ultimate permanent hardware fix for Windows 11)
   - Most desktop motherboards (ASUS, MSI, Gigabyte, ASRock)
     feature an onboard TPM Header (12-pin, 14-pin or
     20-pin).
   - Buy a dedicated TPM 2.0 module matching your
     motherboard (Amazon / AliExpress).
   - Plug the physical module into your motherboard's TPM
     header.
   - BIOS > Trusted Computing > switch TPM Device Selection
     from Firmware TPM (fTPM) to Discrete TPM (dTPM).
   - Result: Windows and anti-cheats communicate with the
     new external crypto-chip, which has a brand new,
     legitimate, factory-certified Endorsement Key. Your
     banned CPU fTPM is bypassed forever while keeping full
     Windows 11 compatibility!

 METHOD C: CLEAN TPM PROVISIONING & NVRAM RESET
   (For corrupted attestation certificates / cached tokens)
   - Open PowerShell as Administrator, run: Clear-Tpm
   - In BIOS, select Reset / Clear fTPM NVRAM.
   - Run Adapt (Adapt.bat as Administrator) to flush
     registry identifiers, hardware serials and NGC token
     caches (use the TPM Clean action).

DIAGNOSTIC & VERIFICATION COMMANDS

 1. Check TPM status (PowerShell Admin):
       Get-Tpm
    Displays TPM Presence, Ready State (TpmReady: True)
    and Manufacturer ID.

 2. Check TPM via console: Win + R > tpm.msc
    Verify "The TPM is ready for use" and note the
    Specification Version (2.0).

 3. Inspect Endorsement Key (PowerShell Admin):
       Get-TpmEndorsementKeyInfo


========================================================
 SPOOFER DOWNLOAD
 Disable Windows Defender
========================================================

You must temporarily disable Windows Defender Real-Time
Protection before downloading or running Adapt.

+------------------------------------------------------+
| MANDATORY STEP: Anti-cheats and Windows Defender flag  |
| kernel motherboard flasher drivers (AMI flasher /      |
| kernel memory injectors) as false-positives and block  |
| them from running.                                     |
+------------------------------------------------------+

HOW TO DISABLE REAL-TIME PROTECTION:
 1. Open Windows Security from the Start menu.
 2. Click Virus & threat protection > Manage settings.
 3. Toggle Real-time protection to OFF.


========================================================
 SPOOFER DOWNLOAD
 ADAPT LOADER
========================================================

Extract the Adapt package anywhere (e.g. your Desktop).

+------------------------------------------------------+
| SINGLE FOLDER PACKAGE: The package contains adapt.py,  |
| Adapt.bat, Adapt GUI.bat and the bin\ folder with the  |
| AMI flasher tool and signed drivers. Everything is     |
| included - no extra downloads required.                |
+------------------------------------------------------+

HOW TO LAUNCH:
 1. MOVE TO DESKTOP
    Place the Adapt folder directly on your Windows
    Desktop.
 2. RUN AS ADMINISTRATOR
    Double-click Adapt.bat (runs the full spoof directly)
    or Adapt GUI.bat (opens the modern GUI). Both request
    administrator rights automatically - click Yes on the
    UAC prompt.
 3. NO LOGIN REQUIRED
    Adapt has no license key and no account system. It
    starts working immediately.


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Spoofing
========================================================

Once Adapt is running you will see your current system
specifications and the spoofer actions.

SELECT YOUR SPOOF MODE:

 +-------------------+-----------------------------------+
 | FULL SPOOF        | Recommended for 95% of            |
 | (Standard AMI)    | motherboards: ASUS, Gigabyte,     |
 |                   | MSI, ASRock. Flashes all SMBIOS   |
 |                   | tables permanently. Board brand   |
 |                   | is detected automatically (AUTO). |
 +-------------------+-----------------------------------+
 | TPM CLEAN         | Executes a complete TPM 2.0       |
 |                   | trace reset. Mandatory if banned  |
 |                   | from Valorant / Vanguard.         |
 +-------------------+-----------------------------------+
 | SPOOF MAC         | Standalone MAC address            |
 |                   | randomization for the active      |
 |                   | network adapter.                  |
 +-------------------+-----------------------------------+

SPOOFER EXECUTION CONSOLE LOG
Estimated time: ~15s

[+] SMBIOS serials + UUID written [SUCCESS]
[+] MAC ADDRESS SPOOFED & ADAPTER RESET [SUCCESS]
[+] WINDOWS MACHINEGUID CLEANED [SUCCESS]
[+] ARPCACHE & SYSTEM TRACES WIPED [SUCCESS]
[+] DRIVERS UNLOADED [SUCCESS]
[SUCCESS] FULL SPOOFING APPLIED SUCCESSFULLY!
[!] Please restart your computer to apply all changes
    completely.

+------------------------------------------------------+
| REBOOT YOUR PC IMMEDIATELY: You must restart your      |
| computer right after the success message appears. Do   |
| not open any game before rebooting.                    |
+------------------------------------------------------+


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Network Unflag
========================================================

Anti-cheats flag both your physical network card (MAC
Address) and your external Public IP Address.

 1. MAC ADDRESS SPOOFING (AUTOMATIC)
    Adapt automatically randomizes the MAC address of your
    active physical network adapter during the spoof
    process.

 2. PUBLIC IP ADDRESS RESET (CRUCIAL)
    2.1 RESTART YOUR HOME ROUTER
        Unplug your internet router/modem from the power
        outlet for 60 seconds, then plug it back in. This
        assigns you a new dynamic public IP from your ISP.
    2.2 VERIFY YOUR NEW IP
        Visit whatismyipaddress.com before and after to
        confirm your IP address has changed.


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Make a New Account
========================================================

This is where 90% of beginners make a fatal mistake:
Logging back into an old account.

+------------------------------------------------------+
| CRITICAL RULE: DO NOT LOG INTO ANY ACCOUNT THAT HAS    |
| BEEN PREVIOUSLY BANNED. If you do, the anti-cheat will |
| link your new clean hardware to the banned profile and |
| re-ban you instantly!                                  |
+------------------------------------------------------+

RULES FOR YOUR NEW ACCOUNT:
- Create a brand new email address (ProtonMail, Gmail,
  Outlook).
- Register a brand new game account (Epic Games, Steam,
  Riot, Battle.net).
- Do NOT link old phone numbers or payment cards that were
  used on banned accounts.
- Do NOT add the same friends list immediately.


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Usage of VPN
========================================================

If your Internet Service Provider (ISP) provides a Static
IP Address (an IP that never changes even when you restart
your router), using a gaming VPN is strongly recommended.

RECOMMENDED VPNS:
- Mullvad VPN (best privacy, no logs, port-forwarding).
- NordVPN (fast servers, low ping).
- ExitLag (route optimization, safe for gaming).

+------------------------------------------------------+
| DYNAMIC IP USERS: If your public IP changes when you   |
| reboot your router, a VPN is optional but still        |
| provides an extra layer of safety.                     |
+------------------------------------------------------+


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Extra Safety Step
========================================================

Before launching your game client, you can clear stale
game client logs and caches that might still contain old
account identifiers.

CLEAR GAME CACHE:
Press Win + R, type %localappdata% and delete the trace
folders of your banned game:
- Fortnite: Delete FortniteGame\Saved\Config and
  EpicGamesLauncher\Saved.
- Valorant: Delete Riot Games\Riot Client\Data.


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Finish
========================================================

Open Command Prompt (cmd) to verify your new hardware
serial numbers:

 1. Check Motherboard Serial Number:
       wmic baseboard get serialnumber

 2. Check System UUID:
       wmic csproduct get uuid

 3. Check Network MAC Address:
       getmac

 4. Check Disk Drive Serials:
       wmic diskdrive get serialnumber

If all values are new, launch your game on the new account.


========================================================
 EAC / BE (FORTNITE, APEX, RUST, ETC)
 Fortnite Tournaments
========================================================

Special guidelines if you intend to participate in
competitive Fortnite tournaments (Cash Cups, FNCS, Ranked
Cups):

- Enable 2FA on your brand new clean account using a fresh
  Authenticator App.
- Level up your account through regular Ranked matches
  before registering for high-stakes tournaments.
- Never use public or flagged cheat loaders on the same
  Windows session.

========================================================
 END OF GUIDE - ADAPT
========================================================
