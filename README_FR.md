# SystemPulse

🌍 **Disponible en 7 langues :** 🇺🇸 [English](README.md) | 🇹🇷 [Türkçe](README_TR.md) | 🇩🇪 [Deutsch](README_DE.md) | 🇫🇷 Français | 🇪🇸 [Español](README_ES.md) | 🇯🇵 [日本語](README_JA.md) | 🇨🇳 [中文](README_ZH.md)

Une application légère et native pour la barre de menus macOS qui affiche les métriques système en temps réel avec de beaux mini-graphiques.

<img width="397" height="715" alt="image" src="https://github.com/user-attachments/assets/6868a0ac-1d01-45aa-84d7-8d21dc0daa6b" />

![macOS](https://img.shields.io/badge/macOS-14.0%2B-blue)
![Swift](https://img.shields.io/badge/Swift-5.9%2B-orange)
![Licence](https://img.shields.io/badge/Licence-MIT-green)

## Fonctionnalités

- **Surveillance CPU** - Pourcentage d'utilisation, température, fréquence (Intel), suivi par cœur avec graphique historique
- **Surveillance mémoire** - Mémoire utilisée/libre, répartition active/wired/compressée
- **Surveillance GPU** - Pourcentage d'utilisation, température, taux de rafraîchissement de l'écran (Hz)
- **Surveillance réseau** - Vitesses de téléchargement/envoi, IP locale et publique, totaux de session
- **Surveillance disque** - Pourcentage d'utilisation, espace libre, santé du SSD (si disponible)
- **Surveillance batterie** - Niveau de charge, état de charge, temps restant, consommation électrique
- **Surveillance ventilateurs** - RPM pour chaque ventilateur (si disponible)
- **Infos système** - Charge moyenne, nombre de processus, utilisation swap, version du noyau, temps de fonctionnement, luminosité de l'écran
- **Support multilingue** - Choisissez votre langue depuis le menu (7 langues disponibles)

### Fonctionnalités interactives

- **Cliquez** sur n'importe quelle carte pour ouvrir l'application système correspondante (Moniteur d'activité, Utilitaire de disque, Préférences Système, etc.)
- **Clic droit** sur l'icône de la barre de menus pour un menu rapide avec paramètres et sélection de langue

## Configuration requise

- macOS 14.0 (Sonoma) ou ultérieur
- Mac Apple Silicon ou Intel

## Installation

### Option 1 : Compiler depuis les sources

1. Cloner le dépôt :
   ```bash
   git clone https://github.com/bluewave-labs/systempulse.git
   cd systempulse
   ```

2. Compiler l'application :
   ```bash
   swiftc -O -o SystemPulse SystemPulse.swift -framework Cocoa -framework IOKit -framework Metal
   ```

3. Exécuter :
   ```bash
   ./SystemPulse
   ```

### Option 2 : Créer un bundle d'application (optionnel)

Si vous voulez que SystemPulse apparaisse comme une vraie application macOS :

1. Créer la structure de l'application :
   ```bash
   mkdir -p SystemPulse.app/Contents/MacOS
   cp SystemPulse SystemPulse.app/Contents/MacOS/
   ```

2. Créer `SystemPulse.app/Contents/Info.plist` :
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>CFBundleExecutable</key>
       <string>SystemPulse</string>
       <key>CFBundleIdentifier</key>
       <string>com.bluewave.systempulse</string>
       <key>CFBundleName</key>
       <string>SystemPulse</string>
       <key>CFBundleVersion</key>
       <string>1.0</string>
       <key>LSMinimumSystemVersion</key>
       <string>14.0</string>
       <key>LSUIElement</key>
       <true/>
   </dict>
   </plist>
   ```

3. Déplacer vers Applications (optionnel) :
   ```bash
   mv SystemPulse.app /Applications/
   ```

### Option 3 : Exécuter avec Automator (recommandé)

Cette méthode permet à SystemPulse de fonctionner indépendamment du Terminal, donc il continue de fonctionner même après avoir fermé le Terminal.

1. Compilez d'abord SystemPulse (voir Option 1 ci-dessus)

2. Ouvrez **Automator** (recherchez-le dans Spotlight)

3. Cliquez sur **Nouveau document** et sélectionnez **Application**

4. Dans la barre de recherche, tapez "Exécuter un script shell" et faites-le glisser dans la zone de workflow

5. Remplacez le texte par défaut par le chemin complet vers votre binaire SystemPulse :
   ```bash
   /chemin/vers/systempulse/SystemPulse
   ```
   Par exemple, si vous avez cloné dans votre dossier personnel :
   ```bash
   ~/systempulse/SystemPulse
   ```

6. Allez dans **Fichier** > **Enregistrer** et enregistrez-le sous "SystemPulse" dans votre dossier Applications

7. Double-cliquez sur l'application Automator enregistrée pour exécuter SystemPulse

**Astuce** : Vous pouvez maintenant ajouter cette application Automator à vos Ouverture à la connexion pour démarrer SystemPulse automatiquement au démarrage :
1. Ouvrez **Réglages Système** > **Général** > **Ouverture**
2. Cliquez sur **+** et sélectionnez votre application Automator SystemPulse

### Lancer à la connexion (alternative)

Si vous avez créé un bundle d'application (Option 2), vous pouvez l'ajouter directement aux Ouverture :

1. Ouvrez **Réglages Système** > **Général** > **Ouverture**
2. Cliquez sur **+** et ajoutez SystemPulse.app

## Utilisation

Une fois lancé, SystemPulse apparaît dans votre barre de menus affichant l'utilisation CPU et mémoire.

- **Clic gauche** sur l'élément de la barre de menus pour ouvrir le panneau détaillé
- **Clic droit** pour un menu rapide avec paramètres, sélection de langue et option Quitter
- **Cliquez** sur une carte pour ouvrir l'application système associée

### Changer de langue

1. Faites un clic droit sur l'icône SystemPulse dans la barre de menus
2. Sélectionnez **Langue** dans le menu
3. Choisissez votre langue préférée dans le sous-menu

## Détails techniques

SystemPulse utilise les APIs natives macOS pour des métriques précises :

- **CPU** : API Mach `host_processor_info()`
- **Mémoire** : API Mach `host_statistics64()`
- **GPU** : Service IOKit `IOAccelerator`
- **Réseau** : `getifaddrs()` pour les statistiques d'interface
- **Batterie** : `IOPSCopyPowerSourcesInfo()` depuis IOKit
- **Température/Ventilateurs** : SMC (System Management Controller) via IOKit

## Contribuer

Les contributions sont les bienvenues ! N'hésitez pas à soumettre une pull request.

### Ajouter des traductions

SystemPulse permet d'ajouter facilement de nouvelles langues. Pour ajouter une nouvelle langue :

1. Ajoutez un nouveau cas à l'enum `Language`
2. Ajoutez les traductions pour toutes les chaînes dans le struct `L10n`
3. Soumettez une pull request

## Licence

Licence MIT - voir [LICENSE](LICENSE) pour plus de détails.

## Remerciements

Développé avec Swift et AppKit pour des performances macOS natives.
