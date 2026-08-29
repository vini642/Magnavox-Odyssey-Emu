# Magnavox Today
(vibe coded)

THERES NOW MOBILE SUPPORT!!

A browser-based emulator and homebrew playground for the **Magnavox
Odyssey (1972)** --- the first commercial home video game console.

The Odyssey is *very* different from a normal cartridge console. Its
Game Cards do not contain ROM, code, or a CPU program. Instead, they
connect different electrical contacts inside the console and change how
its built-in video and logic circuits interact.

That makes an Odyssey "game cartridge" hilariously tiny to represent in
software:

``` text
2-4
6-8-14-16-20-22
30-34
31-39
35-37
```

That's basically the ROM. 😭

This project emulates that idea directly and adds tools for
experimenting with original Odyssey-style homebrew games.

## Features

-   Runs entirely in a web browser
-   No installation or server required
-   Emulates the Odyssey's basic video objects:
    -   Player 1
    -   Player 2
    -   Ball
    -   Wall
-   Player movement and English controls
-   Ball movement, direction, and reset behavior
-   44-contact Game Card system
-   Electrical-net based custom cartridges
-   Official Game Card presets
-   Gate Matrix / coincidence behavior
-   English Flip-Flop behavior
-   Ball Flip-Flop support
-   Crowbar-style latched object suppression
-   PNG television overlay support
-   Adjustable overlay opacity
-   Optional CRT scanlines and glow
-   `.ody` cartridge import/export
-   Built-in Homebrew Workshop
-   Beginner wiring cheat sheet
-   Searchable pin guide
-   Advanced live diagnostics

## Running It

Just open the HTML file in a modern browser.

No build step. No dependencies. No web server.

You can also host it directly with **GitHub Pages**.

## Controls

### Player 1

  Control   Key
  --------- -----------------
  Move      `W` `A` `S` `D`
  English   `Q` / `E`
  Reset 1   `F`

### Player 2

  Control   Key
  --------- ------------
  Move      Arrow Keys
  English   `U` / `O`
  Reset 2   `H`

### Master Console

  Control       Key
  ------------- -----------
  Ball speed    `-` / `+`
  Wall center   `[` / `]`

The exact effect of Reset and some other controls depends on the
inserted Game Card --- just like on the real Odyssey.

## How Game Cards Work

The original Odyssey Game Cards are not ROM cartridges.

They are essentially circuit boards that connect contacts in the
console's card slot. Those connections enable circuits and route signals
between parts of the machine.

This emulator represents those connections as plain text.

For example:

``` text
2-4
30-34
35-37
```

means:

-   pins `2` and `4` are electrically connected
-   pins `30` and `34` are electrically connected
-   pins `35` and `37` are electrically connected

A connection has **no direction**. `30-34` does not mean "send pin 30 to
pin 34"; both pins simply belong to the same electrical net.

You can also connect more than two contacts:

``` text
6-8-14-16-20-22
```

All six contacts become one net.

### Nets Can Merge

These:

``` text
6-20
6-14
```

effectively create:

``` text
6-14-20
```

because pin 6 joins both groups.

## `.ody` Homebrew Files

Custom cartridges can be saved as tiny `.ody` text files.

Example:

``` text
# My Odyssey Game
# P1 is the hunter
# P2 is the target

2-4
33-37-39
```

Lines beginning with `#` are comments.

The emulator can load `.ody` and `.txt` cartridge files and can export
the current cartridge as `.ody`.

## Beginner Wiring Examples

Here are a few useful recipes supported by the emulator:

  Goal                                            Connection
  ----------------------------------------------- ------------
  Power the console                               `2-4`
  Player 1 / Ball coincidence route               `35-37`
  Player 2 / Ball coincidence route               `31-39`
  Player hits control horizontal Ball direction   `30-34`
  P2 disappears when P1 catches it                `33-37-39`
  Ball Flip-Flop horizontal-drive route           `34-36`

There are more recipes and explanations in the emulator's **Beginner
Cheat Sheet**.

The cheat sheet also marks features as **Supported**, **Partial**, or
**Not Simulated**, so known real-hardware connections are not confused
with features the emulator fully implements.

## Making Your Own Game

A simple Odyssey homebrew has three main parts:

### 1. Cartridge

Create the electrical behavior you need.

You can start from an official Game Card, press **Use Current Card as
Template**, and modify its connections.

### 2. Overlay

Create a 4:3 PNG containing the static graphics for your game.

For example:

-   a maze
-   tennis court
-   ocean
-   race track
-   dungeon
-   board game
-   space background

Load it using the emulator's PNG overlay controls.

A resolution such as **640x480** is convenient.

### 3. Rules

This is important: the Odyssey itself does not need to enforce
everything.

Original Odyssey games frequently depended on overlays, cards, dice,
tokens, score sheets, and humans following rules.

So your game can do the same!

The hardware might only provide:

> "P1 touched P2, so P2 disappeared."

Your rules can decide that this means:

> "The ghost caught the player. Lose one life."

That is very Odyssey.

## Example Homebrew Idea

### Treasure Tag

Use:

``` text
2-4
33-37-39
```

Make a dungeon or museum overlay.

-   P1 = Guardian
-   P2 = Thief
-   P2 collects treasure by touching marked areas
-   P1 tries to catch P2
-   when P1 touches P2, the Crowbar circuit makes P2 disappear
-   press Reset 2 to begin another round

A complete release could be:

``` text
TreasureTag/
├── treasure_tag.ody
├── treasure_tag.png
└── rules.txt
```

## PNG Overlays

The real Odyssey used translucent plastic overlays placed directly over
the television screen.

This emulator supports the digital equivalent.

You can:

-   load PNG/JPEG/WebP overlays locally
-   change overlay opacity
-   hide/show the overlay
-   clear it
-   enable or disable scanlines
-   enable or disable screen glow

Overlay files stay local in your browser.

## Advanced Diagnostics

The **Advanced diagnostics** section is optional and intended for
homebrew development.

It includes:

-   active circuit modules
-   coincidence indicators
-   Crowbar latch state
-   all 44 live cartridge contacts
-   connected electrical nets
-   pin descriptions
-   event log

If your custom cartridge does something bizarre, this is where to look.
:)

## Accuracy

This project aims to reproduce the **behavior and cartridge-driven
design** of the Odyssey while remaining understandable and fun to
experiment with.

It is **not currently a transistor-level electrical simulation**.

Some parts are simplified, including:

-   custom-card power distribution
-   analog damping networks
-   spot-size circuitry
-   some model/revision-specific contacts
-   electrical contention
-   raster-time coincidence timing

Coincidence detection currently uses a higher-level approximation rather
than reproducing every analog pulse in the original circuitry.

Official card behavior and important circuit relationships were
researched from Odyssey documentation, but custom physical cartridges
for vintage hardware should be designed from original schematics rather
than relying solely on this emulator.

## Documentation

The project also has an **Odyssey Homebrew Handbook** with:

-   generator/module activation information
-   pin reference
-   wiring recipes
-   overlay design
-   debugging help
-   a Pong-ish tutorial
-   a Submarine recreation tutorial
-   Cat & Mouse
-   original homebrew examples

It's a good place to start if staring at 44 numbered contacts makes your
brain evaporate.

## Why?

Because I briefly looked up how Magnavox Odyssey cartridges worked and
realized:

> "wait... these are basically just wires"

...and somehow that turned into a browser emulator, cartridge editor,
debugger, overlay system, and Odyssey homebrew SDK.

Oops.

## Credits / References

Research for the emulator used original Magnavox Odyssey documentation
and existing Odyssey reverse-engineering work, including:

-   **Magnavox Odyssey Service Manual (1TL200 / Manual No. 6500)**
-   **Magnavox Odyssey Instruction / Game Rules Manual (1972)**
-   **ODYEMU / MagnaVox-Odyssey-Emulator-2.0** by Henry D. M.

These are especially useful resources if you want to understand the real
hardware beyond the emulator's simplified model.

## Contributing

Experiments, bug reports, hardware corrections, new homebrew games,
overlays, and improvements are welcome.

If you discover that a pin or Game Card behaves differently on real
hardware, please include the source or schematic section when possible
--- the Odyssey has multiple hardware revisions and some wonderfully
strange analog behavior.

## License

Add your chosen license here.

If you want other people to freely modify and redistribute the emulator,
a permissive license such as MIT is a common choice.
