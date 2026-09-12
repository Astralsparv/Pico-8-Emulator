# Pico-8 Emulator

## What is this?

This is a Pico-8 emulator for Picotron, intended to run Pico-8 cartridges from the new workstation Picotron.

## Using

The (what should be) stable build of the emulator will be found on the [Lexaloffle BBS](https://www.lexaloffle.com/bbs/?tid=153649) and can be used to download it, either from `splore`, the online web bbs, or with `load #p8_emulator -u`.

## Development Status

This project is still heavily in development, but a lot of stuff is functional; especially for basic games.

### Pico-8 Demo carts runnability

If zep/lexaloffle approves of these being packaged with the emulator, they will, otherwise; you will have to source these from your own Pico-8 instance with the `install_demos` command in the real Pico-8 terminal.

`api` - semi-functional, pal() needs updating to make work correctly

`hello` - seems to work, music seems off

`automata` - defunct. memcpy() stuff doesn't work

`bounce` - seems fully functional

`cast` - semi functional, colors wrong and you now can't move? was working in an earlier dev build but has now stopped working; any insight into this is appreciated

`collide` - seems fully functional

`dots3d` - seems fully functional

`drippy` - seems fully functional

`jelpi` - defunct. reset() needs proper implementation

`sort` - seems fully functional

`wander` - seems fully functional

`waves` - seems fully functional

## Codebase

Documentation for how the code is written can be found [here](/codebase/readme.md).

## Credits

Credits can be seen from the `credits` command in the emulator's terminal; this will likely be the most accurate.

### original cart & contributions
by @wasdly

### pico-8 boot animation
by @jte
pico-8 bbs id #16286

### snippets for btnp
by @KeyboardDanni
picotron bbs id #libbtnstate

### pico-8 instrument help
by @KeyboardDanni
by @Nightleek
by @SmellyFishstiks"
