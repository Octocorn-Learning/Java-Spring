---
title: Spring boot - tests
theme: solarized
author: Alexandre Devos
company: Octocorn
contributors: 
  - Alexandre Devos
sources:
  - 
---

# Spring Boot

## Tests

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Tests

### Pourquoi tester ?

- Comme pour tous les projets, il est important de tester son code

- Ils permettent de s'assurer que le code fonctionne correctement, et ne régresse pas

----

## Tests

### Spring Tests

- Spring met à notre disposition un certain nombre d'outils pour tester notre code

- Ces derniers sont presque "clef en main", et permettent de tester les couches de notre application

- Il est possible de tester les controllers, les services, les repositories, etc.

----

## Tests

### Dépendances

- JUnit et Mockito sont déjà inclus dans le starter `spring-boot-starter-test`

- C'est celui que nous avons installé au début du cours

- Il contient déjà toutes les annotations spécifiques à Spring pour les tests !

---

# Tests

## Tester le Service

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Tests

### Tester le Service

- Si on ne doit tester qu'une seule couche, c'est celle-ci

- C'est elle qui contient la logique métier de notre application, c'est donc la plus importante

- On testera les méthodes publiques du service

----

## Tests

### Annotations

- `@SpringBootTest` : Indique que la classe est un test Spring Boot

- `@MockBean` : Indique que l'attribut est un mock 

- `@Mock` : pour mocker une classe

- Et toutes les autres annotations de base de JUnit/Mockito !

----

## Tests

### Préparation

- Pour nous faciliter la vie, nous allons légèrement modifier notre entité

- Nous allons ajouter l'annotation `@Builder` de Lombok

- Cette annotation permet de créer un builder pour notre entité

- Cela nous permettra de créer des instances de notre entité plus facilement

----

## Tests

### Préparation

```java [9]

@Entity
@Getter
@Setter
@NoArgsConstructor
@Table(name = "film")
@JsonIdentityInfo(generator = ObjectIdGenerators.PropertyGenerator.class, property = "id")
@AllArgsConstructor
@Builder
public class Film {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id;

    @Column(nullable = false)
    private String titre;

    @Column(nullable = false)
    private LocalDate dateSortie;

    @Column(nullable = false)
    private int duree;

    @Column(length = 500)
    private String synopsis;

    @JoinColumn(name = "realisateur_id")
    @ManyToOne
    private Realisateur realisateur;

    @ManyToMany
    @JoinTable(
            // Nom de la table de liaison
            name = "acteur_film",
            // Spécifie le nom de la colonne de liaison (FK) pour la table courante
            joinColumns = @JoinColumn(name = "film_id"),
            // Spécifie le nom de la colonne de liaison (FK) pour la table liée
            inverseJoinColumns = @JoinColumn(name = "acteur_id")
    )
    private List<Acteur> acteurs = new ArrayList<>();
}
```

----

## Tests

### Préparation

Exemple d'utilisation du builder :

```java
Film film = Film.builder()
    .titre("Star Wars : Episode IV - Un nouvel espoir")
    .dateSortie(LocalDate.of(1977, 5, 25))
    .duree(121)
    .synopsis("Il y a bien longtemps, dans une galaxie très lointaine...")
    .build();
```

----

## Tests

### Création de la classe

```java [0]

// Indique que la classe est un test Spring Boot
@SpringBootTest
class FilmServiceTest {

    // Mock de la classe FilmRepository
    @Mock
    private FilmRepository filmRepository;

    // Injection dans le service du mock de FilmRepository
    private FilmService filmService;
    
    @BeforeEach
    void setUp() {
        // Création du service avec le mock de FilmRepository
        filmService = new FilmService(filmRepository);
    }

}
```

----

## Tests

### Création d'un jeu de données

```java [13-45]

@SpringBootTest
class FilmServiceTest {

    @Mock
    private FilmRepository filmRepository;
    
    private FilmService filmService;

    private List<Film> films;

    @BeforeEach
    void setUp() {
        filmService = new FilmService(filmRepository, acteurService);
        Film.builder()
                .id(1)
                .titre("Star Wars : Episode IV - Un nouvel espoir")
                .dateSortie(LocalDate.of(1977, 5, 25))
                .duree(121)
                .synopsis("")
                .build();
        Film.builder()
                .id(2)
                .titre("Star Wars : Episode V - L'Empire contre-attaque")
                .dateSortie(LocalDate.of(1980, 5, 21))
                .duree(124)
                .synopsis("")
                .build();
        Film.builder()
                .id(3)
                .titre("Star Wars : Episode VI - Le Retour du Jedi")
                .dateSortie(LocalDate.of(1983, 5, 25))
                .duree(131)
                .synopsis("")
                .build();
        
        // Ajout des films dans la liste
        films = List.of(
                premierFilm,
                deuxiemeFilm,
                troisiemeFilm
            );
        // On indique à Mockito comment réagir lorsqu'on appelle la méthode findAll
        when(filmRepository.findAll()).thenReturn(films);
    }
    
}
```

----

## Tests

### findAll

```java [45-51]

@SpringBootTest
class FilmServiceTest {

    @Mock
    private FilmRepository filmRepository;
    
    private FilmService filmService;

    private List<Film> films;

    @BeforeEach
    void setUp() {
        filmService = new FilmService(filmRepository, acteurService);
        Film.builder()
                .id(1)
                .titre("Star Wars : Episode IV - Un nouvel espoir")
                .dateSortie(LocalDate.of(1977, 5, 25))
                .duree(121)
                .synopsis("")
                .build();
        Film.builder()
                .id(2)
                .titre("Star Wars : Episode V - L'Empire contre-attaque")
                .dateSortie(LocalDate.of(1980, 5, 21))
                .duree(124)
                .synopsis("")
                .build();
        Film.builder()
                .id(3)
                .titre("Star Wars : Episode VI - Le Retour du Jedi")
                .dateSortie(LocalDate.of(1983, 5, 25))
                .duree(131)
                .synopsis("")
                .build();
        
        films = List.of(
                premierFilm,
                deuxiemeFilm,
                troisiemeFilm
            );
        when(filmRepository.findAll()).thenReturn(films);
    }

    @Test
    void devraitRetournerTousLesFilms() {
        // On appelle la méthode à tester
        List<Film> filmsTrouves = filmService.findAll();
        // On vérifie que le résultat est bien celui attendu
        assertEquals(films, filmsTrouves);
    }
}
```

----

## Tests

### findById

```java [51-59]

@SpringBootTest
class FilmServiceTest {

    @MockBean
    private FilmRepository filmRepository;
    
    private FilmService filmService;

    private List<Film> films;

    @BeforeEach
    void setUp() {
        filmService = new FilmService(filmRepository, acteurService);
        Film.builder()
                .id(1)
                .titre("Star Wars : Episode IV - Un nouvel espoir")
                .dateSortie(LocalDate.of(1977, 5, 25))
                .duree(121)
                .synopsis("")
                .build();
        Film.builder()
                .id(2)
                .titre("Star Wars : Episode V - L'Empire contre-attaque")
                .dateSortie(LocalDate.of(1980, 5, 21))
                .duree(124)
                .synopsis("")
                .build();
        Film.builder()
                .id(3)
                .titre("Star Wars : Episode VI - Le Retour du Jedi")
                .dateSortie(LocalDate.of(1983, 5, 25))
                .duree(131)
                .synopsis("")
                .build();
        
        films = List.of(
                premierFilm,
                deuxiemeFilm,
                troisiemeFilm
            );
        when(filmRepository.findAll()).thenReturn(films);
    }

    @Test
    void devraitRetournerTousLesFilms() {
        List<Film> filmsTrouves = filmService.findAll();
        assertEquals(films, filmsTrouves);
    }

    @Test
    void devraitRetournerLeFilmDemande() {
        // On indique à Mockito comment réagir lorsqu'on appelle la méthode findById
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));
        // On appel la méthode à tester
        Film film = filmService.findById(1);
        // Et on vérifie que le résultat est bien celui attendu
        assertEquals(films.get(0), film);
    }
}
```

----

## Tests

### Contrôler les données

```java [58-73]

@SpringBootTest
class FilmServiceTest {

    @Mock
    private FilmRepository filmRepository;

    private FilmService filmService;

    private List<Film> films;

    @BeforeEach
    void setUp() {
        filmService = new FilmService(filmRepository, acteurService);
        Film.builder()
                .id(1)
                .titre("Star Wars : Episode IV - Un nouvel espoir")
                .dateSortie(LocalDate.of(1977, 5, 25))
                .duree(121)
                .synopsis("")
                .build();
        Film.builder()
                .id(2)
                .titre("Star Wars : Episode V - L'Empire contre-attaque")
                .dateSortie(LocalDate.of(1980, 5, 21))
                .duree(124)
                .synopsis("")
                .build();
        Film.builder()
                .id(3)
                .titre("Star Wars : Episode VI - Le Retour du Jedi")
                .dateSortie(LocalDate.of(1983, 5, 25))
                .duree(131)
                .synopsis("")
                .build();
        
        films = List.of(
                premierFilm,
                deuxiemeFilm,
                troisiemeFilm
            );
        when(filmRepository.findAll()).thenReturn(films);
    }

    @Test
    void devraitRetournerTousLesFilms() {
        List<Film> filmsTrouves = filmService.findAll();
        assertEquals(films, filmsTrouves);
    }

    @Test
    void devraitRetournerLeFilmDemande() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));
        Film film = filmService.findById(1);
        assertEquals(films.get(0), film);
    }

    @Test
    void leFilmDevraitContenirToutesLesInformations() {
        // On récupère le premier film de la liste
        Film film = films.get(0);
        
        // Et on compare les informations
        assertEquals("Star Wars : Episode IV - Un nouvel espoir", film.getTitre());
        assertEquals(121, film.getDuree());
        assertEquals(LocalDate.of(1977, 5, 25), film.getDateSortie());
        assertEquals(realisateur, film.getRealisateur());
        assertEquals(List.of(acteur), film.getActeurs());
        assertEquals(1, film.getId());
        assertEquals("", film.getSynopsis());
        // Ce qui assure que le film a bien la forme attendue
        // Ca permet également de passer les tests de la classe à 100%
    }
}
```

----

## Tests

### Pour le rest ...

Rien de particulier comparé à des tests habituels !

```java [0]

@SpringBootTest
class FilmServiceTest {

    @Mock
    FilmRepository filmRepository;

    @Mock
    ActeurService acteurService;
    
    FilmService filmService;

    List<Film> films;

    @Mock
    Realisateur realisateur;

    @Mock
    Acteur acteur;

    @BeforeEach
    void setUp() {
        
        filmService = new FilmService(filmRepository, acteurService);
        Film premierFilm = Film.builder()
                .id(1)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film deuxiemeFilm = Film.builder()
                .id(2)
                .titre("Le seigneur des anneaux 2")
                .duree(120)
                .dateSortie(LocalDate.of(2002, 12, 18))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film troisiemeFilm = Film.builder()
                .id(3)
                .titre("Le seigneur des anneaux 3")
                .duree(120)
                .dateSortie(LocalDate.of(2003, 12, 17))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        films = List.of(
                premierFilm,
                deuxiemeFilm,
                troisiemeFilm
        );

        when(filmRepository.findAll()).thenReturn(films);
    }

    @Test
    void devraitRetournerTousLesFilms() {
        List<Film> films = filmService.findAll();
        assertEquals(3, films.size());
    }

    @Test
    void devraitRetournerLeFilmDemande() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));
        Film film = filmService.findById(1);
        assertEquals(films.get(0), film);
    }

    @Test
    void devraitLeverUneExceptionSiLeFilmNExistePas() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.empty());
        assertThrows(FilmNotFoundException.class, () -> filmService.findById(1));
    }

    @Test
    void devraitSupprimerLeFilmDemande() {
        int filmId = 1;
        when(filmRepository.findById(filmId)).thenReturn(java.util.Optional.of(films.get(0)));

        filmService.deleteById(filmId);

        verify(filmRepository, times(1)).findById(filmId);
        verify(filmRepository, times(1)).deleteById(filmId);
    }

    @Test
    void devraitLeverUneExceptionSiLeFilmNExistePasLorsDeLaSuppression() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.empty());
        assertThrows(FilmNotFoundException.class, () -> filmService.deleteById(1));
    }

    @Test
    void leFilmDevraitContenirToutesLesInformations() {
        Film film = films.get(0);
        assertEquals("Le seigneur des anneaux", film.getTitre());
        assertEquals(120, film.getDuree());
        assertEquals(LocalDate.of(2001, 12, 19), film.getDateSortie());
        assertEquals(realisateur, film.getRealisateur());
        assertEquals(List.of(acteur), film.getActeurs());
        assertEquals(1, film.getId());
        assertEquals("", film.getSynopsis());
    }

    @Test
    void devraitSauvegarderLeFilm() {
        Film nouveauFilm = Film.builder()
                .titre("Star Wars : La menace fantôme")
                .duree(120)
                .dateSortie(LocalDate.of(1999, 5, 19))
                .realisateur(realisateur)
                .acteurs(List.of(acteur))
                .synopsis("")
                .id(1)
                .build();

        when(filmRepository.save(nouveauFilm)).thenReturn(nouveauFilm);

        Film filmSauvegarde = filmService.save(nouveauFilm);

        assertEquals(nouveauFilm, filmSauvegarde);
    }

    @ParameterizedTest
    @MethodSource("generatEachError")
    void devraitLeverUneExceptionSiManqueDesChamps(Film film) {
        when(filmRepository.save(any())).thenReturn(film);

        assertThrows(BadRequestException.class, () -> filmService.save(film));
    }

    public static Stream<Arguments> generatEachError() {
        return Stream.of(
                Arguments.of(new Film()),
                Arguments.of(Film.builder().titre("Le seigneur des anneaux").build())
        );
    }

    @Test
    void devraitMettreAJourLeFilm() {
        Film film = films.get(0);
        film.setTitre("Le seigneur des anneaux : La communauté de l'anneau");
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));

        when(filmRepository.save(film)).thenReturn(film);

        Film filmMisAJour = filmService.update(film, 1);
        assertEquals(film, filmMisAJour);
    }

    @Test
    void devraitRetournerLeFilmParTitre() {
        when(filmRepository.findByTitre("Le seigneur des anneaux")).thenReturn(java.util.Optional.of(films.get(0)));
        Film film = filmService.findByTitre("Le seigneur des anneaux");
        assertNotNull(film);
    }

    @Test
    void devraitRetournerUneErreurSiLeFilmNExistePas() {
        when(filmRepository.findByTitre(anyString())).thenReturn(java.util.Optional.empty());
        assertThrows(ResponseStatusException.class, () -> filmService.findByTitre("Le seigneur des anneaux"));
    }

    @Test
    void devraitRetournerLesActeursDuFilm() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));
        List<Acteur> acteurs = filmService.findActeursByFilmId(1);
        assertNotNull(acteurs);
    }

    @Test
    void devraitRetournerLeRealisateurDuFilm() {
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(films.get(0)));
        Realisateur realisateur = filmService.findRealisateurByFilmId(1);
        assertEquals(this.realisateur, realisateur);
    }

    @Test
    void devraitRetournerLesFilmsParRealisateurId() {
        when(filmRepository.findByRealisateurId(1)).thenReturn(films);
        List<Film> films = filmService.findByRealisateurId(1);
        assertEquals(3, films.size());
    }

    @Test
    void devraitAjouterUnActeurAuFilm() {
        Film film = films.get(0);
        Acteur nouvelActeur = new Acteur();
        when(filmRepository.findById(1)).thenReturn(java.util.Optional.of(film));
        when(acteurService.findById(1)).thenReturn(nouvelActeur);
        when(filmRepository.save(film)).thenReturn(film);

        Film filmMisAJour = filmService.addActeur(1, nouvelActeur);
        assertTrue(filmMisAJour.getActeurs().contains(nouvelActeur));
    }
}
```

---

# Tests

## Tester le Controller

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Tests

### Tester le Controller

- Spring met à notre disposition un outil pour tester les controllers

- Il s'agit de `MockMvc`

- Cet outil permet de simuler des requêtes HTTP sur nos endpoints

----

## Tests

### Annotations

- `@WebMvcTest` : Indique que la classe est un test Spring Boot pour un controller

- `@MockBean` : Indique que l'attribut est un mock

- `@Autowired` : Permet d'injecter des beans dans le controller

----

## MockMvc

Dispose de plusieurs méthodes :
- `perform` : Permet de simuler une requête HTTP
- `andExpect` : Permet de vérifier le résultat de la requête (assertion)
- `andDo` : Permet d'afficher le résultat de la requête dans la console

----

## MockMvc

### Perform

`
mockMvc.perform(get("/films"))
`

- Prend en paramètre la requête à simuler

- Retourne un objet `ResultActions`

- Cet objet permet de vérifier le résultat de la requête

----

## MockMvc

### Perform

- `get` : Simule une requête HTTP GET

- `post` : Simule une requête HTTP POST

- `put` : Simule une requête HTTP PUT

- `delete` : Simule une requête HTTP DELETE

----

## MockMvc

### andExpect

`
mockMvc.perform(get("/films"))
    .andExpect(status().isOk())
    .andExpect(jsonPath("$", hasSize(3)))
`

- `status` : vérifie le code de retour HTTP

- `jsonPath` : vérifie le contenu d'un JSON (`$` représente le JSON)

- `content` : vérifier le contenu de la réponse

----

## MockMvc

### andDo

- `print` : Affiche le résultat de la requête dans la console

- `log` : Affiche le résultat de la requête dans les logs

- `debug` : Affiche le résultat de la requête dans les logs en mode debug

> ⚠️ Attention, ces méthodes ne sont pas des assertions ! ⚠️

----

## Tests

### Création de la classe

```java [0]

@WebMvcTest(FilmController.class)
class FilmControllerTest {
    // injecter le MockMvc
    @Autowired
    MockMvc mockMvc;

    // injecter le service
    @MockBean
    FilmService filmService;

    // Création d'une liste de films
    // pour contenir un jeu de data contrôlé
    List<Film> films;

    // Création de mocks pour le réalisateur et l'acteur
    @Mock
    Realisateur realisateur;
    @Mock
    Acteur acteur;

    @BeforeEach
    void setUp() {
        // Génération du jeu de data contrôlé
        Film premierFilm = Film.builder()
                .id(1)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film deuxiemeFilm = Film.builder()
                .id(2)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        films = new ArrayList<>(List.of(premierFilm, deuxiemeFilm));

        // Définition du comportement des mocks (service)
        when(filmService.findAll()).thenReturn(films);
        when(filmService.findById(1)).thenReturn(premierFilm);
        when(filmService.findById(2)).thenReturn(deuxiemeFilm);
        when(filmService.save(any(Film.class)))
                .thenAnswer(invocation -> invocation.getArgument(0));
    }
}
```

----

## Tests

### findAll

```java [54-67]

@WebMvcTest(FilmController.class)
class FilmControllerTest {
    // injecter le MockMvc
    @Autowired
    MockMvc mockMvc;

    // injecter le service
    @MockBean
    FilmService filmService;

    // Création d'une liste de films
    // pour contenir un jeu de data contrôlé
    List<Film> films;

    // Création de mocks pour le réalisateur et l'acteur
    @Mock
    Realisateur realisateur;
    @Mock
    Acteur acteur;

    @BeforeEach
    void setUp() {
        // Génération du jeu de data contrôlé
        Film premierFilm = Film.builder()
                .id(1)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film deuxiemeFilm = Film.builder()
                .id(2)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        films = new ArrayList<>(List.of(premierFilm, deuxiemeFilm));

        // Définition du comportement des mocks (service)
        when(filmService.findAll()).thenReturn(films);
        when(filmService.findById(1)).thenReturn(premierFilm);
        when(filmService.findById(2)).thenReturn(deuxiemeFilm);
        when(filmService.save(any(Film.class))).thenAnswer(invocation -> invocation.getArgument(0));
    }

    @Test
    public void leNombreDeFilmsRecuperesEstCorrect() throws Exception {
                // Requête de type GET sur l'URL /films
        mockMvc.perform(get("/films"))
                // Vérification du code de retour HTTP
                .andExpect(status().isOk())
                // Vérification du nombre de films retournés
                .andExpect(
                        // jsonPath permet de vérifier le contenu d'un JSON ($)
                        jsonPath("$",
                                // hasSize permet de vérifier la taille d'une liste
                                hasSize(films.size())
                        )
                );
    }

}
```

----

## Tests

### findById

```java [59-68]

@WebMvcTest(FilmController.class)
class FilmControllerTest {
    @Autowired
    MockMvc mockMvc;
    
    @MockBean
    FilmService filmService;
    
    List<Film> films;

    @Mock
    Realisateur realisateur;
    @Mock
    Acteur acteur;

    @BeforeEach
    void setUp() {
        Film premierFilm = Film.builder()
                .id(1)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film deuxiemeFilm = Film.builder()
                .id(2)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        films = new ArrayList<>(List.of(premierFilm, deuxiemeFilm));

        when(filmService.findAll()).thenReturn(films);
        when(filmService.findById(1)).thenReturn(premierFilm);
        when(filmService.findById(2)).thenReturn(deuxiemeFilm);
        when(filmService.save(any(Film.class))).thenAnswer(invocation -> invocation.getArgument(0));
    }

    @Test
    public void leNombreDeFilmsRecuperesEstCorrect() throws Exception {
        mockMvc
                .perform(get("/films"))
                .andExpect(status().isOk())
                .andExpect(
                        jsonPath("$",
                                hasSize(films.size())
                        )
                );
    }

    @Test
    public void leFilmDemandeEstRetourne() throws Exception {
        mockMvc
                .perform(get("/films/1"))
                .andExpect(status().isOk())
                .andExpect(
                        // Compare l'attribut "id" du JSON avec la valeur 1
                        jsonPath("$.id").value(1)
                );
    }

}
```

----

## Tests

### create

```java [17-19 | 73-92]

@WebMvcTest(FilmController.class)
class FilmControllerTest {
    @Autowired
    MockMvc mockMvc;
    
    @MockBean
    FilmService filmService;
    
    List<Film> films;
    
    @Mock
    Realisateur realisateur;
    @Mock
    Acteur acteur;

    // Injection de l'objet ObjectMapper
    @Autowired
    private ObjectMapper objectMapper;

    @BeforeEach
    void setUp() {
        Film premierFilm = Film.builder()
                .id(1)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        Film deuxiemeFilm = Film.builder()
                .id(2)
                .titre("Le seigneur des anneaux")
                .duree(120)
                .dateSortie(LocalDate.of(2001, 12, 19))
                .realisateur(realisateur)
                .acteurs(new ArrayList<>(List.of(acteur)))
                .synopsis("")
                .build();

        films = new ArrayList<>(List.of(premierFilm, deuxiemeFilm));

        when(filmService.findAll()).thenReturn(films);
        when(filmService.findById(1)).thenReturn(premierFilm);
        when(filmService.findById(2)).thenReturn(deuxiemeFilm);
        when(filmService.save(any(Film.class))).thenAnswer(invocation -> invocation.getArgument(0));
    }

    @Test
    public void leNombreDeFilmsRecuperesEstCorrect() throws Exception {
        mockMvc
                .perform(get("/films"))
                .andExpect(status().isOk())
                .andExpect(
                        jsonPath("$",
                                hasSize(films.size())
                        )
                );
    }

    @Test
    public void leFilmDemandeEstRetourne() throws Exception {
        mockMvc
                .perform(get("/films/1"))
                .andExpect(status().isOk())
                .andExpect(
                        jsonPath("$.id").value(1)
                );
    }

    @Test
    public void siJAjouteUnFilmIlEstBienAjoute() throws Exception {
        // Création d'un nouveau film
        Film newFilm = Film.builder()
                        .titre("Le seigneur des anneaux")
                        .duree(120)
                        .dateSortie(LocalDate.of(2001, 12, 19))
                        .realisateur(realisateur)
                        .acteurs(new ArrayList<>(List.of(acteur)))
                        .synopsis("")
                        .build();
        mockMvc.perform(post("/films")
                        // POST d'un json
                        .contentType("application/json")
                        // Contenu du json (nouveau film converti en json)
                        .content(objectMapper.writeValueAsString(newFilm))
                )
                // Réponse attendue : 200
                .andExpect(status().isOk());
    }
}
```

----

## Tests

### A vous de jouer !

Testez votre application !

---

# Spring Boot Test

C'est la fin de ce cours ! 

N'hésite pas à mettre une étoile sur le repo si tu as aimé !