This is a general guide about Mods for all FullScreenShenanigans projects. This guide will describe how mods and their triggers attach to these projects.

### Table of Contents
1. [ModAttachr](#modattachr)

# ModAttachr

ModAttachr is a module that allows for extensible triggered mod events.

A mod is a custom modification that changes how the game normally runs and/or how the player interacts with the game. Each mod defines several events to run when they are fired off in the game. When an event is fired in the game, ModAttachr goes through all the enabled mods that have a defined event under that name. An event can also be triggered for only a specific mod.

When a mod is added, it is added to the pool of mods and is added to the respective events. A mod can be added with `addMod`.

When a mod is enabled, its `onModEnable` event is called and similarly when a mod is disabled, its `onModDisable` event is called. A mod can be switched from being enabled to disabled or vice-versa easily with `togglemMod`.

More can be read about ModAttachr on its [Readme](https://github.com/FullScreenShenanigans/ModAttachr/blob/master/README.md)