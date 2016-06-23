This guide will describe how inputs from the user (keyboard, mouse, etc.) are routed through InputWritr and how the DeviceLayr Gamepad can be used on top of it.

### Table of Contents
1. [InputWritr](#inputwritr)
2. [DeviceLayr](#devicelayr)

# InputWritr

InputWritr is a module that automates interactions with user-called events and callbacks. 

Events are in game responses to user input.
They allow the user to take control of some part of the game or have interaction with it.
Events can be added and removed using `addEvent` and `removeEvent`.

```typescript
InputWritr.addEvent("onKeyDown", 39, keyDownRightFunction);
InputWritr.removeEvent("onKeyDown", 39);
```

Triggers are what allow the user to trip events.
They are the user inputs that get mapped to the events.
They are stored as both character codes (e.g. 37) and string representation of their inputs (e.g. "left").
Events can be triggered by any number of inputs.

Aliases are additional inputs that allow for an event to be triggered from multiple user sources.
They can be added and removed using `addAliasValues` and `removeAliasValues`.

```typescript
InputWritr.addAliasValues("left", [30, 34, 35]);
InputWritr.removeAliasValues("left", [30, 34, 35]);
```

```typescript
{
    "aliases": {
        "left":   [65, 37],     // a, left button
    },
    "triggers": {
        "onkeydown": {
            "left": function (event?: IEvent): void {
                console.log("left button was pressed");

                // to prevent browser default events
                if (event && event.preventDefault) {
                    event.preventDefault();
                }
            }
        },
        "onkeyup": {
            "left": function (event?: IEvent): void {
                console.log("left button was released");

                if (event && event.preventDefault) {
                    event.preventDefault();
                }
            }
        }
    }
}
```

A history is saved so its possible to play back event information by simulating keystrokes.
The timestamp indicates when events happen which allows for proper delay between events.

More can be read about InputWritr on its [Readme](https://github.com/FullScreenShenanigans/InputWritr/blob/master/README.md).

# DeviceLayr

DeviceLayr is a module for GamePad API bindings which allow for the use of devices like controllers.

DeviceLayr has its own input and trigger mappings in addition to the mappings for InputWritr.
Connected devices are detected and registered with `checkNavigatorGamepads`.
The inputs for gamepads are joystick tilts and button presses.
If a trigger was activated by the user, `activateAllGamePadTriggers` calls the equivalent InputWritr event. 
Aliases for gamepad triggers are binary signals for whether an active change was made to the state of the gamepad (e.g. pressing a button as opposed to releasing).


More can be read about DeviceLayr on its [Readme](https://github.com/FullScreenShenanigans/DeviceLayr/blob/master/README.md).