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
- Style « Apple », retenu par Alexis le 22/10/2026 à la place de la version Big Shoulders / Archivo. Noir OLED `#000`, surfaces iOS `#1c1c1e` / `#2c2c2e`, texte secondaire `#8e8e93`, bleu système `#0a84ff` pour les actions de la feuille. Couleurs de phase : préparation jaune `#ffd60a`, effort rose Move `#ff375f`, repos et fin vert `#30d158`. Polices système uniquement (SF Pro via `-apple-system`), aucune police web.
- Composition : grand titre iOS 34 pt à gauche avec le mode et le réglage en sous-titre gris, bouton rond de réglages à droite, barre de rounds fine en couleur de phase. Anneau épais (11) avec piste de la même teinte à 20 %, comme les anneaux Activité. Chiffres en graisse 200, très grands, comme le minuteur de l'app Horloge. Trois boutons ronds en bas avec liseré séparé par un filet noir (Horloge) ; Démarrer est plus grand et teinté de la couleur de phase à 26 %.
- Réglages : feuille qui monte du bas (`dialog`, courbe `--sheet`), poignée, Annuler / OK en bleu système, contrôle segmenté pour le mode, liste groupée avec séparateurs iOS d'un demi-pixel, steppers iOS.
- Interface en français. Détail métier : l'heure de fin de séance (« fin 17:32 ») sur la ligne du bas pendant que ça tourne.
- Mouvement : courbes `--ease` (iOS) et `--sheet` (feuille) dans `:root`, transform/opacité/couleur. Les 3 dernières secondes font pulser les chiffres ; la fin dessine l'anneau. `prefers-reduced-motion` coupe les transforms, garde les fondus.
- État pause : `data-paused` sur `.app` atténue anneau et centre à 40 %, « En pause » sous l'anneau. `data-phase` sur `<html>` pilote la couleur de phase ; `meta[name=theme-color]` reste `#000000`.
- Steppers des réglages : un tap = un pas, maintien appuyé = répétition (pointerdown + setInterval). Le `click` ne sert qu'au clavier (`e.detail === 0`).
