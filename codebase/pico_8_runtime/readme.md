# Pico-8 runtime

Most things related to the Pico-8 runtime is found at `/core/...`, with `/core/core.lua` (may be renamed to `/core/main.lua`) stitching it together.

This defines things such as `wantedFramerate` (will be moved to be a property of `loaded_p8`) and `loaded_p8`, general things for the Pico-8 environment.

The variable `p8frame` (defined in `/main.lua`, will be moved to `/core/core.lua`) is a `userdata u8`, being the 128x128 Pico-8 display. Each time the pico-8 environment is in runtime; the draw target is set to this userdata.

The picotron `_draw()` function simply `blit()`s (windowed) or `sspr()`s (fullscreen) to the screen.

## `loaded_p8`

This is the table that will hold all data to do with the pico-8 environment that is globally accessible.

It's properties are as follows:

### filepath (str)
The filepath of the Pico-8 cartridge

### title (str)
The title of the Pico-8 cartridge, generated from the comments in the first Pico-8 code tab or the basename of the file.

### env (table)
The Pico-8 lua environment; this shouldn't need to be touched manually and new functions are implemented in `/core/p8functs/...`.

### code (str)
The raw code of the Pico-8 cartridge (should be able to just delete to save memory once all actions related are done)

### code_length (number)
The length of the code, for the print in the terminal with:
```
> load cart.p8
```
(should be able to just delete to save memory once all actions related are done)

### mem (userdata u8)
A 0x8000 userdata, being Pico-8's userdata.

Will need expanding to the full memory of Pico-8.

Memory is handled by `/core/memory.lua` and `/core/p8functs/memory.lua` and memory status is:
```
0x0     0x0fff  Sprite sheet (0-127)*                                   [IMPLEMENTED]
0x1000  0x1fff  Sprite sheet (128-255)* / Map (rows 32-63) (shared)     [IMPLEMENTED]
0x2000  0x2fff  Map (rows 0-31)                                         [IMPLEMENTED]
0x3000  0x30ff  Sprite flags                                            [IMPLEMENTED]
0x3100  0x31ff  Music                                               
0x3200  0x42ff  Sound effects                                           [IMPLEMENTED]
0x4300  0x55ff  General use (or work RAM)                               [IMPLEMENTED]
0x5600  0x5dff  General use / custom font (0.2.2+)                      [IMPLEMENTED]
0x5e00  0x5eff  Persistent cart data (64 numbers = 256 bytes)           [IMPLEMENTED]
0x5f00  0x5f3f  Draw state                                                           
0x5f40  0x5f7f  Hardware state                                      
0x5f80  0x5fff  GPIO pins (128 bytes)                               
0x6000  0x7fff  Screen data (8k)*                                       [IMPLEMENTED]
0x8000  0xffff  General use / extended sprite sheets / extended map 
```

### active (bool)
Whether the cartridge is actively running or not.

### spritesheet (userdata u8)
A 128x128 userdata defining the loaded spritesheet.

### mapsheet (userdata i16)
A 128x64 userdata defining the mapsheet.

### spriteflags (userdata u8)
A 256x1 userdata defining the spriteflags. These are also `fset()`'d for optimisations for `map()` as it uses Picotron's C map function for speed.

### created (number)
A timestamp (with `time()`, accurate to picotron `_update`) of when the cart was loaded

### created_epoch (number)
An epoch timestamp (with `stat(86)()`) of when the cart was loaded.

### menu_triggered (bool|nil)
When set, the menu will be triggered the next time the cart is updated.

Used by `extcmd()`.

## Ripping

Ripping is triggered in `/core/core.lua`, using either `.p8` or `.p8.png` (`.p8.png` not fully supported) format.

Ripping code can be found at `/core/ripping/...`.

The code for splitting sections for `.p8` format is found in `/core/core.lua` and will be moved to a file in `/core/ripping/p8/...`

The wiki is the best location for documentation on the file formats.

## Functions

Pico-8 functions can be added to the Pico-8 environment by making a file or adding them to a pre-existing file in `/core/p8functs/...`.

Each file in here is automatically fetched and included in runtime, and does not need to be manually added to the code when a new `.lua` or function is added.

New Pico-8 functions are added in a `.lua` file in `/core/p8functs/...`; feel free to look at pre-existing files for this. `.lua` files should be made for new categories, and pre-existing `.lua` files should be used for categories already added.

They look like the following:
```lua
p8env.example_function=function(argument)
    return argument+1
end
```

and the Pico-8 cart would call `example_function()`.

Non pico-8 functions should **not** be added to this.

> Currently any picotron functions that exist can be used by the Pico-8 environment. This will eventually be changed with a list of all Pico-8 functions and only allowing those to be in the virtual environment. Some Picotron functions are included for compatibility as many work the same or very similar (very similar functions will require custom creations, but can likely be a wrapper for the picotron function)