# CarsPlugin

Drivable cars on a Spigot server: each car is a resource pack model you place with a command, and players sit in it and drive it with the usual movement keys.

## Features

- Cars are invisible armor stands wearing a resource pack model, so nothing is needed on the client but the pack itself.
- Driving through the vanilla vehicle controls: W accelerates, S brakes and reverses, A and D turn the car.
- Per-car physics in the configuration: maximum speed forward and backwards, acceleration, braking, rotation and friction.
- Several seats per car, with the offset of each seat relative to the car, so passengers ride along.
- One permission per car for the driver (`cars.drive.<car>`) and one for the passengers (`cars.passenger.<car>`).
- Cars survive a restart: every car gets a random licence plate id, and the entities are re-registered when a player joins the server.
- Terrain handling: slabs and half blocks in front of the car are climbed instead of blocking it.

## Requirements

- Spigot 1.17 (the movement code uses NMS, `v1_17_R1`).
- A resource pack with the car model and the seat model: the car body is the helmet of a hidden armor stand with `CustomModelData` 4, the seats use `CustomModelData` 47.

## Installation

1. Build the plugin (see below) and drop the jar in `plugins/`.
2. Add the car models to your resource pack (body 4, seats 47) and send the pack to your players.
3. Start the server once: `plugins/CarsPlugin/config.yml` is generated with a `Test` car you can copy.

## Commands

| Command | Description | Permission |
| --- | --- | --- |
| `/cars place <car>` | Places a car where you stand, facing the way you look | `cars.command.place` |
| `/cars remove` | Removes the car you are looking at | `cars.command.remove` |
| `/cars reload` | Reloads the configuration | `cars.command.reload` |

`/cars` without an argument prints the commands the sender may use, and every argument is tab-completed. A placed car is announced in the chat with its licence plate id.

## Driving

Right-click the first seat with `cars.drive.<car>` to get in as the driver, the other seats with `cars.passenger.<car>` to ride along. Once seated, W accelerates, S brakes then reverses, A and D turn the car, and getting out leaves the car where it is.

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

- `Seats`: one entry per seat, numbered from 1. Seat 1 is the driver's seat, and the numbers are what the passengers click. `X` is the offset towards the front of the car, `Y` upwards and `Z` sideways, in blocks.
- `maxSpeed` / `maxSpeedBackwards`: the speed cap, in blocks per tick.
- `accelerationSpeed`: how much speed is gained each tick while accelerating.
- `breakingSpeed`: how much speed is lost each tick while braking.
- `frictionSpeed`: how much speed is lost each tick when no key is pressed.
- `rotateSpeed`: how many degrees the car turns per tick.

## Building

```sh
mvn clean package
```

The shaded jar lands in `target/`.
