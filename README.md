# Fedora Post-Installation Guide

Eine strukturierte Übersicht und Befehlsliste für die Einrichtung von Fedora Linux direkt nach der Installation.

## 🚀 Post-Install Befehle (Nach RPM Fusion)

Führe diese Befehle nacheinander im Terminal aus, um dein System zu aktualisieren, Codecs freizuschalten und die Grafiktreiber zu installieren.

| Schritt | Ziel | Befehl | Hinweis |
| :--- | :--- | :--- | :--- |
| **1** | **System aktualisieren** | `sudo dnf upgrade --refresh -y` | Aktualisiert alle Pakete und erneuert den Cache vor den Codecs. |
| **2** | **RPM Fusion aktivieren** | `sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm -y` | Schaltet die Repositories für zusätzliche Codecs und NVIDIA frei. |
| **3** | **Vollständiges ffmpeg tauschen** | `sudo dnf swap ffmpeg-free ffmpeg --allowerasing -y` | Ersetzt die eingeschränkte Standard-Version durch das vollständige ffmpeg. |
| **4** | **Multimedia-Codecs installieren** | `sudo dnf group install multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin -y` | Installiert die nötigen Audio- und Videopakete ohne unnötige Abhängigkeiten. |
| **5** | **Intel Hardware-Beschleunigung** | `sudo dnf install intel-media-driver -y` | Aktiviert die GPU-Videobeschleunigung für Intel-Prozessoren. |
| **6** |use the openh264 library, so you need the repository to be explicitly enabled On Fedora up to 40.| `sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1`       |

    
| **7** | **NVIDIA Treiber installieren** | `sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda -y` | Installiert den proprietären Grafiktreiber inklusive CUDA-Unterstützung. |

---

## ⚠️ Wichtiger Hinweis zu Schritt 6 (NVIDIA)

Nachdem du den NVIDIA-Befehl ausgeführt hast, **warte unbedingt 3–5 Minuten vor einem Neustart!** 

Das System baut den Treiber im Hintergrund für deinen aktuellen Kernel (`akmod`). Wenn du zu früh neu startest, kann es zu einem Blackscreen kommen. Sobald die Festplattenaktivität nachlässt, starte den PC neu.

### Treiber-Status prüfen
Nach dem Neustart kannst du mit folgendem Befehl überprüfen, ob der NVIDIA-Treiber korrekt geladen wurde:
```bash
modinfo nvidia | grep version
```
