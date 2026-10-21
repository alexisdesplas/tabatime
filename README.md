# Tabatime

Timer d'intervalles pour la corde à sauter, installable sur iPhone (PWA).

Trois modes, à choisir depuis le bouton en haut à droite :

- **Tabata** : rounds d'effort chronométrés séparés par du repos. Par défaut 6 × 1:00 / 0:30.
- **EMOM** (Every Minute On the Minute) : un intervalle fixe, tu fais tes reps puis tu récupères jusqu'au suivant. Par défaut 10 × 1:00, objectif 30 reps.
- **Séries** : rounds × séries × reps avec repos entre les séries. La série n'est pas chronométrée : appuie sur **Fait** quand elle est finie et le repos démarre. Par défaut 4 × 3 × 10, repos 0:30.

Une préparation de 10 s précède le départ dans tous les modes.

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
