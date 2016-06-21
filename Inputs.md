This guide will describe how inputs are routed through InputWritr and how DeviceLayr Gamepad can be used on top of it.

# InputWritr

InputWritr is a module that automates interactions with user-called events with callbacks. 

The module stores triggers, aliases for triggers, and a history of records of actions (i.e. keystrokes) with a timestamp.
A trigger is a mapping of events to their key codes, to their callbacks.
Events can be triggered by any number of inputs.
A history is saved so as to be able to play back event information by simulating keystrokes.

```typescript
// An example inputs mapping
{
    "aliases": {
        "left":   [65, 37],     // a,     left
        "right":  [68, 39],     // d,     right
        "up":     [87, 38],     // w,     up
        "down":   [83, 40],     // s,     down
    },
    "triggers": {
        "onkeydown": {
            "left": // left key down function
            "right": // right key down function
            "up": // up key down function
            "down": // down key down function
        },
        "onkeyup": {
            "left": // left key up function
            "right": // right key up function
            "up": // up key up function
            "down": // down key up function
        }
    }
}
```

Aliases can be added and removed using `addAliasValues` and `removeAliasValues` respectively.

```typescript
InputWritr.addAliasValues("left", [30, 34, 35]);
InputWritr.removeAliasValues("left", [30, 34, 35]);
```

Events can be added and removed using `addEvent` and `removeEvent`

```typescript
InputWritr.addEvent("onKeyDown", 39, keyDownRightFunction);
InputWritr.removeEvent("onKeyDown", 39);
```

More can be read about InputWritr on its [Readme](https://github.com/FullScreenShenanigans/InputWritr/blob/master/README.md).