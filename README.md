# CTNH-Modules

[![Build](https://github.com/CTNH-Team/CTNH-Modules/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/CTNH-Team/CTNH-Modules/actions/workflows/build.yml)

Multi core mod workspace for the modpack Create: New Horizon (CTNH).

## Documentation

Module guides are **not** stored in this repository. They live in [CTNH-Docs](https://github.com/CTNH-Team/CTNH-Docs) and are published as the `ctnh-docs` skill (date-versioned releases).

| Scope | Guide |
|-------|-------|
| Cross-module architecture contract | `docs/_architecture/AGENTS.md` |
| Per module | `docs/<Module>/AGENTS.md` |
| Per domain | `docs/<Module>/<domain>/AGENTS.md` |

Read the architecture contract before touching machine, trait, recipe capability, or Jade code: it defines state ownership boundaries, `@DescSynced`/`@Persisted` rules, Jade data minimization, and the module migration steps. Read the matching module guide before editing that module's source.

Get the guides either from the skill release (`ctnh-docs-skill-<date>.zip` on the [CTNH-Docs releases page](https://github.com/CTNH-Team/CTNH-Docs/releases/latest)) or by fetching a raw file:

```text
https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/_architecture/AGENTS.md
https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/<Module>/AGENTS.md
```

Guide edits belong in the CTNH-Docs repository, not here.

## Building

All the core mods are gathered and compiled into single CTNH-Core mod (via jarJar).

```shell
$ git clone --recursive https://github.com/CTNH-Team/CTNH-Modules.git 
```

Then open the folder with IDEA and check out each submodule's own branch (`dev` for the CTNH modules, `ctnh` for `GregTech-Modern`). Use Java 17; IntelliJ builds need the bundled JetBrains Runtime as the Gradle JVM.

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

Most submodules use `dev` as the default branch; `GregTech-Modern` uses `ctnh`. The submodules in this repository will not be updated to the latest commit synchronously, so you may need to pull the latest changes manually.

- [CTNH-Core](https://github.com/CTNH-Team/CTNH-Core): core mod containing other modules, and compatible with GregTech-Modern
- [CTNH-Lib](https://github.com/CTNH-Team/CTNH-Lib): common library for other modules
- [CTNH-Bio](https://github.com/CTNH-Team/CTNH-Bio): biological part, compatible with Biomancy
- [CTNH-Energy](https://github.com/CTNH-Team/CTNH-Energy): applied energistics part, compatible with AE2
- [CTNH-Mana](https://github.com/CTNH-Team/CTNH-Mana): mana(magical) part, compatible with Botania, blood magic, ars nouveau, etc.
- [CTNH-Astral](https://github.com/CTNH-Team/CTNH-Astral): astral part — materials, enchantments, custom worldgen/dimensions, oxygen environment
- [CTPP](https://github.com/CTNH-Team/CTPP): compatible mod between GregTech-Modern and Create
- [Create-Enough-Items](https://github.com/CTNH-Team/Create-Enough-Items): EMI experience integration (sidebar groups, search, recipe-page filters)
- [GregTech-Modern](https://github.com/CTNH-Team/GregTech-Modern): vendored upstream GTCEu on the `ctnh` branch — a dependency and reference, changed only when a task explicitly targets GTCEu internals

## Credits

This multi-module workspace is setup based on [sa-shiro/Minecraft-MultiProject-Template](https://github.com/sa-shiro/Minecraft-MultiProject-Template/tree/main/modules).

## License

Unless otherwise stated, this repository is licensed under the [GNU LGPL v3.0 License](https://www.gnu.org/licenses/lgpl-3.0.html).

### Third-party notice

This repository is based on [sa-shiro/Minecraft-MultiProject-Template](https://github.com/sa-shiro/Minecraft-MultiProject-Template/tree/main/modules), licensed under MIT. The MIT notice for reused template portions is distributed in the `THIRD_PARTY_NOTICES` file.

### Submodule licenses

Each git submodule is an independent project with its own license. For exact terms, see the `LICENSE` file in each submodule repository.
