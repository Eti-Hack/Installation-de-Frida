
# Guide d'installation et d'utilisation de Frida


##  Objectifs 

- Installer et configurer Frida (client Python + CLI) sur Windows
- Déployer et lancer `frida-server` sur un émulateur Android
- Établir une connexion et injecter des scripts d'instrumentation
- Observer les bibliothèques natives, les appels réseau et les accès fichiers
- Hooker des méthodes Java liées à SharedPreferences, SQLite et au débogage

---

##  Environnement de travail <a name="environnement"></a>

| Composant | Valeur |
|-----------|--------|
| Système d'exploitation | Mac Os (13) |
| Architecture PC | AMD64 |
| Python | 3.13.5 |
| Frida client | 17.9.3 |
| frida-server | 17.9.3 |
| ADB | 1.0.41 (version 37.0.0) |
| Émulateur | Android Emulator 5554 |
| Image système | Google APIs Intel x86_64 — Android 11.0 (API 35) |
| Architecture émulateur | x86_64 |

---

## Étape 1 — Installation de Python et Frida <a name="étape-1"></a>

### 1.1 Vérification de Python

```powershell
python --version
```

> **Résultat obtenu :**
> ```
> Python 3.13.5
> ```

### 1.2 Installation de Frida

```bash
pip3 install frida-tools
```
```bash
(base) us@MacBook-Pro-de-us ~ % pip3 install frida-tools
Collecting frida-tools
  Downloading frida_tools-14.8.2.tar.gz (4.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.7/4.7 MB 1.5 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: colorama<1.0.0,>=0.2.7 in /opt/miniconda3/lib/python3.13/site-packages (from frida-tools) (0.4.6)
Collecting frida<18.0.0,>=17.9.0 (from frida-tools)
  Downloading frida-17.9.3-cp37-abi3-macosx_10_13_x86_64.whl.metadata (2.3 kB)
Collecting prompt-toolkit<4.0.0,>=2.0.0 (from frida-tools)
  Using cached prompt_toolkit-3.0.52-py3-none-any.whl.metadata (6.4 kB)
Requirement already satisfied: pygments<3.0.0,>=2.0.2 in /opt/miniconda3/lib/python3.13/site-packages (from frida-tools) (2.19.1)
Collecting websockets<14.0.0,>=13.0.0 (from frida-tools)
  Using cached websockets-13.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata (6.8 kB)
Collecting wcwidth (from prompt-toolkit<4.0.0,>=2.0.0->frida-tools)
  Using cached wcwidth-0.6.0-py3-none-any.whl.metadata (30 kB)
Downloading frida-17.9.3-cp37-abi3-macosx_10_13_x86_64.whl (22.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 22.9/22.9 MB 1.0 MB/s eta 0:00:00
Using cached prompt_toolkit-3.0.52-py3-none-any.whl (391 kB)
Using cached websockets-13.1-cp313-cp313-macosx_10_13_x86_64.whl (155 kB)
Using cached wcwidth-0.6.0-py3-none-any.whl (94 kB)
Building wheels for collected packages: frida-tools
  WARNING: Building wheel for frida-tools failed: [Errno 13] Permission denied: '/Users/us/Library/Caches/pip/wheels/51'
Failed to build frida-tools
ERROR: Failed to build installable wheels for some pyproject.toml based projects (frida-tools)
(base) us@MacBook-Pro-de-us ~ % frida --version
zsh: command not found: frida
(base) us@MacBook-Pro-de-us ~ % sudo pip3 install frida-tools
Password:
WARNING: The directory '/Users/us/Library/Caches/pip' or its parent directory is not owned or is not writable by the current user. The cache has been disabled. Check the permissions and owner of that directory. If executing pip with sudo, you should use sudo's -H flag.
Collecting frida-tools
  Downloading frida_tools-14.8.2.tar.gz (4.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.7/4.7 MB 1.4 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: colorama<1.0.0,>=0.2.7 in /opt/miniconda3/lib/python3.13/site-packages (from frida-tools) (0.4.6)
Collecting frida<18.0.0,>=17.9.0 (from frida-tools)
  Downloading frida-17.9.3-cp37-abi3-macosx_10_13_x86_64.whl.metadata (2.3 kB)
Collecting prompt-toolkit<4.0.0,>=2.0.0 (from frida-tools)
  Downloading prompt_toolkit-3.0.52-py3-none-any.whl.metadata (6.4 kB)
Requirement already satisfied: pygments<3.0.0,>=2.0.2 in /opt/miniconda3/lib/python3.13/site-packages (from frida-tools) (2.19.1)
Collecting websockets<14.0.0,>=13.0.0 (from frida-tools)
  Downloading websockets-13.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata (6.8 kB)
Collecting wcwidth (from prompt-toolkit<4.0.0,>=2.0.0->frida-tools)
  Downloading wcwidth-0.6.0-py3-none-any.whl.metadata (30 kB)
Downloading frida-17.9.3-cp37-abi3-macosx_10_13_x86_64.whl (22.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 22.9/22.9 MB 1.7 MB/s eta 0:00:00
Downloading prompt_toolkit-3.0.52-py3-none-any.whl (391 kB)
Downloading websockets-13.1-cp313-cp313-macosx_10_13_x86_64.whl (155 kB)
Downloading wcwidth-0.6.0-py3-none-any.whl (94 kB)
Building wheels for collected packages: frida-tools
  Building wheel for frida-tools (pyproject.toml) ... done
  Created wheel for frida-tools: filename=frida_tools-14.8.2-py3-none-any.whl size=4716375 sha256=712476daf8f4beba70ac0871f1777e2c177b0892ca39b5cfc1f57c327b86788a
  Stored in directory: /private/tmp/pip-ephem-wheel-cache-cezh0dyr/wheels/51/9e/90/61721a5b2fb2ff9c7daf6328ee64466608edb6f30be3899cd0
Successfully built frida-tools
```

### 1.3 Vérification de Frida

```powershell
frida --version
```
```powershell
(base) us@MacBook-Pro-de-us ~ % frida --version
17.9.3
```


---

## Étape 2 — Installation d'ADB

### 2.1 Vérification d'ADB

```bash
adb version
```

```bash
(base) us@MacBook-Pro-de-us Downloads % adb version
Android Debug Bridge version 1.0.41
Version 37.0.0-14910828
Installed as /usr/local/bin/adb
Running on Darwin 19.6.0 (x86_64)
(base) us@MacBook-Pro-de-us Downloads % 
```

### 2.2 Détection de l'émulateur

```bash
adb devices
```
```bash
(base) us@MacBook-Pro-de-us ~ % adb devices                  
List of devices attached
192.168.56.101:5555	device
```

## Étape 3 — Déploiement de frida-server

### 3.1 Identification de l'architecture

```bash
adb shell getprop ro.product.cpu.abi
```
```bash
x86_64
```

### 3.2 Téléchargement de frida-server

Fichier : `frida-server-17.9.3-android-x86_64.xz`  
Source : https://github.com/frida/frida/releases/tag/17.9.3

> ⚠️ La version de `frida-server` correspond exactement à la version du client Frida.

### 3.3 Décompression

Extraction  → fichier renommé en `frida-server`

### 3.4 Copie vers l'émulateur

```bash
adb push frida-server /data/local/tmp/
```

```bash
(base) us@MacBook-Pro-de-us Downloads % adb push frida-server /data/local/tmp
frida-server: 1 file pushed, 0 skipped. 1.5 MB/s (110825032 bytes in 68.208s)
```


### 3.5 Attribution des droits d'exécution

```bash
adb shell chmod 755 /data/local/tmp/frida-server
```

### 3.6 Lancement de frida-server

```bash
adb shell "/data/local/tmp/frida-server &"
```

### 3.7 Vérification

```bash
adb shell ps | grep frida
```

```bash
(base) us@MacBook-Pro-de-us ~ % adb shell ps | grep frida
root          10010      1 11890420 135580 poll_schedule_timeout 7bfbcf17aa0a S frida-server
```

---

