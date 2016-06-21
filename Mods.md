This guide will describe how mods and their triggers attach to these projects.

### Table of Contents
1. [ModAttachr](#modattachr)

# ModAttachr

ModAttachr is a module that allows for extensible triggered mod events.

A mod is a custom modification that changes how the game normally runs and/or how the player interacts with the game.
Each mod can define any number of events, keyed by string, to call when they are fired in the game.
When an event is fired in the game, ModAttachr goes through all the enabled mods that have a defined event under that name.
An event can also be triggered for only a specific mod.

When a mod is added, it is added to the pool of mods and is added to the respective events.
A mod can be added with `addMod`.
```javascript
// The following adds a new mod to the pool of mods.
ModAttachr.addMod({
    name: "Infinite Health",
    events: {
        "onModEnable": function (mod: ModAttachr.IModAttachrMod): void {
            return;
        },
        "onModDisable": function (mod: ModAttachr.IModAttachrMod): void {
            return;
        }
    },
    enabled: false
});
```


When a mod is enabled, its `onModEnable` event is called; when a mod is disabled, its `onModDisable` event is called.
A mod can be toggled easily with `toggleMod`.
```javascript
// Given a mod under the name 'Infinite Health', the following will enable the mod.
ModsAttachr.enableMod("Infinite Health");

// The following disables the mod.
ModsAttachr.disableMod("Infinite Health");

// The following toggles the mod.
ModsAttachr.toggleMod("Infinite Health");
```

More can be read about ModAttachr on its [Readme](https://github.com/FullScreenShenanigans/ModAttachr/blob/master/README.md)