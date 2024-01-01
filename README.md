# CollectCallMappingKit
A collection of files for making custom maps for Collect Call.

This mapmaking kit contains all the assets used to create Collect Call's levels. Before you start making your own, I suggest looking at some of mine so you know how this works.

#OPENING EXISTING MAPS
If you look in the maps folder, you'll see 15 maps which were made for Collect Call. You can open these with Tiled. 

If you see a bunch of red Xs everywhere, don't be alarmed. The red X means that Tiled wasn't able to automatically find
the Tilesets for the map. All the Tilesets for my maps are in the tileset folder. You should see a list of missing files at the top of your screen, click one and then click "Locate File". Most maps will have 3-4 tilesets. 

#MAP PROPERTIES
Each map has some properties that control how the game reads it. I'll list the important parameters below.
 - defaultZoom: This controls how the camera will zoom by default. This is usually a decimal between 0.1 and 0.3. You can adjust this as you wish. This property is required.
 - weatherParticleTextures: This is the path the game should use to find weather particles in. Use "textures/raindrop.png" for a raindrop.
 - weatherParticlesSpawnAngle: This is the angle relative to the player that the particles should spawn at, and the direction they should travel.
 - weatherParticlesSpeed: This controls how fast the particles move.
 - weatherParticlesSpread: How spread out the particles should be when they spawn. I recommend putting this around 90 or higher.

Looking in Tiled, you can see each map is made up of layers. Some layers contain just Tiles with textures, while others have Objects which have properties and shapes. 

#TILE LAYERS
Your maps can have as many tile layers as you want, and their names don't matter either. You can use multiple layers to produce a parralax effect. Tile layers don't need any special properties.
Tile layers can have parallax on them, but be aware that entities and puzzle objects will always be drawn on top of the map, so be careful or it will look very weird.

#OBJECT LAYERS
This game uses object layers to store data about places in the map. Objects are usually shapes that have some data attached. A few layers are required for a map to load, but some are optional.

Required Object Layers:
 - EntitySpawner - This layer tells the game where entities should spawn.
	Objects here control the placement and hitboxes of entities. 
	FLOAT speed: Speed of the entity
	INT textureHeight: Height of the entity's texture
	INT textureWidth: Width of the entity's texture
	STRING type: Sets which entity class to use. Player, Follower, or Prop
 - LevelCollision - Tells the game where collision should go.
	Objects here represent the world's collision. No extra properties necessary.
 - LevelSwitch - Controls switching to other levels, including ending the game.
	STRING destinationMapFilePath: Controls which level should be loaded next.
	FLOAT destinationX: The player's x axis upon switching to next level
	FLOAT destinationY: The player's y axis upon switching to next level

Optional Layers:
 - Hints - Provides button prompts
	STRING button: Which button the player is prompted to press. A, B, X, Y, Left, Right, Up, Down, StickUp, Start. If the player uses a keyboard, the corrosponding input will be shown instead.
	FLOAT showDist: Button prompt will show when player is within this distance.
 - CameraModes - Overrides default camera behavior when the player is colliding with an object on this layer.
	STRING cameramode: Either "center" or "follow", sets which behavior the camera should use.
	FLOAT targetzoom: camera will smoothly zoom to this distance
 - PuzzleObjects - Places puzzles in the level. 
	Required properties:
	 INT puzzlePieceID: unique identifier for a puzzle piece, starting from 0. Do not skip IDs or the game will crash. (i.e, do not use ID of 2 unless there is an ID of 1 somewhere in the level.)
	 STRING textureFilePath: Path to the texture file. Must be present, but can be set as an empty string if you want an invisible object.
	 STRING type: Controls which type of puzzle object is loaded.
	 FLOAT deactivateAfterSeconds: Object will remain active for this amount of time, not used by all puzzle types.
	Optional properties:
	 BOOLEAN animateOnInternalActivation: Animation will not be affected by the defaultActivated property.
	 BOOLEAN defaultActivated: If enabled, this makes the object active when it would be inactive and vice versa.
	 BOOLEAN flipped: Flips the object's texture.
	 BOOLEAN or: If enabled, only 1 item in the activators list is needed to activate this object, rather than all of them.
	 STRING passiveAnimationFilePath: path to an animation the object should play when inactive.
	 BOOLEAN permanentlyActivated: If enabled, this makes the object stay active forever.
	 BOOLEAN loopAnimation: If enabled, the object will play its animation as long as it is activated.
	PuzzleObject Types:
		PuzzleListener: Generic puzzle object which is activated by other puzzle objects.
		 - STRING activators: Comma-separated list of puzzle object IDs which this object should listen to.
		 Subtypes: 
			Clock: Oscilates between reporting active and inactive so long as it is activated. If inactive, it will report whatever state it was in when it lost activation.
			 - FLOAT onTime: Length of time, in milliseconds, that it should be on.
			 - FLOAT offTime: Length of time, in milliseconds, that it should be off.
			FlagSetter: Sets one or more flags when it transitions from inactive to active. If the flags are later unset, this object will re-set them if it is deactivated and then reactivated.
			 - STRING flags: Comma-separated list of flags this object should set.
			PuzzleCollision: Generic puzzle listener with modifiable collision.
			 Subtypes:
				MovingPlatform: Collision that moves between two locations.
				 - BOOLEAN absoluteActivatedPos (optional): activated position will be relative to the map rather than relative to the object's default position. Use this if you like headaches, I might remove it later.
				 - FLOAT activatedX: Sets where the object will move on X axis when it is activated.
				 - FLOAT activatedY: Sets where the object will move on Y axis when it is activated.
				 - FLOAT time: Time, in seconds, for the object to move between positions.
				ToggleCollision: Collision that can be toggled on/off by puzzles. No special properties.
		EntityListener: Generic puzzle object which is activated by entities
		 - STRING activators: Comma-separated list of entity types which this object should listen for.
		 - FLOAT gravity: Strength that this object will pull activators towards it. Use a negative number to repel activators.
		 Subtypes:
		FlagListener: Activated by flags, which are variables set by the game to track progress.
		 - STRING activators: Comma-separated list of flag strings the object should listen for.
		UseListener: Toggled on/off by player interaction.
		 - STRING highlightAnimation: path to an alternate texture/animation that should show when the player gets close.
 - Ladders
	Ladders are set in a manner similar to LevelCollision. Anytime the player is overlapping a rectangle object in the Ladders layer, the player will have ladder controls rather than normal controls.
