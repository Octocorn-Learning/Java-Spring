# TP 4 : Fidéliser 💪

## Objectifs 🎯

Ce TP a pour objectif de pratiquer les API rest avec Spring Boot.
Il vous sera demandé de terminer les DTO concernant les entités Film, Acteurs, et Réalisateurs.

### Prérequis 📚

- Vous vous baserez sur la démo fil rouge cinéma.

- Vous devrez donc reprendre le code du TP précédent/démo et le modifier pour répondre aux exigences de ce TP.

### Rendu 📝

- Votre projet Cinema sera à rendre sur un dépot GitHub.

- Vous inclurez y README pour expliquer comment lancer votre projet.

### Evaluation 🚨

Vous serez évalué sur les points suivants :

- Qualité du README
- Qualité générale du code (découpage, documentation, bonnes pratiques, etc.)
- La complétion ou non de l'exercice
- La qualité des commits


## Consignes 📝

Vous devez fidéliser le backend déjà en place.

Vous trouverez pour chaque route, un exemple de body/réponse JSON attendue.
Il s'agit essentiellement de créer des DTO pour les entités Film, Acteurs et Réalisateurs.

### GET /acteurs

```json
[
  {
    "id": 1,
    "nom": "Ford",
    "prenom": "Harrison"
  },
  {
    "id": 2,
    "nom": "Fisher",
    "prenom": "Carrie"
  },
  {
    "id": 3,
    "nom": "Hamill",
    "prenom": "Mark"
  }
]
```

#### Aides 🐣

- Inspirez-vous de ce qui a déjà été fait pour les films sur `GET /films`

- Ce sera globalement le même concept, mais avec des acteurs

- Il s'agira de créer une DTO pour les acteurs, et de la retourner dans le controller

- La DTO ne doit pas contenir les films : juste le nom, prénom et ID

### GET /acteurs/{id}

```json
{
  "nom": "Ford",
  "prenom": "Harrison",
  "films": [
    {
      "titre": "Star Wars : Episode IV - Un nouvel espoir",
      "dateSortie": "1977-05-25",
      "realisateur": {
        "nom": "Lucas",
        "prenom": "George"
      }
    },
    {
      "titre": "Star Wars : Episode V - L'Empire contre-attaque",
      "dateSortie": "1980-05-21",
      "realisateur": {
        "nom": "Kershner",
        "prenom": "Irvin"
      }
    },
    {
      "titre": "Star Wars : Episode VI - Le Retour du Jedi",
      "dateSortie": "1983-05-25",
      "realisateur": {
        "nom": "Marquand",
        "prenom": "Richard"
      }
    },
    {
      "titre": "Star Wars : Episode VII - Le Réveil de la Force",
      "dateSortie": "2015-12-16",
      "realisateur": {
        "nom": "Abrams",
        "prenom": "J.J."
      }
    },
    {
      "titre": "Star Wars : Episode IX - L'Ascension de Skywalker",
      "dateSortie": "2019-12-18",
      "realisateur": {
        "nom": "Abrams",
        "prenom": "J.J."
      }
    }
  ]
}
```

#### Aides 🐣

- Une autre DTO ici, mais avec les films

- On retire les acteurs, mais on laisse le réalisateur

- Il y aura donc une DTO à créer pour les films sans acteurs

- Et une autre pour les acteurs réduits (avec films sans acteurs, avec réalisateur)

### GET /film/{id]/acteurs

```json
[
  {
    "nom": "Ford",
    "prenom": "Harrison"
  },
  {
    "nom": "Fisher",
    "prenom": "Carrie"
  },
  {
    "nom": "Hamill",
    "prenom": "Mark"
  }
]
```

#### Aides 🐣

- Ici il faudra créer une nouvelle route `GET /film/{id}/acteurs`

- N'oubliez pas d'utiliser l'annotation `@PathVariable` pour récupérer l'ID du film

- Il faudra réutiliser la DTO créée précédemment pour `GET /acteurs` : elle est identique.

- Il faudra créer une méthode dans le service qui ne retournera que les acteurs d'un film

- C'est cette méthode qui sera appelée par le controller.

- Il n'y aura rien à ajouter dans le repository

### GET /film/{id}/realisateur

```json
{
  "nom": "Lucas",
  "prenom": "George"
}
```

#### Aides 🐣

- Exactement la même chose que pour les acteurs, mais avec les réalisateurs

### GET /realisateurs/{id}

```json
{
  "nom": "Lucas",
  "prenom": "George",
  "films": [
    {
      "titre": "Star Wars : Episode IV - Un nouvel espoir",
      "dateSortie": "1977-05-25"
    }
  ]
}
```

#### Aides 🐣

- Cette fois, c'est sur le controller des réalisateur que nous faisons une modification

- Seulement, les films sont stockés dans le film et non dans le réalisateur

- Il vous faudra donc créer une méthode dans le répository `FilmRepository` qui retourne les films d'un réalisateur

- Suivez la norme JPA pour créer la méthode : Il s'agit de **trouver les films par réalisateur id**

- Vous pourrez ensuite appeler cette méthode dans le service `FilmService`

- `FilmService` pourra ensuite être injecté dans `RealisateurService` pour récupérer les films, afin d'accéder à la méthode

- Vous pourrez ensuite retourner la DTO dans le controller `RealisateurController`

> Qui a dit que les services ne pouvaient pas s'appeler entre eux ? 🤔

### GET /realisateurs/{id}/films

```json
[
  {
    "titre": "Star Wars : Episode IV - Un nouvel espoir",
    "durée": 121,
    "dateSortie": "1977-05-25"
  }
]
```

#### Aides 🐣

- Ici, vous pourrez réutiliser la méthode créée précédemment dans le repository `FilmRepository`

- Vous devrez ensuite utiliser une DTO qui ne retourne que le titre et la date de sortie

- Et enfin, vous pourrez retourner la DTO dans le controller `RealisateurController`

### POST /film/{id}/acteurs

Permet d'ajouter un acteur existant à un film.

```json
{
  "id": 1,
  "nom": "Ford",
  "prenom": "Harrison"
}
```

Si la création s'est bien passée, vous pourez retourner : 

```json
{
    "id": 1,
    "titre": "Star Wars : Episode IV - Un nouvel espoir",
    "dateSortie": "1977-05-25",
    "acteurs": [
        {
        "id": 1,
        "nom": "Ford",
        "prenom": "Harrison"
        }
    ],
    "realisateur": {
        "id": 1,
        "nom": "Lucas",
        "prenom": "George"
    }
}
```

#### Aides 🐣

- Il y aura plusieurs choses à faire ici.

- Déjà, il faudra injecter `ActeurService` dans `FilmService` pour pouvoir appeler la méthode `findById` de `ActeurService`

- Il vous faudra ensuite créer une méthode dans `FilmService` qui ajoute un acteur à un film

- Pour ensuite sauvegarder le film

- Vous pourrez ensuite retourner la DTO `FilmCompletDTO` dans le controller `FilmController`
