# Components and Modules

GameStartr instances mostly consist of **components** and **modules**.
In this lingo:
* A **component** is is a piece of the game or game engine, with functionality specific to the game or game engine.
* A **module** is a separate project that happens to be used by the instance.

## Instantiation

`EightBittr` provides a public `reset` called during the constructor that, in (coincidentally alphabetical) order, resets _c_omponents, _e_lements, and _m_odules.
This order allows resetting elements to use the built-int `Utility` component and for modules to take in components or elements.

### Best Practices

Don't override `reset`.
Reactions to the game changing or resetting state should be handled by callbacks given to modules.

## Components

Components contain functionality specific to the game or game engine and extend from the [`Component` class](https://github.com/FullScreenShenanigans/EightBittr/blob/master/src/Component.ts).
They all contain a `.gameStarter` reference to their parent GameStartr, and may optionally contain child components.

The `GameStartr` class comes with a few built-in components for core engine features, such as `Graphics`, `Maps`, and `Physics`.

You can implement your own compoents by subclassing `Component`.

```typescript
import { Component } from "eightbittr/lib/Component";

import { MyGameStartr } from "../MyGameStartr";

export class MyComponent<TGameStartr extends MyGameStartr> extends Component<TGameStartr> {
    // ...
}
```

### Best Practices

Don't violate the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle).
If you find yourself adding too much to one component, either split it into separate components or give a parent component multiple children components.

```typescript
import { Component } from "eightbittr/lib/Component";

import { MyGameStartr } from "../MyGameStartr";
import { SubComponentOne } from "./SubComponentTwo";
import { SubComponentTwo } from "./SubComponentTwo";

export class RootComponent<TGameStartr extends MyGameStartr> extends Component<TGameStartr> {
    public readonly subComponentOne: SubComponentOne<TGameStartr> = new SubComponentOne(this.gameStarter);
    public readonly subComponentTwo: SubComponentTwo<TGameStartr> = new SubComponentTwo(this.gameStarter);
}
```

It's bad practice for components themselves to maintain any sort of game state or other transient properties.
Those should be kept in modules, or in rare cases as members of the root GameStartr (such as `player` objects).

## Modules

Modules are classes that manage behavior too complex or state-based for components.
They are all represented by a single class and typically take in a single large settings object.

`resetModules` should call `super.resetModules`, set the Thing arrays given to `pixelDrawer` to draw, and start gameplay.

```typescript
protected resetModules(settings: IProcessedSizeSettings): void {
    super.resetModules(settings);

    this.userModular = this.createUserModulr(this.moduleSettings, settings);

    this.pixelDrawer.setThingArrays([
        this.groupHolder.getGroup("ThingsOne") as IThing[],
        this.groupHolder.getGroup("ThingsTwo") as IThing[]
    ]);

    this.gameplay.gameStart();
}
```

Each module corresponds to a creation function on the root GameStartr that can be overriden if necessary.
That creation function should take two arguments:
* `IModuleSettings`, settings for all modules generated in the GameStartr's `createModuleSettings`
* `IProcessedSizeSettings`, information on the screen size (and othe miscellaneous user requests)

```typescript
protected createUserModulr(moduleSettings: IModuleSettings, settings: IProcessedSizeSettings): IMenuGraphr {
    return new UserModulr({
        sizeSetting: settings.sizeSetting,
        ...moduleSettings.user
    });
}
```

### Best Practices

Don't keep module code in the same repository as your game.
Modules should be made generic enough that they can exist as a standalone open source library.

Your `resetModules` should not re-create any modules that GameStartr creates.
You should instead change the `moduleSettings` given to that module.
