<div align="center">

# 🎮 BO3 Linux

Lancez **Call of Duty®: Black Ops III** avec le **client EZZ BOIII** sur **Fedora Linux** en utilisant **Wine + DXVK**.

![Fedora](https://img.shields.io/badge/Fedora-42-294172?logo=fedora)
![Wine](https://img.shields.io/badge/Wine-10.x-red)
![DXVK](https://img.shields.io/badge/DXVK-enabled-orange)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Guide simple pour installer et jouer à **BO3** sur Fedora Linux avec **Wine**, **DXVK** et le **client EZZ BOIII**.

</div>

---

## 📸 Aperçu

<p align="center">
  <img src="images/gamepreview1.png" alt="Menu principal de BO3 sur Fedora Linux" width="48%">
  <img src="images/gamepreview2.png" alt="Gameplay de BO3 sur Fedora Linux" width="48%">
</p>

> [!NOTE]
> Ces captures d’écran ont été prises sur **Fedora Linux** avec **Wine + DXVK** et le **client EZZ BOIII**.

---

## ✨ Fonctionnalités

- 🎮 Jouez à **Call of Duty®: Black Ops III** avec le **client EZZ BOIII**
- 🐧 Configuration centrée sur Fedora
- ⚡ Rendu Vulkan via **DXVK**
- 🔊 Support PipeWire / PulseAudio
- 🚀 Script de lancement léger
- 🧩 Compatible Zombies, Multijoueur, Mods et cartes custom
- 🧩 Compatible avec le contenu Steam Workshop via BOIIIWD
- 🛠️ Facile à reproduire sur un nouveau préfixe Wine

---

> [!IMPORTANT]
> Ce guide suppose que vous possédez déjà **Call of Duty: Black Ops III**.

## 📦 Prérequis

- Fedora 41+
- Wine
- Winetricks
- Pilotes Vulkan
- Installation Windows de BO3

---

# 🍷 Installer Wine

```bash
sudo dnf install wine winetricks
```

Installer les bibliothèques nécessaires :

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

# 📁 Créer le préfixe Wine

```bash
export WINEPREFIX="$HOME/.wine-bo3"
export WINEARCH=win64

wineboot -u
```

---

# ⚙️ Configurer Wine

Installer DXVK :

```bash
export WINEPREFIX="$HOME/.wine-bo3"

winetricks -q dxvk
```

Activer PulseAudio :

```bash
export WINEPREFIX="$HOME/.wine-bo3"

winetricks -q sound=pulse
```

---

# 🎮 Installer Steam

Télécharger Steam :

```bash
cd /tmp

wget https://cdn.akamai.steamstatic.com/client/installer/SteamSetup.exe

wine SteamSetup.exe
```

> [!NOTE]
> Steam n’est nécessaire que pour l’installation initiale du client EZZ BOIII.  
> Une fois installé, laissez l’installation de Steam telle quelle.

# 📦 Installer les fichiers BOIII

```
mkdir -p ~/.wine-bo3/drive_c/users/$USER/AppData/Local/boiii

cd ~/.wine-bo3/drive_c/users/$USER/AppData/Local/boiii

wget https://github.com/Ezz-lol/boiii-free/archive/refs/tags/v1.3.2.zip

unzip boiii-free-1.3.2.zip
```

---

# 🚀 Lancer BOIII

Créez le fichier `launch-bo3.sh` :

```bash
#!/bin/bash

export WINEPREFIX=~/.wine-bo3
export PULSE_LATENCY_MSEC=60
export WINEDLLOVERRIDES="winepulse.drv=b;winealsa.drv=n;dsound=n,b;xinput1_0=b;xinput1_2=b;xinput_9_1_0=b;XINPUT_9_1_0=b;xinput1_3=b;xinput=b"

cd "/chemin/vers/Black Ops III" || { echo "Dossier du jeu introuvable !"; exit 1; }

wine boiii.exe -unsafe-lua -nointro -noupdate -nosteam -noratelimit -mitigatepacketspam
```

Rendez-le exécutable :

```bash
chmod +x launch-bo3.sh
```

Lancez-le :

```bash
./launch-bo3.sh
```

---

# 🔧 Dépannage

### Écran noir

- Vérifiez que Vulkan est bien installé.
- Réinstallez DXVK.

### Pas de son

Exécutez :

```bash
winetricks sound=pulse
```

### Le jeu plante

Supprimez le préfixe Wine :

```bash
rm -rf ~/.wine-bo3
```

puis recréez-le.

---

# ⚠️ Avertissement

Ce dépôt **ne distribue aucun** fichier du jeu.

Vous devez légalement posséder **Call of Duty: Black Ops III**.

---

## ✅ Compatibilité

| Composant | Statut |
|-----------|:------:|
| Fedora 41 | ✅ |
| Fedora 42 | ✅ |
| GPU AMD | ✅ |
| GPU Intel | ✅ |
| Pilote propriétaire NVIDIA | ✅ |
| PipeWire | ✅ |
| X11 | ✅ |
| Wayland | ✅ |

## ❓ FAQ

<details>
<summary>Comment jouer avec un ami qui est sur Windows ou Linux ?</summary>

Des outils comme Radmin VPN ne fonctionnent pas avec cette méthode. Je recommande d’utiliser quelque chose comme [playit](https://playit.gg/) pour la personne qui héberge, avec un tunnel **UDP** sur le port **27017** ou **27018**.

</details>

<details>
<summary>Est-ce que ça marche avec Steam Proton ?</summary>

Non.

Ce guide est spécifiquement conçu pour **Wine** et le **client EZZ BOIII**.

</details>

<details>
<summary>Est-ce que je peux jouer en mode Zombies ?</summary>

Oui.

BOIII prend en charge Zombies, le Multijoueur et les Mods.

</details>

<details>
<summary>Est-ce que je peux utiliser PipeWire ?</summary>

Oui.

PipeWire assure la compatibilité PulseAudio sur Fedora.

</details>

---

## 🧩 Utiliser le Workshop avec BOIIIWD

Si vous voulez télécharger et gérer du contenu **Steam Workshop** (cartes custom, mods, etc.), vous pouvez utiliser **BOIIIWD**.

BOIIIWD permet de parcourir et d’installer facilement du contenu Workshop tout en restant compatible avec le **client EZZ BOIII**.

### Téléchargement

- **Releases :** https://github.com/faroukbmiled/BOIIIWD/releases  
- **Code source :** https://github.com/faroukbmiled/BOIIIWD

> [!TIP]
> BOIIIWD est optionnel. Vous n’en avez besoin que si vous comptez utiliser du contenu Steam Workshop.

## ❤️ Crédits

| Projet | Description |
|--------|-------------|
| [EZZ BOIII](https://github.com/Ezz-lol/boiii-free) | Client communautaire BOIII |
| [Wine](https://www.winehq.org/) | Couche de compatibilité Windows |
| [Winetricks](https://github.com/Winetricks/winetricks) | Scripts d’aide pour Wine |
| [DXVK](https://github.com/doitsujin/dxvk) | Traduction Direct3D → Vulkan |
| [Fedora Project](https://fedoraproject.org/) | Distribution Linux |

---

## 📄 Licence

Ce projet est sous licence MIT.

Voir le fichier [LICENSE](LICENSE) pour plus de détails.

<p align="center">

Fait avec ❤️ pour la communauté du jeu sur Linux.

</p>
