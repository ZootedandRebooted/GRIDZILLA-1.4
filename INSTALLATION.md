# GRIDZILLA for MPC Sample firmware 1.4

This package is for **Akai MPC Sample firmware 1.4.0.8 (d87ac8c12e9)**. Other firmware builds are rejected by the launcher. “1.4” identifies the supported MPC firmware.

## Install

1. Back up your SD card. On the MPC, open **SHIFT + PAD 8 → Dev Enhancements (Beta)**. Enable **Developer Mode** and **Custom Plug-In Support**, accepting their prompts, then return to the normal screen.
2. Extract `GRIDZILLA-1.4.zip`. Copy the package's **WaveGrid** folder and **autorun.sh** to the SD-card root. Merge folders when updating an existing installation; preserve existing release folders and back up files before replacing them. Keep your projects and samples.
3. Insert the card in the MPC. Accept the one-time **Autorun script** installation prompt. If needed, re-enable Developer Mode and return to the normal screen. Wait for installation to finish; its result is saved in `WaveGrid/STARTUP-INSTALL-RESULT.txt`.
4. Restart the MPC with the card inserted. Leave the stock screen idle while startup completes, normally about 20–30 seconds. **Gridzilla now starts automatically without a Mac or another Yes prompt.**

On macOS or Linux, the included SD-copy installer also verifies the package and backs up existing startup pointers. From the extracted package folder, run `sh install.sh --install "/path/to/mounted/SD"`, then perform steps 3–4 on the MPC. `sh install.sh --verify` checks the package without installing it.

The one-time installer removes its own root `autorun.sh` after success. Do not copy it back for normal use. A successful log ends with `SUCCESS`. A failed installation leaves the log and installer in place so the reported problem can be corrected.

## Using Gridzilla

| Open | Control |
| --- | --- |
| Grid | SHIFT + SEQ |
| Mixer and inserts | SHIFT + PAD 5 |
| Henry effects | Mixer → pad or MASTER → Inserts → empty slot |

The nine HS effects are Heat, Pump, Bus Compressor, Channel Strip, Para EQ, Tube Strip, Bass Focus, Crush and Saturator. Flex Beat remains available on pad inserts. The included control sheets cover the Arranger and HS effect editing, pages, bypass and presets.

## Disable or remove automatic startup

- **Temporary bypass:** remove the SD card before restarting, or create a file named `DISABLE_AUTOSTART` inside `WaveGrid` on the card. The MPC starts stock. Remove that file to enable automatic startup again.
- **Remove the persistent startup integration:** copy `Recovery/autorun.sh` to the SD-card root as `autorun.sh`, replacing only a previous Gridzilla installer file. Insert the card and accept the removal prompt once. Check `WaveGrid/STARTUP-REMOVAL-RESULT.txt`, then restart. This preserves Gridzilla's files, projects and samples.
- With an existing developer shell on the MPC, `sh /media/<card-name>/WaveGrid/Startup/install-startup.sh --remove` performs the same removal. `--verify` checks the package; `--install` reinstalls startup.

## How startup is installed

The installer adds a small compiled callback under `/data/gridzilla-startup/1.4-r1` and its configuration at `/var/lib/alsa/conf.d/99-gridzilla-startup.conf`. This is persistent custom code loaded during audio initialization. Damaged callback files or configuration can interfere with audio startup; keep the payload and configuration together, and use the removal installer rather than deleting the payload alone.

The callback leaves audio settings unchanged. It waits for a ready, stopped stock project, verifies the exact firmware and runtime, and launches Gridzilla from the SD card. It makes one launch attempt per boot and retains the stock-app fallback. It changes no firmware image or bootloader. Existing startup configurations are backed up; unrelated configurations are refused.

The runtime and startup paths require the SD card. A renamed card is supported. If the matching package is absent, disabled or fails verification, startup leaves the stock app running.
