# PROJECT KNOWLEDGE BASE

**Generated:** 2026-05-27 Asia/Shanghai
**Commit:** 1d428a1
**Branch:** master

## OVERVIEW
CTNH-Modules is a Gradle multi-project workspace for Create: New Horizon core mods on Minecraft 1.20.1 / Forge 47.4.1 / Java 17. The root build gathers CTNH modules and the vendored `GregTech-Modern` dependency so `:modules:CTNH-Core:build` produces the distributable jar.

## STRUCTURE
```text
CTNH-Modules/
├── build.gradle              # shared CTNH subproject setup; excludes GregTech-Modern
├── settings.gradle           # includes modules:* projects
├── gradle/scripts/           # repositories, deps, Spotless, moddev runs/datagen
├── modules/                  # one mod per subproject; see modules/AGENTS.md
├── modules/GregTech-Modern/  # vendored upstream GTCEu; dependency and reference first
├── .github/workflows/        # CI builds CTNH-Core only
└── .run/                     # IntelliJ run configs for module runData/runServer
```

## WHERE TO LOOK
- Add/remove module: `settings.gradle`, `modules/<Name>/`. Also add submodule under `modules/` when external.
- Shared build behavior: `build.gradle`, `gradle/scripts/*.gradle`. CTNH modules share plugins; GregTech-Modern is not in `ctnhSubprojects`.
- Dependency versions: `gradle/ctnh.versions.toml`, `modules/GregTech-Modern/gradle/*.toml`. Root version catalogs import both local CTNH and GTCEu catalogs.
- Runtime/datagen args: `gradle/scripts/moddevgradle.gradle`. `runData` writes `src/generated/resources`.
- Formatting: `gradle/scripts/spotless.gradle`, `spotless/`. Java only; `spotless:off/on` appears around large registries.
- CI release artifact: `.github/workflows/build.yml`. Runs `./gradlew :modules:CTNH-Core:build`; uploads `modules/CTNH-Core/build/libs/*.jar`.
- Shared Ponder framework: `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/client/ponder/CTNHPonderSceneBuilder.java`. Core/Energy keep module adapters and scene/tag registrations.

## CODE MAP
- `CTNHCore`: mod entry at `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/CTNHCore.java`; main aggregation/core mod entry.
- `CTNHCoreGTAddon`: GT addon at `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/CTNHCoreGTAddon.java`; GTCEu integration hook for CTNH-Core.
- `CTNHLib`: mod entry at `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/CTNHLib.java`; shared library and registrate helpers.
- `CTPP`: mod entry at `modules/CTPP/src/main/java/com/mo_guang/ctpp/CTPP.java`; Create/GregTech compatibility mod.
- `GTAddon` / `IGTAddon`: upstream API at `modules/GregTech-Modern/src/main/java/com/gregtechceu/gtceu/api/addon/`; GTCEu addon API used by CTNH modules.

## CONVENTIONS
- Java 17, UTF-8, Forge 47.4.1, Minecraft 1.20.1, Parchment mappings from root `gradle.properties`.
- CTNH modules apply `net.neoforged.moddev.legacyforge`, Sponge Mixin, Lombok, Spotless, `com.ctnhlang.langprovider`, and Forge access transformers from root `build.gradle`.
- Every CTNH module has `gradle.properties` with `mod_id` / `mod_name`, `src/main/resources/META-INF/mods.toml`, and `${mod_id}.mixins.json`.
- `sourceSets.main.resources` includes both `src/main/resources` and `src/generated/resources`.
- Module `README.md` files mirror root build snippets; prefer root scripts for truth.
- Recipe placement policy: most new recipes should live in CTNH-Core; Core may depend on other CTNH modules, but other modules must not depend on Core.
- Ponder placement policy: reusable scene builder/text helpers live in CTNH-Lib; Core/Energy keep only module adapters, plugins, scene/tag registrations, and module-specific helpers.
- No CTNH test suites are present; only `modules/GregTech-Modern/src/test/java` has tests.

## ANTI-PATTERNS (THIS PROJECT)
- Do not hand-edit `build/`, `.gradle/`, `run/`, or `src/generated/resources` outputs unless intentionally patching generated artifacts; normally change datagen Java then run `runData`.
- Do not treat `GregTech-Modern` like ordinary CTNH code; it is vendored upstream and should be changed only when the task explicitly targets GTCEu internals.
- Do not assume one package namespace: modules use `io.github.cpearl0`, `com.moguang`, `tech.luckyblock`, `tech.vixhentx`, `com.mo_guang`, and `com.ctnh`.
- Do not add new modules only in Gradle; README requires adding under `modules/` and including in `settings.gradle`.
- Do not introduce dependencies from feature modules back to CTNH-Core; dependency direction is Core -> modules, not modules -> Core.
- Do not move module-specific Ponder content into CTNH-Lib: scene/tag registrations stay in feature modules, and Energy's `AE2CablePonderHelper` stays in CTNH-Energy.

## COMMANDS
```bash
./gradlew :modules:CTNH-Core:build
./gradlew :modules:CTNH-Core:runData
./gradlew :modules:CTNH-Core:spotlessCheck
./gradlew :modules:<ModuleName>:runData
./gradlew :modules:<ModuleName>:spotlessApply
```

## CTPP RECIPE TYPES
CTPP registers 8 custom GT recipe types (kinetic/electric via `StressRecipeCapability`), 3 Create fan processing recipes, and 23 wrapped Create/addon recipe builders (vanilla, Diesel Generators, Metallurgy, Vintage). Full table and infrastructure details → [modules/CTPP/AGENTS.md](modules/CTPP/AGENTS.md).

## NOTES
- CI path filter rebuilds only for `modules/**`, `gradle/**`, Gradle files, wrapper files, and `.github/**`.
- Root `ctnhSubprojects` excludes `:modules:GregTech-Modern`; shared CTNH scripts do not configure GTCEu.
- `moddevgradle.gradle` passes existing mods `gtceu`, `ctpp`, `biomancy`, `expatternprovider`, `botania`, `bloodmagic`, and `create_new_age` to datagen.
- Several modules are dirty or untracked in the current worktree; preserve user/submodule changes.
