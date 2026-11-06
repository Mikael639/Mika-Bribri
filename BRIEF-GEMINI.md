# Brief — Scène 3D (Three.js + GSAP) pour la boîte et le gâteau

Ce brief s'adresse à l'IA (Gemini) qui va coder la suite. Lis-le en entier avant de toucher au code.

## Contexte

Carte d'anniversaire **mobile-first** pour Brigitte (20 ans, le 8 novembre 2026).
Tout le site tient dans **`index.html`** (HTML + CSS + JS, aucun build, aucune dépendance).
Déploiement : Vercel, **branche `main` publiée automatiquement à chaque push**.
→ **Travaille sur `main` uniquement, aucune autre branche.** Chaque push part en ligne : ne pousse que du code testé.

## Objectif

Remplacer **uniquement** la boîte cadeau + le gâteau de l'écran d'ouverture (aujourd'hui en CSS 3D + SVG)
par une **vraie scène Three.js**, et piloter l'enchaînement d'ouverture avec une **timeline GSAP**.
**Tout le reste de la carte ne bouge pas** (lettre à la plume, histoire, photos, appels, fin).

### La scène à construire (fidèle à l'existant)

1. **Boîte** blanche cassé (#FCFBFC) à petits points caramel/rose/chocolat, vue légèrement de dessus.
   Couvercle en **deux rabats**, avec un **cœur rose poudré (#E8B4BE) coupé en deux** au milieu.
   Avant ouverture : légère oscillation + le cœur pulse (glow).
2. **Ouverture** (au toucher) : la boîte tremble, les deux rabats s'ouvrent vers l'extérieur (~118°),
   rayons de lumière derrière, le **gâteau monte** hors de la boîte.
3. **Gâteau** : cylindre rose (dégradé #E27BA5 → #FDCBDE → #BB4772), dessus rose clair,
   **couronne de perles nacrées** en haut et en bas (sphères blanches, léger reflet, apparition une par une),
   3 nœuds bordeaux sur le côté, petits vermicelles, **assiette blanche**.
   Textes : « Happy Birthday » sur le dessus, **« Mon amour »** sur le côté (écrits progressivement).
4. **Bougie en forme de « 20 »** (rayée rose/blanc) avec **flamme animée** + **PointLight chaude** qui éclaire le gâteau.
5. **Nuit** : la lumière ambiante baisse, seule la bougie éclaire (ombres douces si le téléphone le permet).
6. **Souffler** : la flamme s'éteint, fumée, la lumière revient → l'existant gère la suite (confettis, feux, titre).
7. Bonus si fluide : elle peut **faire tourner le gâteau du doigt** (OrbitControls limités, sans zoom).

Palette générale de la carte : fond crème caramel #F8EFE6, chocolat #3B2418, rose poudré #B5677A / #7A3E4E.

## Contraintes techniques (importantes)

- **Pas de build** : charger Three.js et GSAP en modules ES depuis un CDN avec un `<script type="importmap">`
  (versions **fixées**, ex. `three@0.16x` et `gsap@3.x`). Ne pas ajouter npm/Vite.
- **Performance mobile** : viser 60 fps sur un Android moyen.
  `renderer.setPixelRatio(Math.min(devicePixelRatio, 2))`, géométries légères, ombres seulement si nécessaire,
  **arrêter la boucle de rendu** quand la scène n'est plus visible (après « Lire ma carte »).
- **Repli obligatoire** : si WebGL n'est pas disponible ou si le chargement CDN échoue,
  **garder la version actuelle CSS/SVG** (elle doit rester dans le code et fonctionner).
- **`prefers-reduced-motion`** : respecter (pas d'animations, états finaux directs).
- Ne pas casser la **responsivité** (testé de 320 px à 1280 px) ni l'écriture Playwrite FR Trad.

## Comment c'est branché aujourd'hui (à respecter)

Dans `index.html`, le JS est un seul bloc en bas. Points d'entrée :

| Élément | Rôle |
|---|---|
| `<div class="gift-wrap">` / `<button id="gift">` (~ligne 1315) | conteneur de la boîte + gâteau à remplacer par le `<canvas>` Three.js |
| `var T = {...}` (~2075) | timings de l'ouverture (à reprendre dans la timeline GSAP) |
| `box.addEventListener("click", ...)` (~2369) | un seul écouteur : ouvrir / accélérer / souffler |
| `startOpen()`, `openBox()`, `goDark()`, `showWish()`, `skipToWish()` | étapes de l'ouverture |
| `blowOut()` (~2488) | bougie soufflée → confettis, feux, titre, `showRead()` |
| `center(flame)` | position écran de la flamme : utilisée pour la lumière de nuit (`#dim`) et la fumée → **fournir l'équivalent depuis la scène 3D** (projeter la position de la flamme en coordonnées écran) |
| `writeCake()` / `unwriteCake()` | écriture des textes du gâteau |
| `fx` (moteur canvas 2D) | confettis, feux d'artifice, fumée, cœurs, papillons — **à garder tel quel**, il se superpose à la scène |
| `sfx` | bruitages — à garder |
| bouton « Rejouer » (~2892) | doit **remettre la scène 3D à zéro** (rabats fermés, gâteau en bas, bougie éteinte) |
| classes d'état sur `#sceneBox` | `is-open`, `is-candle`, `is-lit`, `is-wish`, `is-blown`, `is-done`, `fast` |

Le micro (souffler pour de vrai) appelle `blowOut()` et met à jour `--wind` sur la flamme : brancher ça sur l'inclinaison de la flamme 3D.

## Tests à faire avant chaque push

- Ouvrir `index.html?apercu` (saute le compte à rebours ; n'importe quelle réponse passe aux questions).
- Parcours complet : toucher le cœur → ouverture → toucher pour accélérer → souffler → « Lire ma carte » → « Rejouer » → rouvrir.
- Aucune erreur dans la console. Tester en largeur 375 px et 1280 px.
- Tester le **repli** : désactiver WebGL (ou bloquer le CDN) → la version CSS/SVG actuelle doit s'afficher.
- Vérifier sur un **vrai téléphone** après déploiement.

## À ne pas faire

- Ne pas créer de branche, ne pas réécrire le reste de la carte, ne pas changer les textes.
- Ne pas mettre de nouvelles données personnelles (le dépôt est public).
- Ne pas supprimer la version CSS/SVG : c'est le repli.
