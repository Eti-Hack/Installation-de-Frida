
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

### 1.3 Vérification de Frida

```powershell
frida --version
```



---

## Étape 2 — Installation d'ADB <a name="étape-2"></a>

### 2.1 Vérification d'ADB

```bash
adb version
```



### 2.2 Détection de l'émulateur

```powershell
adb devices
```



---

