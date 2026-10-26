# Tabatime — contexte pour Claude Code

Timer d'intervalles pour la corde à sauter, en appli web installable (PWA) sur l'iPhone d'Alexis. Pas de build, pas de dépendances : HTML/CSS/JS tout simples.

## État actuel
- L'appli est terminée et testée au format iPhone : `index.html` (interface, timer, sons, réglages), `manifest.webmanifest`, `sw.js` (hors connexion), `icons/`.
- Trois modes (clé `mode` dans les réglages, gardés dans le localStorage) :
  - `tabata` : rounds × effort / repos (défaut 6 × 1:00 / 0:30).
  - `emom` : `emomRounds` intervalles de `emomEvery` s, objectif `emomReps` reps (0 = libre). Pas de phase repos, le repos est implicite dans l'intervalle.
  - `series` : `serRounds` × `serSets` séries × `serReps` reps, `serRest` s de repos entre séries. La phase série est non chronométrée (`untimed`) : chrono qui monte, bouton Go devenu « Fait » qui passe au repos.
  - `prep` (préparation, 0:30 par défaut comme dans la maquette) commun à tous les modes.
- Moteur : `build()` génère une liste `phases` selon le mode, `tick()` consomme le temps ; une phase `untimed` accumule `up` au lieu de décrémenter `remaining`.
- Dépôt GitHub public : https://github.com/alexisdesplas/tabatime (branche `main`, remote `origin`).
- GitHub Pages activé (branche `main`, racine `/`) : https://alexisdesplas.github.io/tabatime/

## Prochaines étapes
1. Ouvrir https://alexisdesplas.github.io/tabatime/ dans Safari sur l'iPhone → Partager → « Sur l'écran d'accueil ».

## À savoir
- Cible : iPhone (Safari). `navigator.vibrate` n'existe pas sur iOS ; les bips passent par Web Audio et ne démarrent qu'après un tap (bouton Go). Le mode silencieux peut couper le son.
- À chaque mise à jour, incrémenter `CACHE` dans `sw.js` (ex. `tabatime-v2`) pour que le téléphone récupère la nouvelle version.
- Tester en local : `python3 -m http.server` puis http://localhost:8000.
- Style Hardwood (design system, mode « Indigo DT »), passé en code le 26/10/2026 à partir de la page Figma « Tabatime · App » (sections A à E). Remplace le style Apple du 22/10. Fond `#0f1016`, cartes `#1a1b24`, discret `#22232e`, bords `#2e303d` / `#636883`, texte `#f5f6fa` / `#c9ccd8` / `#9da1b8`, accent `#b3bcff` (soft `#262b4d`). Couleurs de phase : préparation `#ffa24d`, effort `#ff7a88`, repos et fin `#4ade80`. Police Inter auto-hébergée (`fonts/inter-latin.woff2`, en cache dans `sw.js`).
- Écran séance : titre 28 px = nom du mode, réglage en sous-titre, bouton rond de réglages, barre de rounds, anneau (trait 11, piste sombre de la teinte de phase), chiffres Inter Bold 64. Trois boutons Hardwood de 48 px : Reset / Passer en secondaire bordé, l'action principale au centre en accent indigo (et non plus en couleur de phase).
- Interface en français. Détail métier : l'heure de fin de séance (« fin 17:32 ») sur la ligne du bas pendant que ça tourne.
- Mouvement : courbes `--ease` et `--push` dans `:root`, transform/opacité/couleur. Les 3 dernières secondes font pulser les chiffres ; la fin dessine l'anneau. `prefers-reduced-motion` coupe les transforms, garde les fondus.
- État pause : `data-paused` sur `.app` atténue anneau et centre à 50 %, « En pause » sous l'anneau. `data-phase` sur `<html>` pilote la couleur de phase ; `meta[name=theme-color]` reste `#0f1016`.
- Réglages : écran dédié poussé depuis la droite (« ‹ Séance » annule, OK enregistre), contrôle segmenté pour le mode, sélecteur à roues (scroll-snap, 40 px par ligne, clavier ↑ ↓), aperçu de la séance (durée ou volume total, frise des phases, légende), « Rétablir les valeurs par défaut ». Les réglages absents de la maquette (préparation, reps visées EMOM, rounds Séries) sont dans une seconde carte de roues plus courte, sous l'aperçu.
- Lancement : écran vide puis le lettrage « Tabatime » apparaît, ~1,1 s, un tap le passe.
