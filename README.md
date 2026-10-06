# MukiVuki sous Linux / Wine

Installation et utilisation de MukiVuki Chess Studio sous Linux avec Wine.

Testé avec :

- Debian 13
- Wine Staging 11-16
- MukiVuki Windows 0.1.266 x64
- préfixe Wine 64 bits dédié

## Création du préfixe

```bash
export WINEARCH=win64
export WINEPREFIX="$HOME/.wine_muki"
wineboot



Installation

L'installateur MukiVuki utilise PowerShell pour rechercher et arrêter
une éventuelle instance déjà lancée de MukiVuki.
Sous Wine, l'implémentation PowerShell est incomplète et l'installateur
peut rester bloqué avec le message :
MukiVuki ne peut pas être fermé. Veuillez le fermer manuellement et
cliquer sur Réessayer pour continuer.

La solution consiste à désactiver powershell.exe pendant l'installation :

WINEPREFIX="$HOME/.wine_muki" \
WINEDLLOVERRIDES="powershell.exe=d" \
wine "$HOME/Téléchargements/MukiVuki-Windows-Setup-0.1.266-x64.exe"

Lancement
PowerShell n'a plus besoin d'être désactivé :

WINEPREFIX="$HOME/.wine_muki" \
wine "$HOME/.wine_muki/drive_c/Program Files/MukiVuki/MukiVuki.exe"


État
MukiVuki démarre et fonctionne sous Wine.
Testé avec succès :
- interface graphique
- moteur d'échecs Muki
- analyse
- jeu d'une partie
- serveur local utilisé par MukiVuki/Lichess
Au démarrage, MukiVuki peut notamment annoncer :

MukiVuki avec Lichess: http://127.0.0.1:27463/

Remarques
Quelques messages Chromium/Electron/Wine peuvent apparaître concernant
DirectComposition ou le processus GPU. Ils n'ont pas empêché le
fonctionnement de MukiVuki lors des tests.
Une erreur Electron :

A JavaScript error occurred in the main process
Error: open EACCES

a été observée lors d'un premier lancement, puis a disparu sans
modification supplémentaire. Les lancements suivants ont fonctionné
normalement.
Les refus EACCES observés avec strace concernaient essentiellement
/dev/input, /dev/hidraw et certains périphériques USB et ne se sont
pas révélés bloquants.

