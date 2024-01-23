# TP 1 : Les températures

## Objectifs 🎯

Vous devez créer une API REST qui permet d'afficher des températures sous différents formats (Celsius, Fahrenheit,
Kelvin).

Elle se base sur les degrés celsius et retourne un objet JSON avec les trois formats.

### Attendus 📚

- Créer un projet Spring Boot
- Créer un contrôleur qui permet d'afficher les températures
- Créer un service qui permet de convertir les températures

### Routes 🛣️

#### GET `/temperature`

- Retourne un objet JSON avec une température en Celsius, Fahrenheit et Kelvin
- La température de Celsius est toujours 0

```json
{
  "celsius": 0,
  "fahrenheit": 32,
  "kelvin": 273.15
}
```

#### GET `/temperature?celsius=0`

- Retourne un objet JSON avec une température en Celsius, Fahrenheit et Kelvin

```json
{
  "celsius": 0,
  "fahrenheit": 32,
  "kelvin": 273.15
}
```

### POST `/temperature/conver`

Convertir une température donnée dans un format donné
Le body contient un objet JSON avec une température et un format

```json
{
  "value": 0,
  "unit": "celsius"
}
```

- Retourne un objet JSON avec une température en Celsius, Fahrenheit et Kelvin

```json
{
  "celsius": 0,
  "fahrenheit": 32,
  "kelvin": 273.15
}
```

## Aides du poulet 🐔

- Il existe plusieurs moyens de convertir les températures :
    - Déléguer la conversion au service
    - Utiliser l'encapsulation dans les objets en utilisant des getters
- Dans tous les cas, vous aurez au moins des getters pour les trois formats de température
- Vous pouvez utiliser les formules suivantes :
  - `fahrenheit = celsius * 1.8 + 32`
  - `kelvin = celsius + 273.15`
  - `celsius = (fahrenheit - 32) / 1.8`
  - `celsius = kelvin - 273.15`
- Utilisez le format `double` pour les températures

## Aides du poussin 🐣

### Cheat sheet des annotations

- `@GetMapping` sur une méthode du controller pour créer une route GET
- `@PostMapping` sur une méthode du controller pour créer une route POST
- `@RequestParam` sur un paramètre pour récupérer un paramètre dans l'URL (`?param=valeur`)
- `@RequestBody` sur un paramètre pour récupérer un paramètre POST dans le body de la requête
- `@PathVariable` sur un paramètre pour récupérer un paramètre dans l'URL (`/param/valeur`)
- `@RestController` : sur une classe déclare une classe comme étant un contrôleur REST
- `@Service` : Déclare une classe comme étant un service
- `@RequestMapping("/films")` : Sur une méthode ou une classe, précise le chemin d'accès à la ressource

### Température

Vous aurez besoin d'une classe Température qui contiendra les trois formats de température.
En fonction de vos choix (conversion dans le controller ou dans le service), elle contiendra les trois formats ou un seul.

Notez que même si vous utilisez Lombok, vous pouvez préciser des getters et setters manuellement ! 

```java
@NoArgsConstrutor // Constructeur vide
@Getter // Créé tous les getters
@Setter // Créé tous les setters
class Foo {
    private String bar; // Propriété bar
    
    public String getEncapsuledBar() {
        // Code ...
        return bar + "des trucs";
    }
}
```

### Controller

- Votre controller contiendra trois méthodes :
    - `getTemperature()` : Retourne une température avec les trois formats
    - `getTemperatureWithCelsius()` : Retourne une température avec les trois formats
    - `convertTemperature()` : Convertit une température dans un format donné

### Service

- Votre service contiendra au moins 2/3 méthodes, voir plus si vous avez décide de déléguer la conversion au service


