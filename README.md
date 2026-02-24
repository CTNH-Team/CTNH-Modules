# CTNH-Modules

[![Build](https://github.com/CTNH-Team/CTNH-Modules/actions/workflows/build.yml/badge.svg?branch=dev)](https://github.com/CTNH-Team/CTNH-Modules/actions/workflows/build.yml)

Multi core mod workspace for the modpack Create: New Horizon (CTNH).

## Building

All the core mods are gathered and compiled into single CTNH-Core mod (via jarJar).

```shell
$ git clone --recursive https://github.com/CTNH-Team/CTNH-Modules.git 
```

Then open the folder with IDEA, change submodules' branch into `dev`.

If there is something wrong with submodules, try the following commands to reset and update them (or just clone submodules manually).

```shell
$ git submodule deinit --all -f
$ git submodule update --init --recursive
$ git submodule update --remote
```

To add new modules, use git to add them under modules, and include them in `settings.gradle`.

Besides using IDEA, you can also build the mod with Gradle manually:

```shell
$ cd CTNH-Modules   # And you may need to update the submodules manually
$ ./gradlew :modules:CTNH-Core:build            # To build the mod .jar
$ ./gradlew :modules:CTNH-Core:runData          # To generate data
$ ./gradlew :modules:CTNH-Core:spotlessCheck    # To check code formatting
$ ...
```

Nightly builds are available on the [Actions](https://github.com/CTNH-Team/CTNH-Modules/actions/workflows/build.yml) page.

## Submodules

All submodules use `dev` as the default branch. The submodules in this repository will not be updated to the latest commit synchronously, so you may need to pull the latest changes manually.

- [CTNH-Core](https://github.com/CTNH-Team/CTNH-Core): core mod containing other modules, and compatible with GregTech-Modern
- [CTNH-Lib](https://github.com/CTNH-Team/CTNH-Lib): common library for other modules
- [CTNH-Bio](https://github.com/CTNH-Team/CTNH-Bio): biological part, compatible with Biomancy
- [CTNH-Energy](https://github.com/CTNH-Team/CTNH-Energy): applied energistics part, compatible with AE2
- [CTNH-Mana](https://github.com/CTNH-Team/CTNH-Mana): mana(magical) part, compatible with Botania, blood magic, ars nouveau, etc.
- [CTPP](https://github.com/CTNH-Team/CTPP): compatible mod between GregTech-Modern and Create

## Credits

This multi-module workspace is setup based on [sa-shiro/Minecraft-MultiProject-Template](https://github.com/sa-shiro/Minecraft-MultiProject-Template/tree/main/modules).

## License

Unless otherwise stated, this repository is licensed under the [GNU LGPL v2.1 License](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.en.html).

### Submodule licenses

Each git submodule is an independent project with its own license. For exact terms, see the `LICENSE` file in each submodule repository.
