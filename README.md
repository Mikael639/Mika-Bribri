# Joyeux anniversaire Brigitte — 8 novembre

Site d'une seule page, pensé pour le téléphone. Aucun build, aucune installation :
double-clic sur `index.html` pour le voir (la musique ne marche qu'en ligne ou via
un petit serveur local).

## Le parcours

1. **Compte à rebours** jusqu'au 8 novembre à minuit (heure de son téléphone).
   Après minuit, cet écran est sauté.
2. ~~**Deux questions secrètes**~~ (retirées après le 8 novembre, `questions: []`) à la suite (son surnom préféré, puis le 5 septembre). Indice après
   2 essais ratés, et elle entre quand même après 5 essais. Une fois réussie, la
   question n'est plus reposée sur ce téléphone.
3. **La boîte cadeau en 3D** (vue de dessus, cœur sur le couvercle) tombe du
   ciel. Quand elle touche le cœur : la boîte tremble, les deux moitiés du
   couvercle s'ouvrent, rayons de lumière, canons à confettis, pluie de cœurs,
   musique. Le gâteau monte, les perles nacrées se posent une à une (petites
   notes), « Happy Birthday » puis **« Brigitte »** s'écrivent sur le gâteau.
   Toucher l'écran pendant l'ouverture **accélère** directement jusqu'au vœu.
4. **La bougie** se plante et s'allume, **la lumière s'éteint**. « Fais un vœu… » :
   elle **souffle dans le micro** (bouton « Souffler pour de vrai ») ou touche le
   gâteau. Fumée, la lumière revient : explosion de confettis, feux d'artifice,
   « Joyeux anniversaire, Brigitte ! ». Un **anneau de 8 photos** apparaît et tourne
   autour du gâteau (liste `CAROUSEL_PHOTOS` dans le script 3D). Puis **Lire ma carte**.
5. La carte s'ouvre **dans un cœur qui grandit**. Titre lettre par lettre, photo
   qui arrive en tournoyant avec des cœurs qui s'envolent, **message écrit par
   un stylo-plume** qui suit chaque lettre (toucher le message l'affiche d'un coup), photos qui arrivent
   penchées avec un reflet — **on peut les toucher pour les voir en grand**.
6. La fin : feu d'artifice, **Réponds-moi** (WhatsApp) et **Rejouer**.

Le micro ne marche qu'une fois en ligne (adresse en https) : le téléphone
demande l'autorisation. Si elle refuse, toucher le gâteau suffit.

Partout : un petit cœur apparaît là où elle touche l'écran. Sur ordinateur, la
souris est en forme de cœur.

## Ce qu'il reste à remplir

Tout ce qui est marqué `EXEMPLE` dans `index.html` :

| Quoi | Où |
|---|---|
| Les 3 paragraphes + la signature | bloc `A REMPLACER PAR TES MOTS` |
| Les 5 légendes des photos | les `<figcaption>` |
| La phrase de fin + « de ma part » | `<section class="outro">` |
| La phrase sous le cœur | `.hero__line` (« You have my heart ») |

Et dans le bloc **REGLAGES** en haut du `<script>` (fin du fichier) :

- `questions` — les deux questions secrètes, leurs réponses acceptées et leurs indices ;
- `musique` — mettre le fichier dans `musique/chanson.mp3` ;
- `distance` — vos deux villes et les km entre vous ;
- `appels` — vos captures d'appels WhatsApp (mettre les fichiers dans `photos/`,
  ex. `photos/appel-1.jpg`), une légende par appel, et ta petite vignette (`moi`) si tu veux ;
- `debutRelation` — déjà réglé sur le 5 septembre 2026 (compteur « Ensemble depuis ») ;
- `age` — son âge : la bougie prend la forme du chiffre (vide = bougie classique) ;
- `whatsapp` — ton numéro au format international, sans + ni espaces ;
- `dateOuverture` — déjà réglée sur le 8 novembre 2026 à minuit.

## Pour tester avant le 8

- `…/index.html?apercu` saute le compte à rebours, et à la question n'importe
  quelle réponse (même vide) passe.
- `…/index.html?reset` fait oublier la bonne réponse, pour revoir la question.
- `…/index.html?compte=20` montre le compte à rebours comme s’il restait 20 secondes
  (pour voir les 10 dernières secondes en grand), puis enchaîne sur la suite.
- `petitsMots` (dans les réglages) — les phrases qui défilent sous le compte à rebours.

## Mise en ligne

Prévu sur Vercel. Le dossier part tel quel, sans réglage.
`anniv-brigitte-originaux/` (à côté) garde la photo de profil d'origine et ne
doit pas être mis en ligne.

## Limites à connaître

- La question secrète protège l'ouverture, pas les fichiers : quelqu'un qui
  connaît l'adresse exacte d'une photo peut toujours l'ouvrir. La page est
  marquée pour ne pas apparaître dans Google.
- Si elle a activé « réduire les animations », tout s'affiche sans mouvement.
