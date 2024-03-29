---
title: Spring Boot - Init, Controller, Services
theme: solarized
author: Alexandre Devos
company: Octocorn
contributors:
  - Alexandre Devos
sources:
  - 
---

# Spring Boot

## Création d'API REST

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Spring Boot

### Initialiser le projet

Pour créer une API rest, il faudra les dépendances suivantes :

- Spring Web

- Spring Data JPA

- Spring Boot DevTools (optionnel)

- Lombok (optionnel)

----

## Spring Boot

### Spring Web

- Permet de créer des API REST

- Basé sur Spring MVC

- Utilise Apache Tomcat par défaut en tant que conteneur

----

## Spring Boot

### Spring Web

- C'est grâce à Spring Web que l'on peut créer des API REST

- Elle permettra de build l'application et de l'exporter en tant que JAR

- On pourra ainsi exécuter des requêtes HTTP sur l'application

> Et le mieux : Tout est déjà configuré !

----

## Spring Boot

### Spring Data JPA

- Permet de créer la couche d'accès aux données

- Basé sur Spring Data et Hibernate

- Basé sur SQL, mais dispose de son équivalent MongoDB : Spring Data MongoDB

> Intègre le connecteur, et l'ORM tout configuré !

----

## Spring Boot

### Spring Boot DevTools

- Intègre le LiveReload

- Accélère le redémarrage de l'application en mode dev

> Un incontournable en développement !

----

## Spring Boot

### Créez un projet

- File > New > Project
- Sélectionnez Spring Initializr
- Choisissez un nom de projet
- Ajoutez :
    - Spring Web
    - Spring Boot DevTools
    - Lombok
- Cliquez sur Next

----

## Spring Boot

### Tour du propriétaire

```shell
└───src
    ├───main
    │   ├───java
    │   │   └───fr
    │   │       └───octocorn
    │   │           └───democoursapi
    │   │                   # Point d'entrée de l'app (Main)
    │   │                   DemoCoursApiApplication.java
    │   │
    │   └───resources
    │       │   # Fichier de configuration de l'app
    │       │   application.properties
    │       │ 
    │       ├───static
    │       └───templates
```

----

## Spring Boot

### `DemoCoursApiApplication.java`

- Point d'entrée de l'application

- Comparable à une classe `Main`

- Annoté avec `@SpringBootApplication`

----

## Spring Boot

### `@SpringBootApplication`

Annotation qui regroupe plusieurs annotations :

- `@EnableAutoConfiguration` : Active la configuration automatique de Spring Boot

- `@ComponentScan` : Active le scan des composants

- `@Configuration` : Indique que la classe est une configuration Spring

> Sans cette classe, nos beans ne seront pas créés, et l'application ne pourra pas démarrer !

----

## Spring Boot

### `application.properties`

- Fichier de configuration de l'application

- Peut être remplacé par un fichier `application.yml`

- On pourra lui ajouter des propriétés de configuration :
    - Port d'écoute
    - Base de données
    - Variables d'environnement
    - etc.

---

# Spring Boot

## Mon premier contrôleur

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Controller

### Démonstration

----

## Controller

### Créer un contrôleur

- Clic droit sur le package `fr.octocorn.democoursapi`

- New > Class

- Nommez la classe `helloworld.HelloController`

----

## Controller

### HelloController

Pour être identifié comme contrôleur, il faut ajouter l'annotation `@RestController` sur la classe.

```java [0]

@RestController
public class HelloController {
}
```

----

## Controller

### HelloController

Une annotation pour chaque type de requête :

- `@GetMapping` : GET
- `@PostMapping` : POST
- `@PutMapping` : PUT
- `@DeleteMapping` : DELETE
- `@PatchMapping` : PATCH

----

## Controller

### Annotations sur les méthodes

- Les annotations peuvent prendre un paramètre

- Il s'agira de l'URI de la requête

----

## Controller

### `@GetMapping`

```java [5]

@RestController
public class HelloController {

    @GetMapping("/hello") // GET http://localhost:8080/hello
    public String hello() {
        return "GET sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@PostMapping`

```java [5]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello
    public String helloPost() {
        return "POST sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@PutMapping`

```java [5]

@RestController
public class HelloController {

    @PutMapping("/hello") // PUT http://localhost:8080/hello
    public String helloPut() {
        return "PUT sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@DeleteMapping`

```java [5]

@RestController
public class HelloController {

    @DeleteMapping("/hello") // DELETE http://localhost:8080/hello
    public String helloDelete() {
        return "DELETE sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@PatchMapping`

```java [5]

@RestController
public class HelloController {

    @PatchMapping("/hello") // PATCH http://localhost:8080/hello
    public String helloPatch() {
        return "PATCH sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@RequestMapping`

- Permet de définir une URI pour plusieurs types de requêtes

- Prend en paramètre une chaîne de caractères ou un tableau de chaînes de caractères

- Peut être ajoutée sur la classe ou sur les méthodes

----

## Controller

### `@RequestMapping` sur la classe

```java [3]

@RestController
@RequestMapping("/hello")
public class HelloController {

    @GetMapping // GET http://localhost:8080/hello
    public String hello() {
        return "GET sur Hello World";
    }

    @PostMapping // POST http://localhost:8080/hello
    public String helloPost() {
        return "POST sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@RequestMapping` sur la méthode

```java [6]

@RestController
@RequestMapping("/hello")
public class HelloController {

    @RequestMapping(method = RequestMethod.POST) // POST http://localhost:8080/hello
    public String helloPost() {
        return "POST sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### `@RequestMapping` sur la méthode

```java [5]

@RestController
public class HelloController {

    @RequestMapping(value = "/hello", method = RequestMethod.POST) // POST http://localhost:8080/hello
    public String helloPost() {
        return "POST sur Hello World";
    }
}
```

> Buildez et testez avec Postman !

----

## Controller

### Récupérer des valeurs

- Dans une vraie application, les requêtes POST, PUT et PATCH contiendront des données

- Il existe deux moyen de les transmettre :
    - Via l'URI (chemin)
    - En paramètre de la requête (?name=Peter)
    - Via le corps de la requête (body)

- Des annotations permettent de récupérer ces valeurs

----

## Controller

### `{variable}` dans l'URI

- On peut définir des variables dans l'URI

- On le précisera en paramètre de l'annotation

- On pourra ensuite récupérer la valeur de la variable en paramètre de la méthode

----

## Controller

### `@PathVariable`

- Permet de récupérer une variable dans l'URI

- Cette annotation s'ajoute dans les paramètres de la méthode

- Elle est placée juste avant le type/nom de la variable

----

## Controller

### `@PathVariable`

```java [0 | 5-6]

@RestController
public class HelloController {

    @PostMapping("/hello/{name}") // POST http://localhost:8080/hello/Peter
    public String helloPost(@PathVariable String name) {
        return "Hello " + name + " !";
    }
}
```

----

## Controller

### `@RequestParam`

- Permet de récupérer une variable dans le corps de la requête

- Comme pour `@PathVariable`, elle s'ajoute dans les paramètres de la méthode

- Elle peut prendre en paramètre :
    - Une valeur par défaut
    - Si la variable est obligatoire ou non
    - Etc.

----

## Controller

### `@RequestParam`

```java [6]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello?name=Alex
    public String helloPost(@RequestParam String name) {
        return "Hello " + name + " !";
    }
}
```

----

## Controller

### `@RequestParam`

Avec paramètre facultatif :

```java [6]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello
    public String helloPost(@RequestParam(required = false) String name) {
        return "Hello " + name + " !";
    }
}
```

----

## Controller

### `@RequestParam`

Avec valeur par défaut :

```java [6]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello
    public String helloPost(@RequestParam(defaultValue = "Alex") String name) {
        return "Hello " + name + " !";
    }
}
```

----

## Controller

### `@RequestBody`

- Permet de récupérer le corps de la requête

- Comme pour `@PathVariable`, elle s'ajoute dans les paramètres de la méthode

----

## Controller

### `@RequestBody`

```java [6]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello
    public String helloPost(@RequestBody String name) {
        return "Hello " + name + " !";
    }
}
```

----

## Controller

### Bonnes pratiques

- La responsabilité d'un contrôleur est de gérer les requêtes HTTP.

- Il ne doit pas contenir de logiques métiers.

- Il doit être le plus léger possible.

----

## Controller

### Bonnes pratiques

Exemple de mauvais contrôleur :

```java [0 | 7-11]

@RestController
public class HelloController {

    @PostMapping("/hello") // POST http://localhost:8080/hello
    public String helloPost(@RequestBody String name) {
        if (name.equals("Alex")) {
            return "Hello Alex !";
        } else {
            return "Hello " + name + " !";
        }
    }
}
```

----

## Controller

### Bonnes pratiques

> Mais du coup, ou mettre la logique métier ?

----

## Controller

### Bonnes pratiques

- La logique métier doit être dans une couche service

- C'est une autre classe, qui ne sert qu'à ça !

- Dans la large majorité des cas, notre controller ne fera que transmettre les données à la couche service !

- La couche service retournera les données au controller, qui les transmettra à l'utilisateur !

> La couche service sera abordée dans le prochain chapitre !

----

## Controller

### Bonnes pratiques

- Il existe bien d'autres bonnes pratiques concernant les controllers

- Ceux-ci seront abordés dans les prochains chapitres !

- En effet, il nous reste encore des notions importantes à découvrir !

---

# Spring Boot

## Les Services

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## Service

### Définition

- Couche intermédiaire entre le controller et le repository

- Contient la logique métier

> C'est le cerveau de l'application !

----

## Service

### Créer un service

- Un service est une classe Java "classique"

- Il faudra lui ajouter l'annotation `@Service`

- Il possède un généralement attribut repository, qui est injecté via l'IoC

> Nous aborderons les Repository dans le prochain chapitre !

----

## Service

### Démonstration

Créons un service claculatrice !

----

## Service

### CalculatriceService

Dans `src/main/java/fr/octocorn/democoursapi/calculatrice`

```java [0]

@Service
public class CalculatriceService {

    /**
     * Additionne deux nombres
     *
     * @param permierNombre
     * @param secondNombre
     * @return a + b
     */
    public int additionner(int permierNombre, int secondNombre) {
        return permierNombre + secondNombre;
    }

    /**
     * Soustrait deux nombres
     * @param premierNombre
     * @param sedoncNombre
     * @return a - b
     */
    public int soustraire(int premierNombre, int sedoncNombre) {
        return premierNombre - sedoncNombre;
    }

    /**
     * Multiplie deux nombres
     * @param premierNombre
     * @param secondNombre
     * @return a * b
     */
    public int multiplier(int premierNombre, int secondNombre) {
        return premierNombre * secondNombre;
    }

    /**
     * Divise deux nombres
     * @param premierNombre
     * @param secondNombre
     * @return a / b
     */
    public int diviser(int premierNombre, int secondNombre) {
        return premierNombre / secondNombre;
    }
}
```

----

## Service

### CalculatriceService

- Le service peut maintenant être injecté dans le controller
- Il faudra donc ajouter un attribut de type `CalculatriceService` dans le controller

```java

@RestController
@RequestMapping("/calculatrice")
public class CalculatriceController {
    private CalculatriceService calculatriceService;

    public CalculatriceController(CalculatriceService calculatriceService) {
        this.calculatriceService = calculatriceService;
    }
    // code ...
}
```

----

## Service

### CalculatriceController

```java [12-15]

@RestController
@RequestMapping("/calculatrice")
public class CalculatriceController {

    private CalculatriceService calculatriceService;

    public CalculatriceController(CalculatriceService calculatriceService) {
        this.calculatriceService = calculatriceService;
    }

    // On passe tous les paramètres en paramètre de la requête
    // L'url ressemblera à : /calculatrice/additionner?a=1&b=2
    @GetMapping("/additionner")
    public int additionner(@RequestParam int a, @RequestParam int b) {
        return calculatriceService.additionner(a, b);
    }

    @GetMapping("/soustraire")
    public int soustraire(@RequestParam int a, @RequestParam int b) {
        return calculatriceService.soustraire(a, b);
    }

    @GetMapping("/multiplier")
    public int multiplier(@RequestParam int a, @RequestParam int b) {
        return calculatriceService.multiplier(a, b);
    }

    @GetMapping("/diviser")
    public int diviser(@RequestParam int a, @RequestParam int b) {
        return calculatriceService.diviser(a, b);
    }
}
```

----

## Service

### CalculatriceController

- Ceci dit, passer toutes les informations en paramètre de la requête n'est pas très pratique

- Que se passera-t-il si on a beaucoup de paramètres ? Ou un objet complexe ?

- Pour ça, il est préférable d'utiliser le corps de la requête !

---

# Spring Boot

## Les DTO

![Spring](./assets/spring.png) <!-- .element: width="20%" align="left"-->

![Spring Boot](./assets/spring-boot.png) <!-- .element: width="40%" align="right"-->

----

## DTO

### Définition

- Les DTO (Data Transfer Object) sont des objets qui permettent de transférer des données

- Il représentera la forme des données que l'on souhaite envoyer

- Il sera ensuite converti en objet Java

----

## DTO

### Créer une DTO

- Une DTO est une classe Java "classique"

- Elle ne contient que des attributs et des getters/setters

- On peut l'annoter avec `@Data` de Lombok pour générer les getters/setters

> Merci Lombok !

----

## DTO

### Démonstration

Créons une DTO pour notre calculatrice !

----

## DTO

### CalculatriceDto

Dans `src/main/java/fr/octocorn/democoursapi/calculatrice/dto` :

```java [0]

@Data
public class CalculatriceDto {

    private int premierNombre;
    private int secondNombre;
}
```

----

## DTO

### CalculatriceController

```java [13, 18, 23, 28]

@RestController
@RequestMapping("/calculatrice")
public class CalculatriceController {

    private CalculatriceService calculatriceService;

    public CalculatriceController(CalculatriceService calculatriceService) {
        this.calculatriceService = calculatriceService;
    }

    @PostMapping("/additionner")
    public int additionner(@RequestBody CalculatriceDto calculatriceDto) {
        return calculatriceService.additionner(calculatriceDto.getPremierNombre(), calculatriceDto.getSecondNombre());
    }

    @PostMapping("/soustraire")
    public int soustraire(@RequestBody CalculatriceDto calculatriceDto) {
        return calculatriceService.soustraire(calculatriceDto.getPremierNombre(), calculatriceDto.getSecondNombre());
    }

    @PostMapping("/multiplier")
    public int multiplier(@RequestBody CalculatriceDto calculatriceDto) {
        return calculatriceService.multiplier(calculatriceDto.getPremierNombre(), calculatriceDto.getSecondNombre());
    }

    @PostMapping("/diviser")
    public int diviser(@RequestBody CalculatriceDto calculatriceDto) {
        return calculatriceService.diviser(calculatriceDto.getPremierNombre(), calculatriceDto.getSecondNombre());
    }
}
```

----

## À vous de jouer !

Réalisez le TP 1 !

---

# Spring Boot

## La suite !

[index](index.html)