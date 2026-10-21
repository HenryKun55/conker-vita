# Conker's Bad Fur Day — native PS Vita port

Conker's Bad Fur Day (Rare, 2001) running natively on the PS Vita / PS TV. This is
not an emulator: the N64 game was statically recompiled to C with
[N64Recomp](https://github.com/N64Recomp/N64Recomp) (based on
[CBFD-Recompiled](https://github.com/sciaschi/CBFD-Recompiled)) and built for the
Vita, with the N64's graphics drawn by a renderer written for the Vita's GPU
(sceGxm) and the audio microcode running on the Vita's CPU.

## Disclaimer

**This release contains no game files, and none will be provided.** It is an
unofficial fan project, not affiliated with or endorsed by Rare, Microsoft,
Nintendo or Sony. To play you need your own copy of the game: a ROM dumped from
your US cartridge. Please don't ask for ROMs in this thread or in DMs.

## What works

- The game, with saves (the N64's EEPROM, kept in `ux0:data/conker/saves`).
- Runs at the game's own 30 fps cap in play (the rainy opening, one of the
  heaviest scenes, holds 29-30 fps on a real Vita), widescreen 16:9 (4:3
  optional), CPU clocked to 444 MHz automatically.
- Sound and music (the audio microcode reimplemented natively, bit-exact with the
  original), cutscenes (skippable, even the ones you haven't seen yet, if you like).
- **Text in six languages**: English, Português (Brasil), Español, Français,
  Deutsch, Italiano. Every speech bubble, menu, tutorial and hint, with the
  accented letters drawn into the game's own font. The translations are faithful
  to the original: swearing where the game swears, local slang and accents
  (the farmhands, the cavemen, Birdy, the Professor...). The text language follows
  your Vita's system language until you pick one.
- Censorship on or off (off by default): the swear words or the game's own symbols.
- **Optional up to 60 fps (interpolated)**: a frame drawn between each two of the
  game's where there's time for it (Frame rate in the port's menu). Off by default:
  shadows, footprints and dust still move at 30.
- **The Vita's buttons in the game**: every N64 button picture (speech bubbles,
  the hint book, menus, the B-pads in Windy) is redrawn as the Vita control bound
  to that button, and follows your remapping.
- Remappable controls, both sticks, front and rear touch; aiming with the right
  stick and inverted aiming, if you like.
- Skip the intro, and cheats (infinite health and lives, full cash).
- Updates from inside the game (see below).

## Requirements

1. A PS Vita or PS TV with HENkaku/Ensō and VitaShell.
2. **libshacccg.suprx** (the runtime shader compiler) in `ur0:data/`. If you
   already play other ports you have it. Otherwise, extract it with
   [ShaRKBR33D](https://github.com/Rinnegatamante/ShaRKBR33D).
3. **The US ROM** of Conker's Bad Fur Day:
   - SHA-1 `4cbadd3c4e0729dec46af64ad018050eada4f47a` (big-endian `.z64`; a
     byte-swapped dump of the same cartridge works too)
   - Other regions (PAL) are not supported.
   - To check yours before copying it: `shasum -a 1 yourfile.z64` (macOS/Linux) or
     `certutil -hashfile yourfile.z64 SHA1` (Windows) must print the SHA-1 above.
     A byte-swapped `.v64`/`.n64` dump of the same cartridge prints a different
     SHA-1 but is accepted too: rename it to `baserom.us.z64` and the game converts it.
   - With a wrong ROM the game says so at start-up ("This isn't the US version of
     Conker's Bad Fur Day, the only one supported.") and closes; nothing is changed.

## Installation

1. Install `conker.vpk` with VitaShell.
2. Copy your ROM to the Vita (USB or FTP with VitaShell) as:

   ```
   ux0:data/conker/baserom.us.z64
   ```

3. Launch **Conker's Bad Fur Day** from the LiveArea. The first start checks the
   ROM and keeps its own copy; after that the file you copied can be deleted.

Updating to a newer version only needs the new vpk (or the in-game updater); your
saves and settings in `ux0:data/conker/` are kept.

## Controls

| Vita | N64 |
| --- | --- |
| Left stick | Analog stick |
| Right stick | C-buttons (camera, ...) |
| Cross | A |
| Square | B |
| L | Z |
| R | R |
| Triangle | C-Up |
| Circle | C-Right |
| Rear touch, bottom left / bottom right | C-Left / C-Down |
| D-pad | D-pad |
| Start | Start |
| Select | L |

Every N64 button can be bound to one or two Vita controls (buttons, either
stick's directions, the four corners of the front or rear touch panels).

## The port's menu

**SELECT twice** opens the port's own menu, at any time (it closes the same
way; the game is paused while it's open). It's in pages:

- **Picture**: widescreen 16:9 or the original 4:3, cutscenes widescreen or 4:3,
  frame rate 30 (original) or up to 60 (interpolated).
- **Buttons**: rebind every N64 button.
- **Sticks and aiming**: which stick moves Conker, dead zone, sensitivity,
  inverting either stick, aiming with the camera stick, aiming left/right and
  up/down inverted or not.
- **Game**: language (English, Português, Español, Français, Deutsch, Italiano),
  censorship, skip cutscenes (only the ones you've watched, any, or even the
  unskippable ones), skip the intro.
- **Cheats**: infinite health, infinite lives, full cash.
- **Reset buttons and sticks**, **Quit game**.

Your choices are saved in `ux0:data/conker/controls.ini`.

## Updates

At start-up, over the boot picture, the game looks for a newer release of the
port (for at most about three seconds; without Wi-Fi it goes straight on). When
one is found it offers it: X installs it, O skips it. The download is checked,
the game closes, puts the new files in place of its own and starts again; saves,
settings and your ROM in `ux0:data/conker` are kept. If anything goes wrong the
installed game keeps working, and the details are in
`ux0:data/conker/update/update.log`. The installed version is shown in the
bottom right corner of the boot screen.

Installing an update needs **Enable Unsafe Homebrew** on in Settings > HENkaku
Settings (the installer writes the game's own folder in `ux0:app`). With it off
the game only tells you that a new version is available and how to turn it on,
then starts as usual.

To turn the check off, create `ux0:data/conker/conker.cfg` with the line
`update_check=0`.

## Known issues

- Only the US ROM is supported.
- Multiplayer has not been tested.
- The log is written to `ux0:data/conker/log.txt`; please include it when
  reporting a problem.

## Source code

The port's source is at [HenryKun55/CBFD-Recompiled](https://github.com/HenryKun55/CBFD-Recompiled)
(branch `vita-port`, the `vita/` folder), with how to build it in its README.

## Credits

- Port to the PS Vita: HenryKun55.
- [CBFD-Recompiled](https://github.com/sciaschi/CBFD-Recompiled) by Sean Ciaschi
  and contributors, on [N64Recomp](https://github.com/N64Recomp/N64Recomp) and
  N64ModernRuntime by Wiseguy and contributors.
- [VitaSDK](https://vitasdk.org), [vitaGL](https://github.com/Rinnegatamante/vitaGL)
  and vitashark by Rinnegatamante.
- Boot screen font: Oswald (SIL Open Font License).

Conker's Bad Fur Day © Rare / Microsoft. This project is not affiliated with them.
