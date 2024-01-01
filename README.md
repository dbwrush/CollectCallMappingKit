# Collect Call Mapmaking Kit

This repository contains all the assets used to create levels for Collect Call. Before you start creating your own maps, it's recommended to explore some existing ones to understand the process.

## Opening Existing Maps

In the `maps` folder, you'll find 15 maps created for Collect Call. You can open these maps using Tiled. If you encounter red Xs, don't worry; Tiled couldn't automatically find the Tilesets. All Tilesets for these maps are in the `tileset` folder. Locate the missing files by following the prompts in Tiled.

## Map Properties

Each map has properties that control how the game interprets it. Important parameters include:

- `defaultZoom`: Controls the default camera zoom (usually a decimal between 0.1 and 0.3).
- `weatherParticleTextures`: Path to weather particle textures (e.g., "textures/raindrop.png").
- `weatherParticlesSpawnAngle`: Angle for particle spawn relative to the player.
- `weatherParticlesSpeed`: Speed of particle movement.
- `weatherParticlesSpread`: Spread of particles when spawned.

## Layers in Tiled

Tiled organizes maps into layers. Tile layers contain textures, and object layers store data about map locations.

### Tile Layers

Your maps can have multiple tile layers for a parallax effect. Tile layers don't require special properties.

### Object Layers

Object layers store data about map locations, including required and optional layers:

#### Required Object Layers:

- `EntitySpawner`: Controls entity spawn points.
- `LevelCollision`: Defines the world's collision.
- `LevelSwitch`: Manages level switching, including game-ending transitions.

#### Optional Layers:

- `Hints`: Provides button prompts.
- `CameraModes`: Overrides default camera behavior.
- `PuzzleObjects`: Places puzzles in the level.
- `Ladders`: Controls player ladder interactions.

For detailed information on each layer and its properties, see the sections below.

## EntitySpawner

Objects here control the placement and hitboxes of entities:

- `speed`: Speed of the entity.
- `textureHeight`: Height of the entity's texture.
- `textureWidth`: Width of the entity's texture.
- `type`: Sets entity class (Player, Follower, or Prop).

## LevelCollision

Objects represent the world's collision. No extra properties necessary.

## LevelSwitch

Controls switching to other levels, including game-ending transitions:

- `destinationMapFilePath`: Path to the next level.
- `destinationX`: Player's x-axis upon switching.
- `destinationY`: Player's y-axis upon switching.

## Hints

Provides button prompts:

- `button`: Button prompt (A, B, X, Y, Left, Right, Up, Down, StickUp, Start).
- `showDist`: Distance for button prompt visibility.

## CameraModes

Overrides default camera behavior:

- `cameramode`: "center" or "follow" for camera behavior.
- `targetzoom`: Camera zoom distance.

## PuzzleObjects

Places puzzles in the level with required and optional properties.

### Required Properties:

- `puzzlePieceID`: Unique identifier for a puzzle piece.
- `textureFilePath`: Path to the texture file.
- `type`: Controls puzzle object type.

### Optional Properties:

- `animateOnInternalActivation`: Animation unaffected by defaultActivated.
- `defaultActivated`: Enables object when it would be inactive and vice versa.
- `flipped`: Flips the object's texture.
- `or`: Enables activation with only one activator.
- `passiveAnimationFilePath`: Path to an animation when inactive.
- `permanentlyActivated`: Keeps the object active forever.
- `loopAnimation`: Object plays animation while activated.

### PuzzleObject Types:

- `PuzzleListener`: Generic puzzle activated by other puzzle objects.
  - `activators`: Comma-separated IDs of puzzle objects to listen to.
  - Subtypes: Clock, FlagSetter, PuzzleCollision.

- `EntityListener`: Activated by entities.
  - `activators`: Comma-separated entity types to listen for.
  - Subtypes: FlagListener, UseListener.

- `FlagListener`: Activated by game flags.
  - `activators`: Comma-separated flag strings to listen for.

- `UseListener`: Toggled on/off by player interaction.
  - `highlightAnimation`: Path to alternate texture/animation when close.

## Ladders

Set similar to LevelCollision; player gets ladder controls when overlapping a rectangle object in the Ladders layer.
