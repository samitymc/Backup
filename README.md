# BrokenSMP plugin downloads

## UnstableHub 3.0.1 — Velocity navigation fix

[Download UnstableHub-Velocity-3.0.1.jar](https://github.com/samitymc/Backup/raw/refs/heads/main/downloads/UnstableHub-Velocity-3.0.1.jar)

Install this JAR on the **Paper hub backend**. It switches players through Velocity using its BungeeCord-compatible plugin messaging channel.

1. Stop the Paper hub server and replace the old UnstableHub JAR. Keep the existing plugin data folder.
2. In the existing `[advanced]` section of `velocity.toml`, set `bungee-plugin-message-channel = true`.
3. Register your backend IP addresses and ports in Velocity's existing `[servers]` section.
4. Hub destinations must match those registered **server names**, for example `roleplay`, `smp`, and `pvp`.
5. Restart the proxy and hub. Use the selector or `/send_to_server roleplay`.

Read [the detailed setup instructions](downloads/SETUP.txt) before changing configuration.

SHA-256: `a7348eaad57ae5447765d6e3923fd0602a10daba1b67c7376200aa9e3a0ba376`

Validation: 64 automated routing/protocol checks and bytecode data-flow checks across 261 methods. Live Paper/Velocity network testing is still needed.
