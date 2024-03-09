# StageScript Manual

This manual describes how to create custom game levels (stages) for _Battle of Aurinoxia_ using the StageScript language syntax.

The syntax and approaches described in this manual are the same used by the developers to create the levels shipped with the game, so you have all the same tools and limitations at your disposal!

## How it Works

Text commands are entered into StageScript files saved with the `.stage` suffix. During game play, the commands from these files are read, processed, and interpreted by the game logic. Enemy spawns, power ups, checkpoints, music, backgrounds, etc. are all scaffolded using the StageScript syntax. Advanced StageScript syntax supports repeating events (intervals), one-shots (tickers), and event-based triggers (callbacks).

The following example illustrates a minimal level setup that spawns the player along with a single enemy. When the player defeats the enemy, they can exit the stage.

```StageScript
BACKGROUND STAGE_1
MUSIC STAGE_1
FADE_IN

SPAWN_PLAYER
ENEMY LASER_BEAM_FIGHTER 80 -30 MOVE_DOWN|CLEAR|FIRE|STOP ON_DESTROY::EXIT

SUB EXIT
    STAGE_CLEAR YOU HAVE DEFEATED THE ENEMY!
RETURN
```

The example illustrates several common concepts:

- Background theme and music are established at the start
- The screen fades in from default black
- The player is spawned
- A powerful enemy is spawned at the center of the screen (80 x pixels) and just off the top of the (-30 y pixels)
- The enemy moves down for one game tick, then settles into an infinite fire/stop pattern
- When the enemy is destroyed the EXIT subroutine is triggered.
- The EXIT subroutine (SUB) establishes the logic for the callback invoked on enemy destroy.
- STAGE_CLEAR spawns the exit portal. When entered by the player ship, the accompanying text is displayed on the Stage Clear screen.

## A Word of Caution

The levels created by the developers have been extensively tested to avoid game errors or crashes. Since the level creation system is currently in Alpha, it is possible to create game-breaking scenarios with bad scripting. The syntax, parsing, and processing are still in active development, so use caution when creating levels. Please report any bugs/issues to the Steam forums.

## Using this Manual

This manual is broken up into several pages in this directory. Consult each page to learn more about the features of the StageScript syntax. The [examples](./examples) directory contains numerous snippets and reusable blocks to get you started.

## Getting Started

### Creating Stages

To create custom stages for the game, add or modify `.stage` files with StageScript commands in the following directory, located inside the Windows Application Data directory.

`%AppData%\Godot\app_userdata\The Battle of Aurinoxia\stages`

Note: You must launch the game at least once for this directory to exist.

The file must be given a name, such as `my_level.stage` to appear in the game. It is recommended to only use letters (a-Z), numbers (0-9) along with periods, underscores, or dashes (._-) to ensure proper display in game. The game menu displays up to 16 characters per file name, excluding the `.stage` prefix.

### Loading Stages

After creating a StageScript file in the stages directory, launch the game and choose `Play Game` then `Custom Stage` from the menu options. You should see a list of all the custom .stage files you have added to the stages directory. If you do not, check that your path and file naming adheres to the requirements above. Select a stage from the list and progress through the following menus to load and play it. 

### Errors and Logs

If your syntax is correct, loading a custom stage should result in whatever play experience you created. However, if you are provided with a blank screen, or if the Stage Clear immediately loads without anything happening, you may have a syntax or other kind of error. Extensive debugging and error logs are written to the `logs` directory. Consult the files generated there for more details.

`%AppData%\Godot\app_userdata\The Battle of Aurinoxia\logs`

- `debugging.log` contains detailed logs about commands, game timing, and events, and may also contain WARNING's issued when a non-game-breaking command fails to process due to syntax or logic error. Note: This file is only written after the player enters an exit portal spawned using STAGE_CLEAR command. The file contents are overwritten each time this occurs.
- `errors.log` contains details about syntax errors, parsing errors, or other game-breaking events. This file is created or overwritten whenever such and event occurs.

### Testing Tools

When developing stages, it is helpful to be able to quickly iterate on ideas as you write, play, test, and debug your syntax. In the `Settings` menu you can enable several useful keyboard shortcuts to assist with testing.

- (`F5`or `LB`) REPL mode. Clear all pending commands and load the lastest commands from the `repl/repl.stage` file.
- (`F6` or `RB`) Dump logs. Immediately dump logs to the debugging.log file in app data directory without having to proceed to stage exit.