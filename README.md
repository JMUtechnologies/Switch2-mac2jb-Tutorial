# Switch 2 · mac2jb Tutorial

Community guide for Apple Silicon Macs, SmileBASIC full **4.4.9**, and the **Japanese Demo 4.4.6**.

**Updated October 27, 2026** against cpypwn's current README, which documents **v0.5.0**. This is a documentation update, not a new hardware test.

[Complete guide](COMPLETE_GUIDE.txt) · [Official releases](https://github.com/cpypwn/mac2jb/releases) · [Upstream instructions](https://github.com/cpypwn/mac2jb/blob/main/README.md)

## Requirements

- Apple Silicon Mac with macOS 14 or newer.
- Switch 2 firmware **23.0.1**, with SmileBASIC full **4.4.9** or demo **4.4.6**. Upstream says earlier firmware should also work; that is not confirmation of every version.
- SmileBASIC on **internal storage**, with **more than 20 MiB free in its save**.
- USB-C **data cable**, and **both devices connected by Ethernet to the same network**.
- Switch USB Keyboard setting: **US English**.

Before testing, follow the linked [console-update blocking guide](https://consolemods.org/wiki/Switch:Blocking_OFW_Updates#90DNS). DNS filtering can block Nintendo services and cannot guarantee update prevention.

## 1. Prepare the Mac

Download and extract the official release, then open `mac2jb.app`.

The unsigned USB driver requires compatible startup-security and SIP settings. **Keep an existing working partial/custom SIP configuration.** Otherwise follow the Recovery and driver instructions in [the complete guide](COMPLETE_GUIDE.txt) and upstream README. These changes reduce Mac protections.

Use **Install & load driver…**, complete any approval and restart prompts, then refresh driver status. Continue only once the driver is loaded and no driver-install restart is pending. Reinstalling creates another restart requirement.

**USB power direction:** the **Switch supplies power to the Mac**. If the Mac is charging the Switch, disconnect the cable at the Switch, wait about one second, and reconnect it. A loaded driver alone does not prove keyboard input works.

## 2. Choose the matching host and replace the bootstrap

**Demo automation uses BermTheSequel, the default host. Keep that host selected for Demo 4.4.6.**

Developer Options also offers pepsipu and kurumuz. Changing hosts requires typing and saving that host's matching bootstrap. pepsipu does not support held RPC; kurumuz and BermTheSequel do.

**Replace your previous bootstrap, including the one from build 516.** Do not reuse an older NS2 merely because it saved successfully.

1. Open SmileBASIC → **Make a Program**, with an empty **PRG0 / slot 0** editor.
2. Open **First Time Setup** in mac2jb.
3. Tick the demo checkbox for **Demo 4.4.6**; leave it off for the full version.
4. Start typing and let it finish.
5. Press **X** on the Switch controller to return to **Direct mode**.
6. Check that the command prompt is empty, then click **Type Save Command** in mac2jb.
7. Confirm the save on the Switch; overwrite NS2 if prompted.
8. Once saving finishes, confirm saving in mac2jb.

Use Skip only when slot 0 already contains the complete bootstrap matching this build and selected host.

Defaults: **10 ms per key**, **1 second between commands**, and **1 second before load confirmation**. Adjust timing through USB & Timing if needed.

**Editor shortcut:** the original contributor used **Ctrl+1** for the PRG0 editor. The current upstream procedure uses controller **X** to return to Direct mode. Confirm the active slot before typing.

## 3. Manual save/load reference

The GUI can type the save command for you. These are the current upstream manual alternatives; enter them in **Direct mode**.

### Full version

Save:

```basic
SAVE "NS2"
```

Load:

```basic
LOAD "TXT:NS2"
```

Accept the load confirmation.

### Demo 4.4.6

Save the entire bootstrap in slot 0 with this single line:

```basic
PRGEDIT 0:C$="":FOR I=0 TO PRGSIZE(0)-1:C$=C$+PRGGET$() :NEXT:SAVEV "TXT:NS2",C$
```

Load:

```basic
PRGEDIT 0:LOADV "TXT:NS2" OUT C$:PRGSET C$
```

The loop reads every line; a single PRGGET$() call does not save the whole bootstrap. With Demo enabled, mac2jb sends the load command and Enter, waits one second, then sends Enter again to accept OK before RUN.

The older eight-line PRG1 workaround is retained as historical reference in the complete guide; it is no longer the main setup route.

## 4. Run the jailbreak

1. Restart the Switch and open SmileBASIC → **Make a Program → Direct mode**.
2. Connect USB and Ethernet. Select the **Mac's wired IPv4 address** in mac2jb.
3. Check the edition checkbox and matching host. **Demo: BermTheSequel.**
4. Confirm the Switch is ready, then click **Jailbreak**.
5. Keep SmileBASIC focused. Wait for **4/4 verification** and a held connection before launching homebrew.

Typing completion is not exploit verification. A saved NS2 preserves program text, not a live jailbreak or RPC connection.

## 5. Homebrew

Choose **one app per held session**. These are upstream-documented options, not a claim that this guide independently tested each app.

| Option | Use |
| --- | --- |
| GPUBrewLauncher | Hello World, Froggy Road and ftpd-sb; FTP port **5000** |
| HBLauncher v0.2.0 | Original GPU launcher; ftpd-sb uses port **5000** |
| IZLauncher | IZ2S console shell and file exchange through held RPC, including demo through BermTheSequel |
| OunceCake | Exports your own games' unencrypted content; FTP port **2121** |
| Stremio | Requires Stremio on the Mac; leave the manual add-on field empty for account add-ons |

For extra compatible launcher apps, copy the complete app folder into the current session's `vendor/GPUBrewLauncher/apps/` or `vendor/HBLauncher/apps/`, then refresh. New sessions have fresh app libraries; keep originals elsewhere.

Stremio uses a dedicated TCP receiver for larger video/menu frames; audio and session control still use RPC. The Mac must stay connected.

## 6. Finish or retry

Close FTP connections, choose **Finish Session**, and wait for cleanup. **Restart the Switch before another attempt. Never send REMOVE twice in the same SmileBASIC session.**

If typing stops halfway, inspect and clear partial input before retrying. Keep logs for diagnosis, but review them before sharing.

## Evidence, credits and scope

The earlier guide recorded contributor-reported Japanese Demo 4.4.6 testing on an M2 MacBook Pro, including bootstrap saving and the contributor's “steps 3 and 4.” Its exact build/macOS version was not recorded. That report does not independently validate v0.5.0 or every homebrew app.

Credits: **cpypwn** (mac2jb); **BermTheSequel** (default host/demo support); **pepsipu** (original ns2jb research); **kurumuz** (compatibility updates); **ShinyQuagsire** (USB research); **misshexa66**, **ravensclawx**, **NiftyNova**, and **byizmysh** (homebrew tools); **HDR** (historical demo save workaround); and the guide contributor.

Documentation only; no exploit binaries, USB drivers, Nintendo software, session tokens or local logs are included. Original licenses and attribution remain applicable. Independent community guide, not affiliated with Nintendo or SmileBASIC.
