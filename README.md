# ECL Loader

This module loader hides your world code from other people, allows you to create modular codes that can be used by other modules and gives you infinite space in your world code.

## Installation
Copy all the code from the file into world code. We advise you not to have any other codes in world code when doing this. You can put your other codes in world code instead, since this code has about 5000 characters and can use many callbacks, depending on what you load with it.

## How to use
The first important thing to do is specifying the positions of the code blocks to load. To do this, simply change `ECL_POSITIONS` to the coordinates you need. 
**Remember to round coordinates down.**
You also have to specify all callbacks your modules use in `ACTIVE_ECL_CALLBACKS` (aside from `tick` and `onPlayerJoin`) because callbacks have to exist on world code init.
The loader runs all matching callbacks from the modules when a callback is called. It will return the action with the highest priority:

```
Module 1
  - onPlayerDamagingOtherPlayer -> 15
Module 2
  - onPlayerDamagingOtherPlayer -> "preventDamage"

--> returns "preventDamage"
```

## Module Format
If you just want to run a code on load, you can just put it in the code block and it will be called. If you want to use callbacks, you have to create a module.

```js
const JumpForCoins = {
  onPlayerJump: (p) => {
    JumpForCoins.giveCoin(p)
    JumpForCoins.counter++
  },
  giveCoin: (p) => {
    api.giveItem(p, "Gold Coin")
  },
  counter: 0
}
```
This is an example for the correct format of a module:
- Define callbacks with their name in the first layer of the object
- Define the modules variables in the first layer of the object
- Access the modules variables or methods using the modules defined name

To finally activate the module, the only thing left to do is calling the `expose` operator to send the module to the loader. This has to be the last step in the code block so the code block would return the value of the operator as result when clicked.

```js
const JumpForCoins = {
  onPlayerJump: (p) => {
    JumpForCoins.giveCoin(p)
    JumpForCoins.counter++
  },
  giveCoin: (p) => {
    api.giveItem(p, "Gold Coin")
  },
  counter: 0
}

expose(
  {
    PACKAGE_NAME: "jump_for_coins",
    mod: JumpForCoins
  }
)
```

This sends your module to the loader and activates it's callbacks. You can also input 2 or more modules to `expose`:
```js
expose(
  {
    PACKAGE_NAME: "package_1",
    mod: Package1
  },
  {
    PACKAGE_NAME: "package_2",
    mod: Package2
  }
)
```

## The `require` operator
If you need a specific module for your module to work, you can use the `require` operator. Since you don't know
- whether the module is even existing in the players world
- whether the module is loaded before your module (if you need it on init)
- if the module will be accessable for your module depending on how it was defined (`const`)
you can't use the modules name to access it:

```js
const ClickForCoins = {
  onPlayerAltAction: (p) => {
    JumpForCoins.giveCoin(p) // --> JumpForCoins isn't accessable
  }
}
```

Instead, you have to use the `require` operator:

```js
const ClickForCoins = {
  jumpModule: require('jump_for_coins'),
  onPlayerAltAction: (p) => {
    ClickForCoins.jumpModule.giveCoin(p) // --> gives coin
  }
}
```

This will requeue the module if the target module wasn't defined. If the target module is never defined, the system automatically detects this and stops retrying. It will also detect recursive modules.




