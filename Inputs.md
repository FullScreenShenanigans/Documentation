This guide will describe how inputs from the user (keyboard, mouse, etc.) are routed through InputWritr and how the DeviceLayr Gamepad can be used on top of it.

### Table of Contents
1. [InputWritr](#inputwritr)
2. [DeviceLayr](#devicelayr)

# InputWritr

InputWritr is a module that automates interactions with user-called events and callbacks. 

The module stores triggers, aliases for triggers, and a history of records of actions (i.e. keystrokes) with a timestamp.
A trigger is a mapping of events to their key codes, to their callbacks.
Events can be triggered by any number of inputs.
A history is saved so as to be able to play back event information by simulating keystrokes.

```typescript
{
    "aliases": {
        "left":   [65, 37],     // a, left button
    },
    "triggers": {
        "onkeydown": {
            "left": function (player: IPlayer, event?: IEvent): void {
                if (!player.canWalk) {
                    return;
                }

                this.TimeHandler.addEvent(
                    player.FSP.keyDownDirectionReal,
                    this.inputDelay,
                    player,
                    3);
                
                this.ModAttacher.fireEvent("onKeyDownLeft");
            }
        },
        "onkeyup": {
            "left": function (player: IPlayer, event?: IEvent): void {
                this.ModAttacher.fireEvent("onKeyUpLeft");

                (<IPlayer>thing).keys[3] = false;
                if (player.nextDirection === 3) {
                    delete player.nextDirection;
                }

                if (event && event.preventDefault) {
                    event.preventDefault();
                }
            }
        }
    }
}
```

Aliases are additional inputs that allow for an event to be triggered from multiple user actions.
They can be added and removed using `addAliasValues` and `removeAliasValues` respectively.

```typescript
InputWritr.addAliasValues("left", [30, 34, 35]);
InputWritr.removeAliasValues("left", [30, 34, 35]);
```

Events can be added and removed using `addEvent` and `removeEvent`.

```typescript
InputWritr.addEvent("onKeyDown", 39, keyDownRightFunction);
InputWritr.removeEvent("onKeyDown", 39);
```

More can be read about InputWritr on its [Readme](https://github.com/FullScreenShenanigans/InputWritr/blob/master/README.md).

# DeviceLayr

DeviceLayr is a module for GamePad API bindings which allow for the use of devices like controllers.

DeviceLayr has its own input and trigger mappings in addition to the mappings for InputWritr.
Connected devices are detected and registered with `checkNavigatorGamepads`.
The inputs for gamepads are joystick tilts and button presses.
Aliases for gamepad triggers are binary signals for whether an active change was made to the state of the gamepad (e.g. pressing a button as opposed to releasing).
If a trigger was made to an axis or a button, `activateAllGamePadTriggers` calls the equivalent InputWritr event. 


More can be read about DeviceLayr on its [Readme](https://github.com/FullScreenShenanigans/DeviceLayr/blob/master/README.md).