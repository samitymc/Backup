# Underworld / Roleplay memory and explosion update

For Paper 1.21.11. Roleplay **1.5.3**, Generation **1.3.0**. Uses the existing Underworld below Y=-65 in `world_nether` and preserves its current dimension height, seed and generated chunks.

## Downloads

- [UnstableRoleplay.jar](./UnstableRoleplay.jar?raw=true)
- [UnstableGenerationPaper.jar](./UnstableGenerationPaper.jar?raw=true)
- [Unstable-Underworld-Expansion-1.21.11.zip](./Unstable-Underworld-Expansion-1.21.11.zip?raw=true)

## Installation

1. Stop Paper and back up the worlds, current JARs and plugin data folders.
2. Replace both existing plugin JARs. Keep their existing data folders/configs, with only one copy of each plugin JAR installed.
3. Use the supplied merged datapack in your main world's `datapacks` folder, usually `world/datapacks`. Replace the earlier merged expansion pack or the original `Unstable-Underworld-1.21.11.zip` height pack. Do not enable multiple copies. The supplied ZIP has `pack.mcmeta` at its root; leave it zipped.
4. Keep the original standalone `Unstable_Underworld_1.21.11_v7.zip` disabled. This adaptation uses the existing lower Nether, without v7's extra dimension, global noise override, automatic teleport functions or entity-removal functions. Keep unrelated datapacks and your working generator assignments.
5. In Roleplay's existing config, ensure `world-protection.block-explosion-damage` is `true`. Edit the existing key; retain the other settings in that section.
6. Start Paper normally. Installing these new JARs requires this one restart. Future supported config changes can use the commands below.

Never delete region files, reset the Nether, or change its height to install this update. Existing chunks are not regenerated or retrofitted; new biome terrain and landmarks remain restricted to newly generated chunks.

## Explosions

TNT, TNT minecarts, creepers, end crystals and other ordinary entity explosions keep normal entity damage and knockback while their affected block lists are cleared. Block explosions use the same terrain protection. Explosion/fireball/crystal ignition is also blocked to prevent subsequent fire damage to terrain.

The explosive itself is still consumed. This protects surrounding terrain; it does not grant reusable TNT or restore a detonated bed/anchor. Existing Underworld placement/interaction restrictions and other plugins' combat/safe-zone protections remain in effect. This update never uncancels another plugin's damage events.

The controlling Roleplay setting is:

```yaml
world-protection:
  block-explosion-damage: true
```

When Roleplay is enabled, Generation follows that setting. Without Roleplay, Generation uses the same setting in its own config, defaulting to `true`. If globally disabled deliberately, the Underworld's local terrain protection still applies.

## Config reload commands

```text
/roleplayreload
/rpreload
/unstablegen reload
```

- Roleplay reload permission: `unstableroleplay.reload` (default OP).
- Generation reload permission: `unstablegeneration.admin` (default OP).
- Console can run the same commands without the leading slash.
- Invalid YAML is rejected before replacing the live config.
- Roleplay reload applies config values read live, including automatic anticheat bans, explosion protection, multi-kit cooldown settings and the cached normal character-kit cooldown.
- Generation reload applies protection/search settings and cancels pending biome rifts so new limits apply cleanly.
- Database connection settings, existing timer schedules, world heights, generator assignments and datapack registry changes require a restart. These commands reload configuration; they do not unload/re-enable plugins or run Minecraft `/reload`.

## Memory changes

- Automatic protagonist checks retain their per-tick eligibility checks while using an eligibility snapshot with no progress text. Detailed progress text is built when requested from the menu.
- Hidden/removed name tags skip repeated whole-world scans when there is no tracked label. The existing orphan cleanup remains active.
- Rollback records share repeated world/material/block-state strings through a bounded 4,096-entry pool. Repeated changes to an already-recorded block skip allocating another rollback entry. Rollback history is retained and memory still scales with the number of recorded changes.
- Amplified terrain reuses one bounded scratch array per generation worker rather than allocating one for each column. Workers have separate arrays; generation output is unchanged.
- Biome travel defaults to one outstanding chunk-generation request, at most eight active searches, and four new candidate chunk requests per search. This reduces bursts from simultaneous menu travel. Normal player exploration is still scheduled by Paper.
- The datapack is already small and has no repeating tick/load functions. Its biome and height files are byte-identical to the previous merged pack. Only its description changed.

Optional Generation settings, within the existing `underworld-expansion` section:

```yaml
underworld-expansion:
  max-concurrent-loads: 1
  max-active-searches: 8
  max-new-chunks-per-search: 4
```

Missing keys use these defaults; no config replacement is necessary. Keep the existing nether-world, maximum-y, biome/landmark and search settings. The five biome themes, six adapted landmarks, boat/pot exceptions, menu integration and previous Roleplay fixes remain available.

The shared multi-kit cooldown remains 30 minutes by default:

```yaml
multi-kit-cooldown-seconds: 1800
```

Changing Wraith choices does not bypass it. Its next-claim timestamp remains stored in player persistent data.

## Starting settings for an 8 GB allocation

Plugin changes cannot guarantee total server RAM use: player count, loaded chunks, other plugins and the retained extended-height world all contribute.

Start with these existing `server.properties` values and restart:

```properties
view-distance=6
simulation-distance=4
```

If your hosting allocation is 8 GB, a Java heap limit of roughly 6 GB leaves room for JVM/native overhead. A typical startup is `java -Xms2G -Xmx6G -jar paper.jar nogui` (use your actual server JAR name or hosting panel's Java-memory setting). Adjust distances/player capacity using live measurements. Do not allocate all 8 GB to `-Xmx`.

## Validation

- 8,297 existing protection, biome, landmark/loot and travel assertions passed.
- 10,288 additional assertions passed for explosion block lists/fire, reload permission and invalid-config handling, cooldown reload, bounded string pooling, scratch-array worker isolation, simultaneous travel requests and unsafe-landing search limits.
- 10,000 randomized JVM-executed comparisons confirmed identical protagonist eligibility and menu progress text; automatic snapshots contain no display lines.
- 1,572,864 sampled Nether block comparisons matched Generation 1.2.0 exactly. The sampled Overworld output and repeated amplified-column output also matched.
- Bytecode data-flow verification passed for 325 Generation methods and 1,279 Roleplay methods. JAR/ZIP integrity and config/datapack parsing passed. No compile-time API fixtures are bundled.
- The existing multi-kit helper and unrelated original JAR entries are preserved.

These checks used API fixtures and isolated JVM test harnesses. No live Paper server, heap profile or complete network of your other installed plugins was available. Test a backup copy before opening the updated server to players.
