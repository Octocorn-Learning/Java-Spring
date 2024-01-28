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
