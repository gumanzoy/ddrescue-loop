## `ddrescue-loop` script restarts ddrescue in case of failure.

Compatible only with Linux, not with other *nix!  
Depends on udev `/dev` and sysfs `/sys` kernel interfaces

For SATA requires AHCI compatible motherboard. For all Intel and modern AMD platforms (AM4 and newer), check the UEFI Setup SATA settings to ensure Port Hot Plug is enabled.

For USB requires `lsusb` from `usbutils` package.  
And optional `uhubctl` for power off/on cycle. Or hardware USB Relay Module LCUS-1 CH340

### Usage:

Please note the order of arguments. Keys must be passed in the same order as they are written.  
Mixing not supported. Space between key and value is required.

## For SATA source drive:

Stop/start drive at SATA port:  
* `ddrescue-loop -ata <n> -stop`		stop drive at SATA port `<n>`  
* `ddrescue-loop -ata <n> -scan`		scan SATA port `<n>`

Launch recovery from SATA drive:  
`ddrescue-loop -ata <n> [-loop <n>] [-pwc] [-wait <n>] [-act <n>] outfile mapfile [ddrescue options]`

Specify the SATA port number to which the source drive is connected:  
`-ata <n>`		SATA port number `<n>` dight (look at dmesg output)

Stop/restart function for SATA drive in case of failure:  
`-loop <n>`		Limit number of attempts `<n>`

Timeout for drive get ready after stop/restart:  
`-wait <n>`		Time in seconds `<n>` [10]

Change ATA command timeout:  
`-act <n>`		Time in seconds `<n>` [30]

## For USB source drive:

Power off/power on USB device VID:PID ID, by hub or rle method:  
* `ddrescue-loop -usb <ID> -pwc hub`	Use `uhubctl --search <ID>`  
* `ddrescue-loop -usb <ID> -pwc rle`	Use USB Relay LCUS-1 CH340 `RLETTY=`

Launch recovery from USB drive:  
`ddrescue-loop -usb <ID> [-loop <n>] [-pwc <hub/rle>] [-wait <n>] outfile mapfile [ddrescue options]`

Specify the VID:PID ID of the source USB drive in Hex form:  
`-usb <ID>`	VID:PID separated by colon (look at `lsusb` output)

Restart `ddrescue` process (power off/on device) in case of failure:  
`-loop <n>`		Limit number of attempts `<n>`

### Main options:  
* `outfile`			output device / outfile  
* `mapfile`			ddrescue map/log file (required)

### After `mapfile` may specify `ddrescue` options:

Use `man ddrescue` to read full list of options. Some important options:

* `-P [<n>]`		show some lines of the latest data read [3]  
* `-b 4096`			Sector size of input device `<bytes>` [default 512]  
* `-c <n>`			Sectors to copy at a time `<n>` [default 128]  
* `-O` Important!		reopen input device file after every read error.  
* `-J` Optional		reread latest good sector after every error.  
* `-r <n>` OR `-r -1`	<n> retry passes before trim (-1=infinity) [0]  
* `-m <domain.mapfile>`	restrict domain to finished blocks in `<file>` `ddru_ntfsbitmap`

## Preview:
`ddrescue-loop`  
[![ddrescue-loop](https://asciinema.org/a/628786.svg)](https://asciinema.org/a/628786)  
`dmesg -Wt`  
[![dmesg -Wt](https://asciinema.org/a/628787.svg)](https://asciinema.org/a/628787)
