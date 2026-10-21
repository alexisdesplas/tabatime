# Tabatime

Timer d'intervalles pour la corde à sauter, installable sur iPhone (PWA).

Par défaut : 6 rounds de 1:00 de corde et 0:30 de repos, avec 10 s de préparation. Tout se règle depuis le bouton en haut à droite.

## Installer sur iPhone

1. Ouvrir https://alexisdesplas.github.io/tabatime/ dans **Safari**.
2. Bouton Partager → **Sur l'écran d'accueil**.
3. Lancer l'appli depuis l'icône : plein écran, fonctionne hors connexion.

## Fichiers

- `index.html` : toute l'appli (interface, timer, sons, réglages)
- `manifest.webmanifest` : nom, icônes, mode plein écran
- `sw.js` : cache hors connexion (changer `CACHE` à chaque mise à jour)
- `icons/` : icônes de l'appli

## Développer

Aucune dépendance. Pour tester en local : `python3 -m http.server` puis ouvrir http://localhost:8000.
Chaque push sur `main` met à jour le site GitHub Pages.
