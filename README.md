# 🎁 Cadeaux de la famille

Liste de cadeaux partagée, sur une seule page, pensée pour le téléphone.
Sans compte : chacun tape son prénom une fois, puis peut réserver, libérer, ajouter ou supprimer des idées, en temps réel.

| Fichier | Rôle |
|---|---|
| `index.html` | Le site (HTML/CSS/JS, sans framework) |
| `firebase-config.js` | La configuration de ton projet Firebase (à remplir) |
| `database.rules.json` | Les règles de sécurité à coller dans Firebase |
| `seed.html` + `seed.json` | Import unique des données de départ |
| `img/` | Photos des articles (facultatif) |

---

## 1. Créer le projet Firebase (depuis le téléphone)

1. Ouvre **https://console.firebase.google.com** dans Chrome ou Safari et connecte-toi avec ton compte Google.
2. Appuie sur **Créer un projet** (ou « Commencer avec un projet Firebase »).
   - Nom : `cadeaux-famille` → **Continuer**.
   - Désactive **Google Analytics** (inutile ici) → **Créer le projet** → **Continuer**.
   - Le projet est automatiquement sur le **forfait Spark (gratuit)** : aucune carte bancaire n'est demandée. Ne clique pas sur « Passer au forfait supérieur ».
3. Ouvre le menu **☰** (en haut à gauche) → **Créer** (ou *Build*) → **Realtime Database** → **Créer une base de données**.
   - Emplacement : **Belgique (europe-west1)** → **Suivant**.
   - Choisis **Démarrer en mode verrouillé** → **Activer**.
     (Pas le « mode test » : il expire au bout de 30 jours. On met nos propres règles juste après.)

## 2. Coller les règles de sécurité

1. Toujours dans **Realtime Database**, ouvre l'onglet **Règles**.
2. Efface tout le texte de l'éditeur et colle **tout le contenu** de [`database.rules.json`](database.rules.json).
3. Appuie sur **Publier**.

Ce que font ces règles :
- lecture et écriture **uniquement** dans `/people` et `/items` (tout le reste est refusé) ;
- impossible d'écrire ou d'effacer la racine, `/people` ou `/items` d'un seul coup : on ne peut toucher qu'**une personne ou une idée à la fois** ;
- personne ne peut supprimer une personne (seulement en ajouter) ;
- chaque champ est vérifié : prénom ≤ 40 caractères, intitulé ≤ 120, note ≤ 300, lien `http(s)://` ≤ 500, prix entre 0 et 100 000, groupe `enfant` ou `adulte`, aucun champ inconnu, et une idée doit appartenir à une personne existante.

> Limite à connaître : comme tout le monde a le droit de supprimer une idée, quelqu'un de mal intentionné qui a le lien pourrait les supprimer une par une avec un script (les règles Firebase ne savent pas compter les suppressions). Pour une liste familiale ce risque est faible ; pense juste à faire de temps en temps une sauvegarde : Realtime Database → **⋮** → **Exporter le fichier JSON**.

## 3. Récupérer la configuration

1. Appuie sur la **roue ⚙️** à côté de « Vue d'ensemble du projet » → **Paramètres du projet**.
2. En bas, section **Vos applications**, appuie sur l'icône **Web `</>`**.
3. Surnom : `site` (ne coche pas Firebase Hosting) → **Enregistrer l'application**.
4. Firebase affiche un bloc `const firebaseConfig = { ... }`. Copie les valeurs.
   - Vérifie qu'il y a bien une ligne `databaseURL` qui se termine par `europe-west1.firebasedatabase.app`. Si elle manque, copie l'adresse affichée en haut de la page **Realtime Database → Données** (elle ressemble à `https://cadeaux-famille-xxxxx-default-rtdb.europe-west1.firebasedatabase.app`).

Ces valeurs ne sont **pas secrètes** (elles sont visibles par n'importe quel visiteur du site) : c'est normal, la protection vient des règles.

## 4. Mettre la configuration dans le site

Sur GitHub (dans le navigateur du téléphone) : ouvre le dépôt → `firebase-config.js` → icône **✏️** → remplace chaque `A_REMPLACER` par tes valeurs → **Commit changes**.

## 5. Importer les données de départ (une seule fois)

Ouvre **https://fbbbrand.github.io/cadeaux-famille/seed.html** et appuie sur **Importer les données**.
La page refuse d'importer si la base contient déjà des personnes (pour ne rien écraser).

Autre possibilité : dans la console, Realtime Database → **⋮** → **Importer le fichier JSON** → `seed.json`.
⚠️ Cette méthode **remplace toute la base** : à faire uniquement quand elle est vide.

## 6. Photos (facultatif)

Une idée peut avoir une photo : mets un fichier dans `img/` (par ex. `img/kurio.jpg`, environ 300 px) puis, dans la console Firebase, renseigne le champ `img` de l'idée avec `img/kurio.jpg`. Sans photo, l'idée s'affiche normalement.
