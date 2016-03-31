This is a general guide about Maps for all FullScreenShenanigans projects. This guide will explain the concept of Maps, Areas, and Locations within a game and how they're connected.

### Table of Contents
1. [MapsCreatr](#mapscreatr)
2. [AreaSpawnr](#areaspawnr)

# MapsCreatr

MapsCreatr is a module that handles the storage of Maps, Areas, and Locations. 

A Map is a collection of Areas. An Area is a 2D space with predefined  `x` and `y` boundaries and Things. Locations are entry points within an Area, where an entry point is where the player may end up after transitioning to a new Area.

Generally in a FullScreenShenanigans project, each Map information is stored in its own .js file, outlining all Areas and Locations within that map. When the game is started, MapsCreatr loads all this data, goes through every Map's raw JSON data to create a new Map object using ObjectMakr, and finally stores it for later reference.

More can be read about MapsCreatr on its [Readme](https://github.com/FullScreenShenanigans/MapsCreatr/blob/master/README.md).

# AreaSpawnr

AreaSpawnr is a module that works with MapsCreatr to spawn Things in a certain Area.

When a new Area is entered by the player, AreaSpawnr gets that Area's information from MapsCreatr. AreaSpawnr uses this to spawn the Area's predefined Things (or PreThings) that exist in the current viewable area, or bounding box. Each PreThing has its own entry in the Area's `creation` property. PreThing attributes are defined here, such as `x` and `y` positioning. AreaSpawnr uses this information to determine which PreThings belong in the current bounding box and ultimately place them.

More can be read about AreaSpawnr on its [ReadMe](https://github.com/FullScreenShenanigans/AreaSpawnr/blob/master/README.md).

## Entrances and Transporters

A Location is a named set of coordinates in an Area that can optionally be linked with a Thing. That Thing is called and entrance and it's where a player may end up after being transported to a Location. The Location's properties are located in the Area's JSON data and contain its `x` and `y` positioning and whether it contains entrance Thing. Entrances are a core part built into MapsCreatr. 

A transporter is a Thing that allows the player to change Areas. They are a concept added by GameStartr projects out of necessity for the way Areas are traveled between. It links to a Location, which describes where the player ends up. When the transporter is triggered, it calls `setLocation`, which brings the player to the specified Location's coordinates. 

For example, In FullScreenMario, a Pipe is commonly used as a transporter and/or entrance. The Pipe the player enters is the transporter and links to where he/she comes out of the Pipe, which is the Location. This Location can link to an entrance Pipe or just coordinates. One-way Pipes will deposit the player in a new Location but the entrance Pipe won't allow entry.