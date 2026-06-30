# Migration du site AFME Paris 13

## Contexte

Le site actuel est hébergé sur **Google Sites** :
`https://sites.google.com/view/afmeparis13/accueil`

Objectif : reprendre la **structure de navigation** (pas le contenu protégé tel quel,
qui devra être ressaisi par l'équipe AFME) pour construire un nouveau site,
plus maintenable, en 3 étapes.

## Roadmap

### Étape 1 — Site statique (GitHub Pages)
- Reproduire l'arborescence de navigation (voir `STRUCTURE.md`)
- Stack proposée : HTML/CSS simple, ou générateur statique (Astro / Eleventy / Hugo)
  si vous voulez des pages générées depuis du contenu Markdown
- Hébergement : GitHub Pages (gratuit, branché sur le repo)

### Étape 2 — Contenu dynamique (Firebase)
- **Firestore** : stockage des contenus de cours (thèmes, fiches, écrits blancs)
  plutôt que des fichiers statiques, pour permettre des mises à jour sans
  redéploiement
- **Firebase Storage** : hébergement des PDF, images, documents (rapports de jury,
  répartition des tuteurs, etc.)
- **Firebase Authentication** : pour gérer l'accès aux ressources réservées
  aux adhérents à jour de cotisation (remplace le système "payez pour accéder"
  actuel)
- Le site GitHub Pages consomme les données Firebase via le SDK JS (front
  uniquement, pas besoin de backend serveur dans un premier temps)

### Étape 3 — Application à part entière
- Une fois la base de contenu stabilisée sur Firebase, possibilité de créer
  une app mobile (Flutter / React Native) consommant la même base Firebase
- Nécessitera un compte développeur Apple (99$/an) et/ou Google Play
  (25$ unique) selon les plateformes ciblées
- Le calendrier Google Calendar actuellement embarqué peut être conservé via
  l'API Google Calendar, ou migré vers une gestion d'événements Firestore

## Arborescence du repo proposée

```
afme-site/
├── README.md
├── STRUCTURE.md              # arborescence des pages (voir fichier dédié)
├── index.html                # page d'accueil
├── /formation-master/
│   ├── index.html
│   ├── ecrit-1/
│   │   ├── index.html
│   │   ├── bases-methodologiques.html
│   │   ├── theme-1-transmettre-et-adapter.html
│   │   ├── theme-2-geste-et-effort.html
│   │   └── ecrits-blancs.html
│   ├── ecrit-2/
│   │   ├── index.html
│   │   ├── theme-1.html
│   │   ├── theme-2.html
│   │   ├── theme-3.html
│   │   └── ecrits-blancs.html
│   ├── oral-1.html
│   ├── oral-2.html
│   └── oral-3.html
├── /formation-licence/
│   ├── index.html
│   ├── ecrit-1/
│   │   ├── index.html
│   │   ├── bases-methodologiques.html
│   │   ├── theme-1-geste-et-effort.html
│   │   ├── theme-2-transmettre-et-adapter.html
│   │   └── ecrits-blancs.html
│   ├── ecrit-2/
│   │   ├── index.html
│   │   ├── bases-methodologiques.html
│   │   ├── theme-1.html
│   │   ├── theme-2.html
│   │   ├── theme-3.html
│   │   ├── theme-4.html
│   │   └── ecrits-blancs.html
│   └── oral-1.html
├── /formation-interne/
│   └── index.html
├── /ressources/
│   ├── index.html
│   ├── bibliotheque-numerique/
│   │   ├── index.html
│   │   ├── textes-officiels.html
│   │   ├── articles.html
│   │   └── copies-de-concours.html
│   ├── concours.html
│   └── contact.html
├── /staff/
│   └── index.html
├── /assets/
│   ├── css/
│   ├── js/
│   └── images/
└── /firebase/
    ├── firebase.json
    ├── firestore.rules
    └── storage.rules
```

## Points d'attention

- **Contenu** : le contenu pédagogique actuel (cours, fiches méthodologiques,
  copies, articles) est la propriété de l'AFME / de ses auteurs. Il doit être
  réimporté manuellement par les personnes habilitées, pas extrait
  automatiquement du Google Site.
- **Accès payant** : prévoir dès la conception Firebase comment gérer
  les adhérents à jour de cotisation (champ `statut_cotisation` dans le profil
  utilisateur Firestore, par ex.)
- **Calendriers** : les deux calendriers Google (Licence / Master) sont déjà
  embarquables tels quels en iframe en attendant une migration éventuelle
- **Nom de domaine** : si vous voulez un domaine custom (`afmeparis13.fr`)
  plutôt que `*.github.io`, c'est configurable sur GitHub Pages avec un
  enregistrement DNS CNAME

## Prochaines étapes concrètes

1. Créer le repo GitHub (`afme-paris13-site` par ex.)
2. Activer GitHub Pages sur la branche `main` ou `gh-pages`
3. Créer le projet Firebase, activer Firestore + Storage + Auth
4. Construire le squelette HTML selon `STRUCTURE.md`
5. Réintégrer le contenu réel page par page
