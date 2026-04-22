# Playerdata UUID Migrator

A small CLI tool for migrating Minecraft Java Edition player state from one UUID to another.

This is meant for cases where copying or renaming a player `.dat` file by hand is annoying, risky, or easy to screw up. It supports both full player-state cloning and selective tag migration for `world/playerdata/<uuid>.dat` files.

## What it does

### Full clone mode
Loads the source player's NBT data and writes it into the target player's `.dat` file while keeping the **target filename** unchanged.

That is the automated version of:
- backing up `playerdata/<target_uuid>.dat`
- copying the full contents of `playerdata/<source_uuid>.dat`
- saving the cloned result as `playerdata/<target_uuid>.dat`

### Selective tag mode
Copies only the tags you choose from the source player file into the target player file.

Useful tags include:
- `Inventory`
- `EnderItems`
- `Pos`
- `Rotation`
- `Dimension`
- `abilities`
- `XpLevel`
- `XpTotal`
- `XpP`
- `Health`
- `foodLevel`
- `foodExhaustionLevel`
- `foodSaturationLevel`

## Features

- automatic target-file creation if it does not exist
- safe `.bak` backups by default
- dry-run mode
- full clone mode
- selective tag-copy mode
- works on UUID-based `world/playerdata/<uuid>.dat` files

## Requirements

- Python 3.9+
- `nbtlib==1.12.1`

Install the dependency with:

```bash
pip install nbtlib==1.12.1
```

## Setup

1. Download or clone this project.
2. Open a terminal or PowerShell window in the project folder.
3. Run the tool with the path to your **world folder** and the source/target UUIDs.

You do **not** need to place this project inside the world folder. It can be run from anywhere as long as you give it the correct world path.

## Quick start

### 1. Back up your world first
Do this before touching any player data.

### 2. Stop the server
Do not edit live playerdata files while the server is running.

### 3. Run the tool

#### Full clone

```bash
python -m playerdata_uuid_migrator.cli   /path/to/world   11111111-1111-1111-1111-111111111111   22222222-2222-2222-2222-222222222222   --full-clone
```

#### Selective tag copy

```bash
python -m playerdata_uuid_migrator.cli   /path/to/world   11111111-1111-1111-1111-111111111111   22222222-2222-2222-2222-222222222222   --tags Inventory EnderItems XpLevel XpTotal XpP
```

## Windows example

```powershell
py -m playerdata_uuid_migrator.cli `
  C:\path\to\your\world `
  11111111-1111-1111-1111-111111111111 `
  22222222-2222-2222-2222-222222222222 `
  --full-clone
```

## Dry run

```bash
python -m playerdata_uuid_migrator.cli   /path/to/world   11111111-1111-1111-1111-111111111111   22222222-2222-2222-2222-222222222222   --tags Inventory EnderItems   --dry-run
```

## Output examples

### Full clone

```text
Mode: full-clone
Source: C:\path	o\your\world\playerdataI11111-1111-1111-1111-111111111111.dat
Target: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat
Backed up existing target to: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat.bak
Wrote cloned player data to: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat
Done.
```

### Selective tag copy

```text
Mode: selective-tags
Source: C:\path	o\your\world\playerdataI11111-1111-1111-1111-111111111111.dat
Target: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat
Backed up existing target to: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat.bak
Copied tags: Inventory, EnderItems, XpLevel, XpTotal, XpP
Saved updated player data to: C:\path	o\your\world\playerdata22222-2222-2222-2222-222222222222.dat
Done.
```

## Safety notes

- Always back up your world first.
- Keep the server offline while migrating files.
- Review the `.bak` files before deleting them.
- This only migrates data stored in `playerdata/<uuid>.dat`.
- Some mods store player-related data elsewhere. Those mods may still need their own migration steps.

## License

MIT
