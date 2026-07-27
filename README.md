# bo3-linux
A full guide on how to install and play BO3 on Fedora Linux with Wine.

**What you will need to install** :

Install wine and winetricks
```sudo dnf install wine winetricks```

32-bits libs and tools that will be useful
```
sudo dnf install wine.i686 mesa-vulkan-drivers mesa-vulkan-drivers.i686 \
                 vulkan-loader vulkan-loader.i686 \
                 libX11.i686 libXext.i686 libXrandr.i686 \
                 alsa-lib.i686 pulseaudio-libs.i686
```

Make a prefix for Wine

```
export WINEPREFIX=~/.wine-bo3
export WINEARCH=win64
wineboot -u
```

Install DXVK and audio and video settings

```
export WINEPREFIX=~/.wine-bo3
winetricks -q sound=pulse shader_backend=glsl renderer=vulkan dxvk
```

Install Steam on Wine

```
export WINEPREFIX=~/.wine-bo3

cd /tmp
wget https://cdn.akamai.steamstatic.com/client/installer/SteamSetup.exe

wine SteamSetup.exe
```

Force Wine to use Pulse for audio in game

```
export WINEPREFIX=~/.wine-bo3

winetricks -q sound=pulse

wine reg add "HKCU\\Software\\Wine\\Drivers" /v Audio /t REG_SZ /d "pulse" /f
```

Go to where you have the game stored and make this sh with right permissions and then run it and you will be able to play https://github.com/Ezz-lol/boiii-free/ on Linux with a BO3 Windows Version

```
#!/bin/bash

export WINEPREFIX=~/.wine-bo3
export PULSE_LATENCY_MSEC=60
export WINEDLLOVERRIDES="winepulse.drv=b;winealsa.drv=n;dsound=n,b;xinput1_0=b;xinput1_2=b;xinput_9_1_0=b;XINPUT_9_1_0=b;xinput1_3=b;xinput=b"

cd "/disk/whereyourgameis/" || { echo "The game folder doesnt exist !"; exit 1; }

wine boiii.exe -unsafe-lua -nointro -noupdate -nosteam -noratelimit -mitigatepacketspam

```

Remplace the location in the cd section, its a placeholder on this guide

