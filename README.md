# Switch 2 · mac2jb Tutorial

A community setup guide for Apple Silicon Macs, with separate instructions for the full SmileBASIC game and the **Japanese SmileBASIC Demo 4.4.6**.

**[Read the complete guide](COMPLETE_GUIDE.txt)** · [Get mac2jb](https://github.com/cpypwn/mac2jb/releases) · [Upstream documentation](https://github.com/cpypwn/mac2jb)

## What was tested

| Component | Guide contributor's reported setup |
| --- | --- |
| Console | Nintendo Switch 2, firmware **23.0.1** |
| SmileBASIC | **Japanese Demo 4.4.6** |
| Mac | Apple Silicon **M2 MacBook Pro** |
| Save method | Eight-line PRG1 script below |
| Network | **Both Mac and Switch 2 on wired Ethernet** during mac2jb steps 3 and 4 |
| Result | Contributor reports successful bootstrap saving and completion of steps 3 and 4 |

These are user-reported results from the original tutorial, not independent hardware validation by this repository. The original report does not identify the exact mac2jb build or macOS version. “Steps 3 and 4” refers to the contributor's mac2jb workflow, not the numbered sections of this document. Do not infer that all homebrew tools were tested.

**Compatibility:** current upstream documentation lists SmileBASIC **4.4.9** on firmware **23.0.1**. Demo **4.4.6** is this contributor's separate result. Ethernet was needed in their setup; that does not prove every setup requires both devices to be wired. Wi-Fi is not verified here.

## Start here

1. Read [COMPLETE_GUIDE.txt](COMPLETE_GUIDE.txt), including the Mac driver caveats.
2. Preserve the tested console/software versions. Configure DNS on the connection actually used.
3. Prepare mac2jb, its USB driver, a USB-C **data** cable, and the shared Ethernet network.
4. Transfer the matching bootstrap into **PRG0**, then save using your edition's method below.
5. Restart the console and follow mac2jb's run instructions. Check actual verification, not just typing completion.

Current releases use a different bootstrap from older releases. Follow the complete guide's migration note before reusing a saved `NS2`.

## Full game: save and load

With the bootstrap in PRG0, return to **Direct mode** (X):

```basic
SAVE "NS2"
```

To restore it later:

```basic
LOAD "TXT:NS2"
```

The full game does not need the demo save script.

## Demo 4.4.6: open the editor and save

1. Launch SmileBASIC Demo and enter its programming workspace (**Make a Program**).
2. Connect a USB keyboard if available.
3. Press **Ctrl+1** to select the **PRG0** editor, which holds the transferred bootstrap. Keep that program intact.
4. Press **Ctrl+2** to select the **PRG1** editor, where you will type the eight-line saving script.
5. After entering the script, press **Esc** to return to **Direct mode** (or use the console's exit-editor control if Esc behaves differently).
6. Execute `RUN 1` **from Direct mode**, not from the editor.

**Keyboard reference:** Ctrl+1 → PRG0; Ctrl+2 → PRG1; Esc → Direct mode in the contributor's tested workflow. Confirm the active program slot before typing.

### Eight-line save script

Leave the complete bootstrap in **PRG0**. Switch the editor to **PRG1** and enter exactly these eight lines:

```basic
PRGEDIT 0
SRC$=""
FOR I=1 TO PRGSIZE(0)
  L$=PRGGET$()
  SRC$=SRC$+L$
NEXT
SAVEV "TXT:NS2",SRC$
END
```

Return to **Direct mode**, then enter:

```basic
RUN 1
```

`RUN 1` is a Direct-mode command, not a ninth script line. Check for errors and verify that `NS2` was saved. This is [HDR's save workaround](https://gist.github.com/HDR/6ca4e04945c1a8096cd36395e4c199f7), preserved from the contributor's tested procedure.

For manual demo loading and verification, see the complete guide. Saving program text does not preserve a live exploit or RPC connection.

## DNS reference

Verified on **October 26, 2026** against [ConsoleMods' 90DNS instructions](https://consolemods.org/wiki/Switch:Blocking_OFW_Updates#90DNS).

| Region | Primary IPv4 DNS | Secondary IPv4 DNS |
| --- | --- | --- |
| North / South America | `207.246.121.77` | `163.172.141.219` |
| Elsewhere | `163.172.141.219` | `207.246.121.77` |

Use Manual DNS on the Switch's selected network. The linked guide specifies `::` for both IPv6 DNS fields on Switch 2; this may be unsuitable for an IPv6-only network. Save, reconnect, and verify filtering. 90DNS also blocks Nintendo services and cannot guarantee update prevention.

## Credits and scope

- **cpypwn** — mac2jb and direct setup assistance acknowledged by the contributor.
- **HDR** — demo save workaround.
- **pepsipu and ns2jb contributors** — original exploit research; **kurumuz** — the compatibility fork used by current mac2jb.
- **ShinyQuagsire** — libusbd and macos_usb_gadget_poc.
- **Guide contributor** — Japanese Demo 4.4.6 testing and Ethernet observations.

This repository contains documentation, not the exploit binaries, USB driver, or Nintendo software. Upstream works keep their own licenses. Independent community guide; not affiliated with Nintendo or SmileBASIC. Source notes and troubleshooting are in [COMPLETE_GUIDE.txt](COMPLETE_GUIDE.txt).
