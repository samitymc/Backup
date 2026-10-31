# Underworld expansion for Paper 1.21.11

This update uses the existing Underworld below Y=-65 in `world_nether`. Existing generated chunks are kept. Five biome themes and six adapted v7 landmarks appear only in newly generated chunks. The Nether above the boundary and the other generation zones retain their existing generator behavior.

## Downloads

- [UnstableGenerationPaper.jar](./UnstableGenerationPaper.jar?raw=true) — generation, protection and biome travel (1.2.0).
- [UnstableRoleplay.jar](./UnstableRoleplay.jar?raw=true) — existing Roleplay plugin with the Underworld Biomes dialog button (1.5.2). Includes the previous Roleplay fixes and a shared 30-minute multi-kit claim cooldown.
- [Unstable-Underworld-Expansion-1.21.11.zip](./Unstable-Underworld-Expansion-1.21.11.zip?raw=true) — merged height/biome datapack.

## Install without resetting the world

1. Stop the Paper server. Back up the world folders, both current JARs and both plugin data folders.
2. Replace `UnstableGenerationPaper.jar` and `UnstableRoleplay.jar` in the Paper server's `plugins` folder. Keep the existing plugin folders and configuration files. Keep only one copy of each plugin JAR.
3. In the server's main world's `datapacks` folder (usually `world/datapacks`), replace the old `Unstable-Underworld-1.21.11.zip` height datapack with `Unstable-Underworld-Expansion-1.21.11.zip`. The merged pack contains the exact original Nether dimension-type height definition. Keep it as a ZIP, with `pack.mcmeta` at its root.
4. Do not install the original `Unstable_Underworld_1.21.11_v7.zip` alongside this update. Its separate dimension, global Nether noise override, automatic teleport functions and entity-removal functions are deliberately excluded from this adaptation. Remove any copy of that original v7 pack before starting; retain unrelated datapacks.
5. Keep the existing working `world_nether` generator assignment, seed and world folders. Do not delete region files or reset the Nether. Do not change the Nether height settings.
6. Fully restart the server; do not use `/reload` or a plugin hot reloader. Check `/datapack list enabled` and `/unstablegen status` in-game. `/underworldbiomes` opens the new biome menu. `/unstablereg` and the existing Underworld travel confirmation also have an Underworld Biomes button.

The default configuration targets exactly `world_nether`, Y=-65 and below. No config edit is required for that setup. Existing configs receive these defaults internally. Optional settings can be added to `plugins/UnstableGenerationPaper/config.yml`:

```yaml
underworld-expansion:
  enabled: true
  nether-world: world_nether
  maximum-y: -65
  protection: true
  landmarks: true
  search-samples: 2048
  search-step: 192
```

Do not add the section twice. Restart after changing it. `maximum-y` cannot be raised above -65 by this update.

## What players get

- Verdant Wastes, Soul Lowlands, Ashen Wastes, Snowbound Grove and Crimson Depths.
- Adapted v7 houses, ruins, sweetberry patches, oathbinders and two crystal templates, with their original chest items and the named weapon's attributes.
- Snowy groves and crimson floating shelves. These features are adapted to the existing lower-Nether generator; this is not a wholesale replacement with v7's separate-dimension noise engine.
- Block breaking and placement are blocked in the Underworld, including buckets and common terrain-changing interactions. Boats/chest boats and bamboo rafts remain placeable. Pot right-clicks and container access remain available. Explosions, pistons and mob block changes cannot alter protected terrain.
- A five-biome dialog using the existing Roleplay menu style. Travel has a three-second standstill warmup and cancels on movement or damage before teleport starts. It checks landing space and respects permissions and the world border.

## Commands and permissions

- `/underworldbiomes` — open the menu; aliases `/uwbiomes`, `/underworldmenu`.
- `/underworldbiomes verdant_wastes` (or `soul_lowlands`, `ashen_wastes`, `snowbound_grove`, `crimson_depths`) — request travel directly.
- `unstablegeneration.underworld.travel` — enabled by default. When Roleplay is enabled, the existing `unstableroleplay.travel.underworld` permission is also required.
- `unstablegeneration.underworld.build` — explicit protection bypass, disabled by default.
- The existing `unstableroleplay.underworld.build` bypass is respected. Operators may already have that permission through Roleplay. Test protection using a normal player account.

The navigator deliberately searches for ungenerated chunks. It does not regenerate existing chunks. If everything within its bounded search is already generated, travel reports that no new terrain was found. Try from another position or increase the bounded search settings. New and old terrain can have visible transitions; old chunks are not retrofitted with the new biomes or landmarks.

## Validation

- 8,297 automated assertions passed with API fixtures: protection exceptions, biome coverage, terrain/landmark bounds, original chest loot, travel permissions, safe landings and asynchronous cancellation/completion.
- Ran original and patched generator methods under isolated class loaders with those fixtures: 1,572,864 Nether block comparisons across three sampled chunks confirmed identical output above Y=-65, and a sampled Overworld chunk was unchanged.
- Bytecode data-flow verification passed for 318 generation methods and 1,265 Roleplay methods. JAR/ZIP integrity, JSON and YAML parsing passed. No compile-time API fixtures are bundled.
- All original JAR entries except the specific generation/menu hooks and plugin metadata/default config remain byte-identical. The original height definition is byte-identical in the merged pack.

A live Paper 1.21.11 server was not available here. Start on a backup copy first to check compatibility with your installed plugins and the real world before reopening to players.

## Multi-kit cooldown update (Roleplay 1.5.2)

`/multikit` has a shared 30-minute cooldown across Mafia, Null and all Wraith choices. Switching `/wraithkit` choices does not reset it. Only successful claims start the cooldown. It is stored in player persistent data. Existing configs default to `multi-kit-cooldown-seconds: 1800`; add that top-level key to make it explicit. Replace only the Roleplay JAR and restart for this update. [Cooldown installation notes and validation](./COOLDOWN-UPDATE.txt).
