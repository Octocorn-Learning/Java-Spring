---
title: Spring Boot
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

```java

@RestController
public class HelloController {
}
```

----

## Controller

### HelloController

- On pourra ensuite créer des méthodes pour gérer les requêtes HTTP

- Une annotation devra être ajoutée en fonction du type de requête
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

```java

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

```java

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

```java

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

```java

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

```java

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

```java

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

```java

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

```java

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

```java [5-6]

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

```java [5-6]
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

```java [5-6]
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

```java [5-6]
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

```java [5-6]
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

```java
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