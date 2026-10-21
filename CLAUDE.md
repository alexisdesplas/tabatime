# Tabatime — contexte pour Claude Code

Timer d'intervalles pour la corde à sauter, en appli web installable (PWA) sur l'iPhone d'Alexis. Pas de build, pas de dépendances : HTML/CSS/JS tout simples.

## État actuel
- L'appli est terminée et testée au format iPhone : `index.html` (interface, timer, sons, réglages), `manifest.webmanifest`, `sw.js` (hors connexion), `icons/`.
- Par défaut : 6 rounds × 1:00 de corde / 0:30 de repos, 10 s de préparation. Réglages modifiables dans l'appli et gardés dans le localStorage.
- Dépôt GitHub public : https://github.com/alexisdesplas/tabatime (branche `main`, remote `origin`).
- GitHub Pages activé (branche `main`, racine `/`) : https://alexisdesplas.github.io/tabatime/

## Prochaines étapes
1. Ouvrir https://alexisdesplas.github.io/tabatime/ dans Safari sur l'iPhone → Partager → « Sur l'écran d'accueil ».

## À savoir
- Cible : iPhone (Safari). `navigator.vibrate` n'existe pas sur iOS ; les bips passent par Web Audio et ne démarrent qu'après un tap (bouton Go). Le mode silencieux peut couper le son.
- À chaque mise à jour, incrémenter `CACHE` dans `sw.js` (ex. `tabatime-v2`) pour que le téléphone récupère la nouvelle version.
- Tester en local : `python3 -m http.server` puis http://localhost:8000.
- Couleurs : effort `#ff5a36`, repos `#2fc4b2`, préparation `#f2c230`, fond `#121417`. Polices : Big Shoulders Display (chiffres/titres) et Archivo (texte).
- Interface en français.
