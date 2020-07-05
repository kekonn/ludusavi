# Ludusavi
[![Version](https://img.shields.io/crates/v/ludusavi)](https://crates.io/crates/ludusavi)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Ludusavi is a tool for backing up your PC video game save data, written in Rust.
It is cross-platform and supports multiple game stores.

This tool uses the [Ludusavi Manifest](https://github.com/mtkennerly/ludusavi-manifest)
for info on what to back up, and it will automatically download the latest version of
the primary manifest. To add or update game entries in the primary manifest, please refer
to that project. Data is ultimately sourced from [PCGamingWiki](https://www.pcgamingwiki.com/wiki/Home),
so you are encouraged to contribute any new or fixed data back to the wiki itself.

## Features
* Backup and restore for Steam as well as other game libraries.
* Preview of the backup/restore before actually performing it.
* Support for saves that are stored as files and in the Windows registry.
* Support for Proton saves with Steam.
* Support for Steam screenshots.

## Usage
### Installation
* Download the executable for your operating system.
* If you are on Windows, when you first run Ludusavi, you may see a popup
  that says "Windows protected your PC", because Windows does not recognize
  the program's publisher. Click "more info" and then "run anyway" to start
  the program.

### Backup mode
> ![Ludusavi in backup mode](./docs/backup.png)

* This is the default mode when you open the program.
* You can press `preview` to see what the backup will include,
  without actually performing it.
* You can press `back up` to perform the backup for real.
  * If the target folder already exists, it will be deleted first,
    then recreated.
  * Within the target folder, for every game with data to back up,
    a subfolder will be created with the game's name encoded as base 64.
    For example, files for `Celeste` would go into a folder named `Q2VsZXN0ZQ==`.
  * Within each game's backup folder, any relevant files will be stored with
    their name as the base 64 encoding of the full path to the original file.
    For example, `D:/Steam/steamapps/common/Celeste/Saves/0.celeste` would be
    backed up as `RDovU3RlYW0vc3RlYW1hcHBzL2NvbW1vbi9DZWxlc3RlL1NhdmVzLzAuY2VsZXN0ZQ==`.
  * If the game has save data in the registry and you are using Windows, then
    the game's backup folder will also contain an `other/registry.yaml` file.
    If you are using Steam and Proton instead of Windows, then the Proton `*.reg`
    files will be backed up like other game files.
* By default, no roots are configured. Roots are folders that Ludusavi can
  check for additional game data. You can click `add root` and configure
  as many as you need, along with its type:
  * For a Steam root, this should be the folder containing the `steamapps` and
    `userdata` subdirectories. Here are some common/standard locations, if you
    haven't chosen a custom place:
    * Windows: `C:/Program Files (x86)/Steam`
    * Linux: `~/.steam/steam`
  * For the "other" root type, it should be a folder whose direct children are
    individual games. For example, in the Epic Games store, this would be
    what you choose as the "install location" for your games (e.g., if you choose
    `D:/Epic` and it creates a subfolder for `D:/Epic/Celeste`, then the root
    would be `D:/Epic`).

### Restore mode
> ![Ludusavi in restore mode](./docs/restore.png)

* Switch to restore mode by clicking the `=> restore` button.
* You can press `preview` to see what the restore will include,
  without actually performing it.
* You can press `restore` to perform the restore for real.
  * For all the files in the source directory, they will be decoded as base 64
    to get the target path and then copied to that location. Any necessary
    parent directories will be created as well before the copy, but if the
    directories already exist, their current files will be left alone (other
    than overwriting the ones that are being restored from the backup).

## Comparison with other tools
There are other excellent backup tools available, but not a singular
cross-platform and cross-store solution:

* [GameSave Manager](https://www.gamesave-manager.com):
  * Only supports Windows and Steam.
  * Closed source, so the community cannot contribute improvements.
  * Interface can be slow or unresponsive; e.g., when (de)selecting all checkboxes,
    it takes half a second per checkbox for them all to toggle.
* [Gaming Backup Multitool for Linux](https://supremesonicbrazil.gitlab.io/gbml-web)
  * Only supports Linux and Steam.
  * Database is not actively updated (as of 2020-06-20, the last update was 2018-06-05).

## Development
Please refer to [CONTRIBUTING.md](./CONTRIBUTING.md).
