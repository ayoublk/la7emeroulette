# La 7ème roulette

Site statique pour GitHub Pages, connecté au projet Firebase `la7emeroulette`.
Le CSV fourni contient 601 entrées, sans colonne film. Les libellés sont conservés.

## Configuration Firebase

1. Créer une base **Cloud Firestore**, avec son identifiant `(default)` (ce site n'utilise pas Realtime Database).
2. Dans Authentication → Sign-in method, activer Google et renseigner l'adresse de support.
3. Après une connexion Google au site, relever l’UID du compte administrateur dans Authentication → Users. Dans Firestore, créer `admins/{UID}` avec le champ booléen `enabled: true`. Seule la console ou un outil privilégié peut attribuer ce rôle.
4. Dans Firestore → Règles, copier le contenu de `firestore.rules` et publier. Si la CLI Firebase est connectée : `firebase deploy --only firestore:rules --project la7emeroulette` depuis ce dossier.
5. Dans Authentication → Settings → Authorized domains, ajouter `ayoublk.github.io` (le domaine seul, sans chemin).

Les règles permettent à tous de lire. Seul l’administrateur peut tirer un personnage, importer et remettre les tirages en jeu. L’historique reste partagé entre tous les visiteurs.

## Import des 601 entrées

Ouvrir le site via HTTP, puis Menu → Connexion admin → Ajouter (CSV) → Ajouter à la liste.
Le CSV fourni est déjà chargé dans le champ. L'import s'effectue par lots de 400 et ignore les identifiants déjà présents ; il peut être relancé après une interruption sans remettre les personnages tirés en jeu.
`characters.json` est une conversion lisible de contrôle, pas un export Firestore à importer dans la console.

## GitHub Pages

Créer le dépôt public `ayoublk/la7emeroulette` et déposer les fichiers de ce dossier à sa racine.
Dans Settings → Pages, choisir Deploy from a branch → main → /(root) → Save.
L'adresse attendue est https://ayoublk.github.io/la7emeroulette/.
Le fichier `.nojekyll` désactive le traitement Jekyll.

## Vérification après publication

La page doit annoncer 601 personnages avant le premier tirage. Tester un tirage : le compteur doit diminuer et le personnage doit figurer dans Déjà tirés sur une seconde fenêtre. Vérifier que l'import et la remise en jeu sont désactivés sans le compte administrateur, puis remettre le personnage de test en jeu avec ce compte.

Documentation :
- https://firebase.google.com/docs/firestore/security/rules-fields
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
