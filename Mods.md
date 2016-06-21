This guide will describe how mods and their triggers attach to these projects.

### Table of Contents
1. [ModAttachr](#modattachr)
    1. [Event Hooks](#event-hooks)

# ModAttachr

ModAttachr is a module that allows for extensible triggered mod events.

A mod is a custom modification that changes how the game normally runs and/or how the player interacts with the game.
Each mod can define any number of events, keyed by string, to call when they are fired.
When an event is fired in the game, ModAttachr calls the corresponding events of all enabled mods that have a defined event under that name.

A mod can be added with `addMod`.

```typescript
ModAttachr.addMod({
    name: "Infinite Health",
    events: {
        "onModEnable": function (mod: ModAttachr.IModAttachrMod): void {
            // event code when mod is enabled
        },
        "onModDisable": function (mod: ModAttachr.IModAttachrMod): void {
            // event code when mod is disabled
        },
        "onBattleStart": function (mod: ModAttachr.IModAttachrMod): void {
            // event code when a battle starts
        }
    },
    enabled: false
});
```
## Event Hooks
When a mod is enabled, its `onModEnable` event is called; when a mod is disabled, its `onModDisable` event is called.
A mod can be toggled with `toggleMod`.

```typescript
// Enables the mod.
ModsAttachr.enableMod("Infinite Health");
// Disables the mod.
ModsAttachr.disableMod("Infinite Health");
// Toggles the mod.
ModsAttachr.toggleMod("Infinite Health");
```
To actually fire events use the module's `fireEvent` function.

```typescript
// Fires the onBattleStart event for all mods
ModAttachr.fireEvent("onBattleStart");
```

And to fire an event for one mod, use `fireModEvent`
```typescript
ModAttachr.fireModeEvent("onBattleStart", "Infinite Health");
```

More can be read about ModAttachr on its [Readme](https://github.com/FullScreenShenanigans/ModAttachr/blob/master/README.md).