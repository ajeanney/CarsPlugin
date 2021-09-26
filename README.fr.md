# CarsPlugin

Des voitures conduisibles sur un serveur Spigot : chaque voiture est un modèle de resource pack que vous posez avec une commande, et les joueurs s'installent dedans et la conduisent avec les touches de déplacement habituelles.

## Fonctionnalités

- Les voitures sont des armor stands invisibles portant un modèle de resource pack : rien n'est nécessaire côté client à part le pack lui-même.
- Conduite avec les contrôles de véhicule vanilla : W accélère, S freine et recule, A et D font tourner la voiture.
- Physique réglable par voiture dans la configuration : vitesse maximale en marche avant et arrière, accélération, freinage, rotation et friction.
- Plusieurs sièges par voiture, avec le décalage de chaque siège par rapport à la voiture, pour transporter des passagers.
- Une permission par voiture pour le conducteur (`cars.drive.<voiture>`) et une pour les passagers (`cars.passenger.<voiture>`).
- Les voitures survivent à un redémarrage : chaque voiture reçoit un identifiant de plaque d'immatriculation aléatoire, et les entités sont réenregistrées quand un joueur se connecte.
- Gestion du terrain : les dalles et demi-blocs devant la voiture sont montés au lieu de la bloquer.

## Prérequis

- Spigot 1.17 (le code de déplacement utilise NMS, `v1_17_R1`).
- Un resource pack avec le modèle de voiture et celui des sièges : la carrosserie est le casque d'un armor stand invisible avec `CustomModelData` 4, les sièges utilisent `CustomModelData` 47.

## Installation

1. Compilez le plugin (voir plus bas) et déposez le jar dans `plugins/`.
2. Ajoutez les modèles de voiture à votre resource pack (carrosserie 4, sièges 47) et envoyez le pack à vos joueurs.
3. Démarrez le serveur une fois : `plugins/CarsPlugin/config.yml` est généré avec une voiture `Test` à copier.

## Commandes

| Commande | Description | Permission |
| --- | --- | --- |
| `/cars place <voiture>` | Pose une voiture là où vous êtes, orientée dans votre direction | `cars.command.place` |
| `/cars remove` | Supprime la voiture que vous regardez | `cars.command.remove` |
| `/cars reload` | Recharge la configuration | `cars.command.reload` |

Lancée sans argument, `/cars` affiche les commandes que l'expéditeur a le droit d'utiliser, et chaque argument est auto-complété. Une voiture posée est annoncée dans le chat avec son identifiant de plaque.

## Conduite

Faites un clic droit sur le premier siège avec `cars.drive.<voiture>` pour monter en conducteur, sur les autres sièges avec `cars.passenger.<voiture>` pour monter en passager. Une fois assis, W accélère, S freine puis recule, A et D font tourner la voiture, et descendre laisse la voiture sur place.

## Configuration

```yaml
CarsList:
  Test:
    Seats:
      1: {X: -1, Y: -0.5, Z: 0}
      2: {X: -1, Y: -0.3, Z: 1}
      3: {X: -1, Y: -0.3, Z: -1}
    breakingSpeed: 0.03
    maxSpeed: 0.7
    maxSpeedBackwards: 0.35
    accelerationSpeed: 0.015
    rotateSpeed: 8
    frictionSpeed: 0.005
```

- `Seats` : une entrée par siège, numérotées à partir de 1. Le siège 1 est celui du conducteur, et les numéros sont ceux sur lesquels les passagers cliquent. `X` est le décalage vers l'avant de la voiture, `Y` vers le haut et `Z` sur le côté, en blocs.
- `maxSpeed` / `maxSpeedBackwards` : la vitesse maximale, en blocs par tick.
- `accelerationSpeed` : la vitesse gagnée à chaque tick en accélérant.
- `breakingSpeed` : la vitesse perdue à chaque tick en freinant.
- `frictionSpeed` : la vitesse perdue à chaque tick quand aucune touche n'est pressée.
- `rotateSpeed` : le nombre de degrés dont la voiture tourne par tick.

## Compilation

```sh
mvn clean package
```

Le jar shaded est écrit dans `target/`.
