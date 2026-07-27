# BO3 Linux

> Play **Call of Duty: Black Ops III** on **Fedora Linux** using Wine and BOIII.

![Fedora](https://img.shields.io/badge/Fedora-42-294172?logo=fedora)
![Wine](https://img.shields.io/badge/Wine-10.x-red)
![DXVK](https://img.shields.io/badge/DXVK-enabled-orange)
![License](https://img.shields.io/github/license/interverti/bo3-linux)

---

## 📖 Table of Contents

- [Requirements](#-requirements)
- [Install Wine](#-install-wine)
- [Create Wine Prefix](#-create-the-wine-prefix)
- [Configure Wine](#-configure-wine)
- [Install Steam](#-install-steam-optional)
- [Launch BOIII](#-launch-boiii)
- [Troubleshooting](#-troubleshooting)

---

## ✨ Features

- Fedora-focused
- Native Vulkan via DXVK
- PulseAudio / PipeWire support
- BOIII compatible
- Simple launch script

---

> [!IMPORTANT]
> This guide assumes you already own **Call of Duty: Black Ops III**.

## 📦 Requirements

- Fedora 41+
- Wine
- Winetricks
- Vulkan drivers
- BO3 Windows installation

---

# 🍷 Install Wine

```bash
sudo dnf install wine winetricks
```

Install the required libraries:

```bash
sudo dnf install \
    wine.i686 \
    mesa-vulkan-drivers \
    mesa-vulkan-drivers.i686 \
    vulkan-loader \
    vulkan-loader.i686 \
    libX11.i686 \
    libXext.i686 \
    libXrandr.i686 \
    alsa-lib.i686 \
    pulseaudio-libs.i686
```

---

# 📁 Create the Wine Prefix

```bash
export WINEPREFIX="$HOME/.wine-bo3"
export WINEARCH=win64

wineboot -u
```

---

# ⚙️ Configure Wine

Install DXVK:

```bash
export WINEPREFIX="$HOME/.wine-bo3"

winetricks -q dxvk
```

Enable PulseAudio:

```bash
export WINEPREFIX="$HOME/.wine-bo3"

winetricks -q sound=pulse
```

---

# 🎮 Install Steam (Optional)

Download Steam:

```bash
cd /tmp

wget https://cdn.akamai.steamstatic.com/client/installer/SteamSetup.exe

wine SteamSetup.exe
```

> [!NOTE]
> Steam on Wine is only required for Ezz! Only install it and dont touch it . BOIII itself can be launched without Steam.

---

# 🚀 Launch BOIII

Create `launch-bo3.sh`

```bash
#!/usr/bin/env bash

export WINEPREFIX="$HOME/.wine-bo3"
export PULSE_LATENCY_MSEC=60

export WINEDLLOVERRIDES="winepulse.drv=b;winealsa.drv=d;dsound=n,b"

cd "/path/to/Black Ops III"

wine boiii.exe \
    -unsafe-lua \
    -nointro \
    -noupdate \
    -nosteam \
    -noratelimit \
    -mitigatepacketspam
```

Make it executable:

```bash
chmod +x launch-bo3.sh
```

Run it:

```bash
./launch-bo3.sh
```

---

# 🔧 Troubleshooting

### Black screen

- Verify Vulkan is installed.
- Reinstall DXVK.

### No audio

Run:

```bash
winetricks sound=pulse
```

### Game crashes

Delete the Wine prefix:

```bash
rm -rf ~/.wine-bo3
```

and recreate it.

---

# ⚠️ Disclaimer

This repository **does not** distribute any game files.

You must legally own **Call of Duty: Black Ops III**.

---

# ❤️ Credits

- Wine
- Winetricks
- DXVK
- BOIII
