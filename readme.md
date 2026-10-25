# Music Addon: Disc Press and Pocket Jukebox

A port of the Bedrock [Music Features Add-On](https://minecraft.wiki/w/Music_Features_Add-On) to a Java server, built as a CraftEngine add-on (`musicaddon`) and a Paper plugin (`DiscPress` 1.1.0). It brings the Empty Disc, the Disc Press and the Pocket Jukebox to Java:

- **Empty Disc**: the blank disc. It is what you burn at the Disc Press, and an ingredient of the Disc Press and every Pocket Jukebox.
- **Burned Disc**: the result of burning. A music disc that plays the sound you picked.
- **Disc Press**: a custom block. Right-click it with an **Empty Disc** to pick a Minecraft menu sound (or a CustomDiscs song) and burn it onto the disc.
- **Pocket Jukebox**: a bundle-based item (undyed or in 15 colours). Put music discs in it and play them from your inventory.

## Requirements

| Needed | Note |
|---|---|
| Paper | 26.2 |
| CraftEngine | 26.9.2 |
| CustomDiscs-SVC and Simple Voice Chat | *optional*: CustomDiscs songs in the menu and in the Pocket Jukebox |

## Installation

1. Copy the CraftEngine files (`configuration/`, `models/`, `textures/`) into `plugins/CraftEngine/resources/musicaddon/`.
2. Put `DiscPress.jar` in `plugins/` and **restart the server** (a new jar needs a restart).
3. Run `/ce reload all` and let the resource pack be sent again.
4. Check with `/ce item get musicaddon:empty_disc` and `/ce item get musicaddon:disc_press`.

## Items

| Item id | Material | custom-model-data | Texture (`textures/item/`) |
|---|---|---|---|
| `musicaddon:empty_disc` | paper | 3001 | `disc.png` |
| `musicaddon:disc_press` | paper (places the block) | 3000 | block model |
| `musicaddon:pocket_jukebox` | bundle | 3002 | `pmp_undyed.png` |
| `musicaddon:white_pocket_jukebox` | bundle | 3003 | `white.png` |
| `musicaddon:light_gray_pocket_jukebox` | bundle | 3004 | `light-gray.png` |
| `musicaddon:black_pocket_jukebox` | bundle | 3005 | `black.png` |
| `musicaddon:brown_pocket_jukebox` | bundle | 3006 | `brown.png` |
| `musicaddon:red_pocket_jukebox` | bundle | 3007 | `red.png` |
| `musicaddon:orange_pocket_jukebox` | bundle | 3008 | `orange.png` |
| `musicaddon:yellow_pocket_jukebox` | bundle | 3009 | `yellow.png` |
| `musicaddon:lime_pocket_jukebox` | bundle | 3010 | `lime.png` |
| `musicaddon:green_pocket_jukebox` | bundle | 3011 | `green.png` |
| `musicaddon:cyan_pocket_jukebox` | bundle | 3012 | `cyan.png` |
| `musicaddon:light_blue_pocket_jukebox` | bundle | 3013 | `lightblue.png` |
| `musicaddon:blue_pocket_jukebox` | bundle | 3014 | `blue.png` |
| `musicaddon:purple_pocket_jukebox` | bundle | 3015 | `purple.png` |
| `musicaddon:magenta_pocket_jukebox` | bundle | 3016 | `magenta.png` |
| `musicaddon:pink_pocket_jukebox` | bundle | 3017 | `pink.png` |

## Recipes

All 18 recipes are in `configuration/recipy.yml`. Define each recipe id only once (do not repeat them in `config.yml`).

**Empty Disc** (shaped)

```
 G 
GPG      G = glass pane
 G       P = paper
```

**Disc Press** (shaped)

```
CDC      C = copper ingot
CTC      D = Empty Disc
SSS      T = crafting table, S = cobblestone
```

**Pocket Jukebox** (shaped)

```
XPX      X = copper nugget (7 in total)
XGX      P = Empty Disc
XXX      G = bundle
```

**Coloured Pocket Jukebox**: the same pattern, with the bundle of the colour in the `G` slot (for example `minecraft:red_bundle` gives `musicaddon:red_pocket_jukebox`). There is one recipe for each of the 15 colours above.

## Empty Disc

| | |
|---|---|
| Item id | `musicaddon:empty_disc` |
| Material | paper |
| custom-model-data | 3001 |
| Texture | `textures/item/disc.png` |
| Recipe | 4 glass panes around 1 paper (see [Recipes](#recipes)) |

Where it is used:

- **Burning:** hold it in your main hand and right-click the Disc Press. Burning uses up one Empty Disc.
- **Crafting:** it is an ingredient of the Disc Press and of every Pocket Jukebox recipe.
- **Recognition:** the plugin accepts an item as an Empty Disc when it has the material `empty-disc.material` (default `PAPER`) and the item model `empty-disc.item-model` (default `musicaddon:empty_disc`). If it is not recognised, check `/discpress info`.

## Disc Press

### The block

- Block id `musicaddon:disc_press`, model `musicaddon:block/disc_press`.
- Appearance state `cactus`, so it has a collision box, does not hide the side of neighbouring blocks and is not animated by shaders. Its hitbox is therefore the (slightly smaller) cactus hitbox.
- Right-click with an Empty Disc in the **main hand** runs `discpress open <player>`; with anything else an action-bar hint is shown.

### Using it

1. Hold an Empty Disc in your main hand and right-click the press. A menu opens.
2. **Left click** a sound or song to burn it onto the disc. The Empty Disc is used up and you receive the burned disc.
3. **Right click** a sound to preview it (not possible for CustomDiscs songs).
4. Right-click a **jukebox** with a burned disc: the sound plays at the jukebox (volume `jukebox-volume`, with note particles and an action-bar message). The disc is not inserted into the jukebox.

### Menu contents

- Every game sound that starts with `ui.` (the menu sounds), found automatically. Prefixes, extra sounds and exclusions are configurable.
- If CustomDiscs-SVC is installed: every `.mp3`, `.wav` and `.flac` in `plugins/CustomDiscs/musicdata`, listed first. Burning one runs `/cd create local <file> "<name>"` for the player with a plain music disc in the main hand, with `customdiscs.create` granted for that one command. If it fails, the Empty Disc is given back.

## Burned Disc

You get a Burned Disc when you burn a Minecraft menu sound (a `ui.*` sound or any sound added in the config) at the Disc Press.

| | |
|---|---|
| Material | `burned-disc.material`, default `MUSIC_DISC_13` |
| Name | `burned-disc.name`, default gold "Burned Disc" |
| Lore | `burned-disc.lore`, by default the sound id (for example `ui.toast.in`) |
| Stored data | the chosen sound id, on the item (key `discpress:sound`) |

Where it works:

- **Normal jukebox:** right-click the jukebox with it. The sound plays at the jukebox (`jukebox-volume`, with note particles and an action-bar message). The disc is not inserted into the jukebox.
- **Pocket Jukebox:** put it in the bundle. It is played to the player (`pocket-jukebox-volume`).

Burning a **CustomDiscs song** does not give a Burned Disc. In that case CustomDiscs creates its own disc, and that disc works in a jukebox and in the Pocket Jukebox through CustomDiscs.

## Pocket Jukebox

The Pocket Jukebox is a bundle (`custom-model-data` 3002 to 3017, or item model `musicaddon:pocket_jukebox`). The discs are the items inside the bundle.

| Action | Result |
|---|---|
| Right-click (air or block) | Starts the next disc in the bundle. Every right-click moves on to the next disc and wraps around to the first. The previous disc is stopped. |
| Sneak + right-click | Stops the music ("Pocket Jukebox stopped."). |
| Right-click with an empty bundle | "Put a music disc inside your Pocket Jukebox first." |

Which discs work:

- vanilla music discs (played as `music_disc.<name>`);
- burned discs from the Disc Press;
- CustomDiscs discs (played through CustomDiscs and Simple Voice Chat; the track name is shown in the action bar).

Vanilla and burned discs are played to the player with volume `pocket-jukebox-volume`. The music is stopped when the player leaves the server. The default bundle behaviour of the right-click is cancelled while holding a Pocket Jukebox.

## Commands and permission

| Command | Does |
|---|---|
| `/discpress open [player]` | opens the Disc Press menu (the player needs an Empty Disc in the main hand) |
| `/discpress info` | shows the material and item model of the item in your main hand |
| `/discpress reload` | reloads `config.yml` and the sound list |

Permission `discpress.admin` (default: op). The Disc Press block runs the command as console, so players do not need it.

## Configuration (`plugins/DiscPress/config.yml`)

| Key | Meaning |
|---|---|
| `empty-disc.material` | material of the Empty Disc (default `PAPER`) |
| `empty-disc.item-model` | item model the plugin expects, default `musicaddon:empty_disc`; `""` checks the material only |
| `burned-disc.material`, `name`, `lore` | the disc the player receives (`<sound>` is replaced by the sound) |
| `discover-sounds`, `sound-prefixes` | find all game sounds that start with a prefix (default `ui.`) |
| `extra-sounds`, `exclude-sounds` | add or hide sounds |
| `customdiscs.enabled`, `plugin-name`, `musicdata-folder`, `command`, `create-permission` | the CustomDiscs link |
| `jukebox-volume` | volume of a burned disc on a normal jukebox (default 3.0) |
| `pocket-jukebox-volume` | volume of the Pocket Jukebox (default 1.0) |
| `messages.*` | the menu and chat texts (MiniMessage) |

If the plugin says "Hold an empty disc" while you hold one, run `/discpress info` and put the shown item model in `empty-disc.item-model`.

## Resource pack layout

```
musicaddon/
├── configuration/
│   ├── category.yml                    CraftEngine category file
│   ├── config.yml                      items and the block
│   └── recipy.yml                      all recipes
├── pack.yml
└── resourcepack/assets/musicaddon/
    ├── models/block/disc_press.json
    └── textures/
        ├── block/                      disc_press_arm, _base_bottom, _base_side, _base_top, _disc
        └── item/                       disc.png, the colour textures (white, light-gray, black, brown, red,
                                        orange, yellow, lime, green, cyan, lightblue, blue, purple, magenta,
                                        pink) and pmp_undyed.png, pmp_undyed_open.png, pmp_undyed_open_back.png
```

## Limitations

- The Pocket Jukebox goes through the discs one by one in bundle order. There is no pause/resume, no random order and no automatic next disc.
- A burned disc plays on a jukebox when right-clicked, but is not inserted (no disc in the jukebox, no comparator output).
- CustomDiscs songs need CustomDiscs-SVC and Simple Voice Chat to be enabled.