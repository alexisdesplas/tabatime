# Tabatime — contexte pour Claude Code

Timer d'intervalles pour la corde à sauter, en appli web installable (PWA) sur l'iPhone d'Alexis. Pas de build, pas de dépendances : HTML/CSS/JS tout simples.

## État actuel
- L'appli est terminée et testée au format iPhone : `index.html` (interface, timer, sons, réglages), `manifest.webmanifest`, `sw.js` (hors connexion), `icons/`.
- Trois modes (clé `mode` dans les réglages, gardés dans le localStorage) :
  - `tabata` : rounds × effort / repos (défaut 6 × 1:00 / 0:30).
  - `emom` : `emomRounds` intervalles de `emomEvery` s, objectif `emomReps` reps (0 = libre). Pas de phase repos, le repos est implicite dans l'intervalle.
  - `series` : `serRounds` × `serSets` séries × `serReps` reps, `serRest` s de repos entre séries. La phase série est non chronométrée (`untimed`) : chrono qui monte, bouton Go devenu « Fait » qui passe au repos.
  - `prep` (préparation) commun à tous les modes.
- Moteur : `build()` génère une liste `phases` selon le mode, `tick()` consomme le temps ; une phase `untimed` accumule `up` au lieu de décrémenter `remaining`.
- Dépôt GitHub public : https://github.com/alexisdesplas/tabatime (branche `main`, remote `origin`).
- GitHub Pages activé (branche `main`, racine `/`) : https://alexisdesplas.github.io/tabatime/

## Prochaines étapes
1. Ouvrir https://alexisdesplas.github.io/tabatime/ dans Safari sur l'iPhone → Partager → « Sur l'écran d'accueil ».

## À savoir
- Cible : iPhone (Safari). `navigator.vibrate` n'existe pas sur iOS ; les bips passent par Web Audio et ne démarrent qu'après un tap (bouton Go). Le mode silencieux peut couper le son.
- À chaque mise à jour, incrémenter `CACHE` dans `sw.js` (ex. `tabatime-v2`) pour que le téléphone récupère la nouvelle version.
- Tester en local : `python3 -m http.server` puis http://localhost:8000.
- Couleurs : effort `#ff5a36`, repos `#2fc4b2`, préparation `#f2c230`, fond `#121417`. Polices : Big Shoulders Display (chiffres/titres) et Archivo (texte).
- Deux états d'écran pilotés par des attributs sur `<html>` : `data-phase` (prep/work/rest/done) et `data-lit`. Le fond reste sombre ; quand le timer tourne (`data-lit`), `--ground` et `--surface` se teintent à 12 % de la couleur de phase via `color-mix` (retenue voulue : Alexis a trouvé les aplats de couleur pleins agressifs, décision du 22/10/2026). En pause, fond neutre `#121417`. Un seul accent à l'écran : la couleur de phase (anneau, libellé, bouton Go). La barre de rounds en haut est bord à bord, sous la zone sûre : c'est la seule rupture de grille, et son remplissage est en encre claire, pas en couleur.
- Pas de traits : boutons et pastille de réglages séparés du fond par un pas de ton (`--surface`), jamais par une bordure.
- Interface en français. Détail métier sur la ligne du bas : l'heure de fin de séance (« fin 17:32 ») pendant que ça tourne.
- Mouvement : courbes `--ease-out` / `--ease-in-out` dans `:root`, durées 120–320 ms, transform/opacité/couleur. Un seul moment signature : la teinte du fond et la couleur de l'anneau qui basculent à chaque phase (320 ms). Les 3 dernières secondes font pulser les chiffres ; la fin dessine l'anneau. `prefers-reduced-motion` coupe les transforms, garde les fondus.
- État pause : l'écran s'éteint (`data-lit` retiré) et `data-paused` sur `.app` atténue anneau et centre à 50 %, « En pause » sous l'anneau. `meta[name=theme-color]` suit la teinte du fond (`GROUND` dans le JS, même mélange que le CSS).
- Steppers des réglages : un tap = un pas, maintien appuyé = répétition (pointerdown + setInterval). Le `click` ne sert qu'au clavier (`e.detail === 0`).
- Détecteur Impeccable : un faux positif « low-contrast » persiste (il croise la couleur de texte de `#go` avec le fond de survol de `.ctl`, que `#go` ne reçoit jamais : `:not(#go)` + spécificité de l'id).
