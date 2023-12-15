# Sovol SV06 Plus firmware and parts

This is a fork of
[Sovol's official repository for the SV06 Plus 3D printer][sovol-sv06-plus-repo]
with customized firmware.

# New features

* On bed leveling ([`G29`][G29]) completion, return the LCD to the leveling
  screen. Originally the LCD would get stuck displaying the final leveling grid
  with no option to exit if `G29` were invoked via serial.

* Using [`M73`][M73], display progress and remaining time on the LCD just like
  when printing from the SD card. The LCD's print cancel button now works to
  stop the print, so long as the client supports the cancel host action (which
  [Octoprint][octoprint] does). This is adapted from
  [the LCD branch of daleye's fork][daleye-fork-lcd-branch].

# Building the firmware

The firmware ([Marlin][marlin]) is built with a tool called
[platformio][platformio], which is a Python package. Install this tool with pip
(or similar tool such as [pipx][pipx]):

```console
pip install platformio
```

Change to the `MarlinSV06Plus` subdirectory:

```console
cd MarlinSV06Plus/
```

Build the firmware:

```console
platformio run -e STM32F103RET6_sovol_maple
```

The built firmware is in `MarlinSV06Plus/.pio/build/STM32F103RET6_sovol_maple/`
named using the format `firmware-YYYYMMDD-HHMMSS.bin`.

# Installing the firmware

The firmware is updated using a microSD card.

[Sovol has an official firmware update video.][sovol-sv06-plus-firmware-update-video]
While the video says the microSD card must be formatted as FAT32 with a 4KiB
(4096 byte) block size, I could not get the firmware to update on my printer
with my microSD card formatted this way. However, using my microSD cards'
default formatting of FAT32 with a 512 block size worked fine, so long as the
card was 16GiB or less.

Update steps:

* Remove all other files and folders from the microSD card.
* Copy the built firmware `.bin` file to your microSD card.
  * **Important note:** The printer's mainboard remembers the file name from the
    previous firmware update, so ensure the new firmware file is named
    differently from the previous update (e.g. use `fw2.bin` if `fw1.bin` was
    used last time). The board only seems to remember the single previous
    filename. Also note that for this comparison, the board may be using only
    the first 6-8 characters of the file name (e.g. ancient MS-DOS filename
    shortening, `long_file_name.bin` -> `long_f~1.bin`).
* Unmount / eject / safely remove the microSD card from your computer.
* Turn off your printer.
* Insert the microSD card into your printer's microSD card slot.
* Turn on your printer.

The SV06 Plus LCD will show the Sovol logo, which will appear to freeze for
several seconds. When the firmware update is complete, the LCD animation of the
extruder printing Sovol's logo will continue as normal. If the LCD remains
frozen for more than 30 seconds or so, the firmware update process has failed.

# Firmware restoration / recovery

If the firmware update process fails, the board will need to be re-flashed with
a working firmware such as Sovol's official SV06 Plus firmware. The process is
the same as above, but using Sovol's official firmware image file.

[Sovol publishes firmware updates here][sovol-download-page]. Their SV06 Plus
firmware link points to [this Google Drive folder][sovol-sv06-plus-gdrive]. As a
last resort,
[I am including a copy of Sovol's official SV06 Plus firmware (version 1.1.5) in this repository as `SV06 Plus_V1.1.5_0324.bin`][local-firmware-copy].


[G29]: https://marlinfw.org/docs/gcode/G029-abl-bilinear.html
[M73]: https://marlinfw.org/docs/gcode/M073.html
[daleye-fork-lcd-branch]: https://github.com/daleye/SV06-PLUS/compare/42f16c0fdb5e7de022d615e0a665aed042e0be49...b5ec92a7a4487f183714fc544ca0ef5fd8cb180a
[local-firmware-copy]: /SV06%20Plus_V1.1.5_0324.bin
[marlin]: https://marlinfw.org/
[octoprint]: https://octoprint.org/
[pipx]: https://github.com/pypa/pipx
[platformio]: https://platformio.org/
[sovol-download-page]: https://www.sovol3d.com/pages/download
[sovol-sv06-plus-firmware-update-video]: https://www.youtube.com/watch?v=b2jUo1KnxZw
[sovol-sv06-plus-gdrive]: https://drive.google.com/drive/folders/1sJL5uCxHxQVBfpwitEse-BqS4v_j-ktz
[sovol-sv06-plus-repo]: https://github.com/Sovol3d/SV06-PLUS
