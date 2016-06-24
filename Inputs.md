This guide will describe how inputs from the user (keyboard, mouse, etc.) are routed through InputWritr and how the DeviceLayr Gamepad can be used on top of it.

### Table of Contents
1. [InputWritr](#inputwritr)
    1. [Events and Triggers](#events-and-triggers)
    2. [Running Events](#running-events)
2. [DeviceLayr](#devicelayr)

# InputWritr

## Events and Triggers

InputWritr is a module that automates interactions with user inputs and events. 

Inputs are stored as both character codes (e.g. 37) and as a string representation of themselves (e.g. "left").
Triggers are what tie inputs with events and are specific actions on the inputs (e.g. press).

Events can be added using `addEvent` which takes in a trigger name, key code, and the function to run when the event is triggered.

```typescript
InputWritr.addEvent("onKeyDown", 37, () => console.log("left button pressed"););
```

...and are removed with `removeEvent` which takes in the trigger and key code.

```typescript
InputWritr.removeEvent("onKeyDown", 37);
```

Events can be triggered by any number of inputs.

Aliases are additional inputs that allow for an event to be triggered from multiple user sources.
They can be added and removed using `addAliasValues` and `removeAliasValues` which both take in the input and an array of key character codes as arguments.

```typescript
InputWritr.addAliasValues("left", [65, 74, 49]); // keys a, j, and 1 respectively 
InputWritr.removeAliasValues("left", [65, 74, 49]);
```

```typescript
let InputWriter: IInputWritr = new InputWritr({
    "aliases": {
        "left": [65, 37], // a, left button
    },
    "triggers": {
        "onKeyDown": {
            "left": () => console.log("left button was pressed");
        }
    }
});
```

## Running Events

To run events, use `callEvent` and `makePipe`.
`makePipe`  takes in a trigger and code label to get the alias from the event and returns a function to run the triggered event.

```typescript
let leftKeyPipe: () => void = InputWritr.makePipe("onKeyDown", "left", true);
leftKeyPipe();
```

`callEvent` takes in the event function/trigger and a key code, then directly runs the event.

```typescript
InputWritr.callEvent("onKeyDown", "left");
```

More can be read about InputWritr on its [Readme](https://github.com/FullScreenShenanigans/InputWritr/blob/master/README.md).

# DeviceLayr

DeviceLayr is a module for GamePad API bindings which allow for the use of devices like controllers.

DeviceLayr has its own input and trigger mappings in addition to the mappings for InputWritr.
Connected devices are detected and registered with `checkNavigatorGamepads` which automatically adds them to the list of added gamepads once called and returns the number
of gamepads added.

```typescript
let num = DeviceLayr.checkNavigatorGamepapds();
console.log(${num}` gamepads were added.`);
```

The inputs for gamepads are joysticks, buttons, and controller triggers.
Joysticks have an x and y axis, which have a negative, netural, and positive status.
These statuses signal which direction the joystick is being tilted.

Aliases for gamepad triggers are binary signals for whether an active change was made to the status of the gamepad (e.g. pressing a button versus releasing).

`activateAllGamePadTriggers` checks the status of all registered gamepads and calls the equivalent InputWritr event if any triggers have occurred.

```typescript
DeviceLayr.activateAllGamePadTriggers();
``` 

To clear the status of all joysticks and buttons, use `clearAllGamePadTriggers`.

```typescript
DeviceLayr.clearAllGamePadTriggers();
```

More can be read about DeviceLayr on its [Readme](https://github.com/FullScreenShenanigans/DeviceLayr/blob/master/README.md).