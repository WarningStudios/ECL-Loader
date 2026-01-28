# ECL Loader

This module loader hides your world code from other people, allows you to create modular codes that can be used by other modules and gives you infinite space in your world code.

## INSTALLATION
Copy all the code from the file into world code. We advise you not to have any other codes in world code when doing this. You can put your other codes in world code instead, since this code has about 5000 characters and can use many callbacks, depending on what you load with it.

## HOW TO USE
The first important thing to do is specifying the positions of the code blocks to load. To do this, simply change `ECL_POSITIONS` to the coordinates you need. 
**Remember to round coordinates down.**
You also have to specify all callbacks your modules use in `ACTIVE_ECL_CALLBACKS` (aside from `tick` and `onPlayerJoin`) because callbacks have to exist on world code init.

## MODULE FORMAT
If you just want to run a code on load, you can just put it in the code block and it will be called. If you want to use callbacks, you have to create a module.

```js
const JumpForCoins = {
  onPlayerJump: (p) => {
    api.giveItem(p, "Gold Coin")
    JumpForCoins.counter++
  },
  counter: 0
}
```
This is an example for the correct format of a module:
- Define callbacks with their name in the first layer of the object
- Define the modules variables in the first layer of the object
- Access the modules variables using the modules defined name

To finally activate the module, the only thing left to do is calling the `expose` operator to send the module to the loader. This has to be the last step in the code block so the code block would return the value of the operator as result when clicked.

```js
const JumpForCoins = {
  onPlayerJump: (p) => {
    api.giveItem(p, "Gold Coin")
    JumpForCoins.counter++
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
