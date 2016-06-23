This guide will describe how inputs from the user (keyboard, mouse, etc.) are routed through InputWritr and how the DeviceLayr Gamepad can be used on top of it.

### Table of Contents
1. [InputWritr](#inputwritr)
    1. [Events and Triggers](#events-and-triggers)
    2. [History](#history)
2. [DeviceLayr](#devicelayr)

# InputWritr

## Events and Triggers

InputWritr is a module that automates interactions with user inputs and events. 

Events are the game's responses to user input.
They allow the user to take control of some part of the game or have interaction with it.
Events can be added using `addEvent` which takes in a trigger name, key code, and the function to run the event is triggered; they are removed with `removeEvent`
which take in the trigger and key code.

```typescript
InputWritr.addEvent("onKeyDown", 37, function (): void {
    console.log("left button pressed");
});
InputWritr.removeEvent("onKeyDown", 37);
```

Triggers are what allow the user to cause events.
They are user inputs that get mapped to the events.
They are stored as both character codes (e.g. 37) and as a string representation of their inputs (e.g. "left").
Events can be triggered by any number of inputs.

Aliases are additional inputs that allow for an event to be triggered from multiple user sources.
They can be added and removed using `addAliasValues` and `removeAliasValues` which take in the trigger and key character codes as arguments.

```typescript
InputWritr.addAliasValues("left", [65, 74, 49]); // keys a, j, and 1 respectively 
InputWritr.removeAliasValues("left", [65, 74, 49]);
```

```typescript
let InputWriter: IInputWritr = new InputWritr({
    "aliases": {
        "left":   [65, 37],     // a, left button
    },
    "triggers": {
        "onkeydown": {
            "left": function (information: any, event?: IEvent): void {
                console.log("left button was pressed");
            }
        }
    },
    "eventInformation": undefined
});
```

To run events, use `callEvent` and `makePipe`.
`makePipe`  takes in the trigger, a code label to get the alias from the event, and a boolean to determine whether the default event should be prevented and
makes a function to run the triggered event.
`callEvent` takes in the event and a key code and directly runs the event.

```typescript
InputWritr.makePipe("onkeydown", "left", true);
InputWritr.callEvent("onkeydown", "left");
```

## History

The module saves a history of the events run with a timestamp.
A history is saved so its possible to play back event information by simulating keystrokes.
The timestamp indicates when events happen which makes for proper delay between events.

```typescript
InputWritr.saveHistory("example"); // mapping example to the saved history
InputWritr.playHistory("example");
```

More can be read about InputWritr on its [Readme](https://github.com/FullScreenShenanigans/InputWritr/blob/master/README.md).

# DeviceLayr

DeviceLayr is a module for GamePad API bindings which allow for the use of devices like controllers.

DeviceLayr has its own input and trigger mappings in addition to the mappings for InputWritr.
Connected devices are detected and registered with `checkNavigatorGamepads` which automatically adds them to the list of added gamepads once called.

The inputs for gamepads are joystick, buttons, and controller triggers.
Joysticks have an x and y axis, which have a negative, netural, and positive status.
These statuses signal which direction the joystick is being tilted.

Aliases for gamepad triggers are binary signals for whether an active change was made to the status of the gamepad (e.g. pressing a button versus releasing).

If the user changes the status of a joystick or button, `activateAllGamePadTriggers` calls the equivalent InputWritr event. 

More can be read about DeviceLayr on its [Readme](https://github.com/FullScreenShenanigans/DeviceLayr/blob/master/README.md).