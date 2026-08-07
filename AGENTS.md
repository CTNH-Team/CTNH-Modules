# PROJECT KNOWLEDGE BASE

**Generated:** 2026-08-07
**Commit:** 24719c7
**Branch:** master

## OVERVIEW
CTNH-Modules is a Gradle multi-project workspace for Create: New Horizon core mods on Minecraft 1.20.1 / Forge 47.4.1 / Java 17. The root build gathers CTNH feature modules, `Create-Enough-Items` EMI integration, and the vendored `GregTech-Modern` dependency so `:modules:CTNH-Core:build` produces the distributable jar. Each module is its own git submodule and owns an independent mod id, namespace, registries, datagen, and optional GTCEu addon.

## STRUCTURE
```text
./
|-- build.gradle              # shared CTNH subproject setup; excludes GregTech-Modern
|-- settings.gradle           # includes modules:* projects
|-- gradle/                   # scripts, version catalogs, Spotless config
|   |-- scripts/              # repositories, deps, Spotless, moddev runs/datagen
|   `-- ctnh.versions.toml    # CTNH dependency versions
|-- modules/                  # one git submodule per CTNH mod; module guides live in the CTNH-Docs repo
|   |-- CTNH-Core/            # aggregate/core mod and release artifact
|   |-- CTNH-Lib/             # shared CTNH helpers and lang provider annotations
|   |-- CTNH-Bio/             # Biomancy/living-machine content
|   |-- CTNH-Energy/          # AE2/energy/quantum computer content
|   |-- CTNH-Mana/            # Botania/Blood Magic/magic content
|   |-- CTNH-Astral/          # astral/worldgen content
|   |-- CTPP/                 # Create/GregTech compatibility
|   |-- Create-Enough-Items/  # EMI sidebar/search/recipe-page integration
|   |-- GregTech-Modern/      # vendored upstream GTCEu; dependency and reference first
|   `-- libs/                 # local flatDir jars
|-- .github/                  # CI and composite checkout/setup action
`-- .run/                     # IntelliJ run configs for module runData/runServer
```

`build/`, `.gradle/`, `run/`, `.omo/`, `.codex/`, and `.claude/` are local build, runtime, index, or agent state. They are not implementation sources. There is no local `docs/` directory: module guides are hosted in the separate `CTNH-Docs` repository and fetched via webfetch only.

## DOCS ACCESS (ctnh-docs skill)
Module guides are NOT stored in this worktree. They are published as the **`ctnh-docs` skill** from the separate repository `CTNH-Team/CTNH-Docs` (released daily with a date-versioned tag like `2026-08-07`).

1. **If the `ctnh-docs` skill is installed** (present in the skills directory): use it directly. Read `docs/<Module>/AGENTS.md` and `docs/<Module>/<domain>/AGENTS.md` from the skill as the authoritative guides.
2. **If the skill is NOT installed**: download it first from the latest release, then use it:
   ```
   https://github.com/CTNH-Team/CTNH-Docs/releases/latest
   ```
   Download the asset `ctnh-docs-skill-<date>.zip`, extract it into the `~/.agents/skills/` directory (the zip contains a `ctnh-docs/` folder with `SKILL.md`).
3. **Fallback when the skill is unavailable or stale**: webfetch the raw guide from CTNH-Docs (also date-versioned on `main`):
   ```
   https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/<Module>/AGENTS.md
   https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/<Module>/<domain>/AGENTS.md
   ```
   Do NOT read guides from the local filesystem or clone the repository into this workspace.

Update flow: edit guides in the `CTNH-Docs` repository, commit and push there; the release workflow packages them into the `ctnh-docs` skill zip. This file's DOMAIN GUIDE ROUTING table is the routing source of truth and must stay in sync.

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Pick a module guide | `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/<Module>/AGENTS.md` | webfetch ONLY; read before editing module code (see DOCS ACCESS) |
| Add/remove a module | `settings.gradle`, `modules/<Name>/` | Also add submodule/content under `modules/` when external |
| Shared build behavior | `build.gradle`, `gradle/scripts/*.gradle` | CTNH modules share plugins; GregTech-Modern is not in `ctnhSubprojects` |
| Dependency versions | `gradle/ctnh.versions.toml`, `modules/GregTech-Modern/gradle/*.toml` | Root version catalogs import both local CTNH and GTCEu catalogs |
| Runtime/datagen args | `gradle/scripts/moddevgradle.gradle` | `runData` writes `src/generated/resources` inside each module |
| Formatting | `gradle/scripts/spotless.gradle`, `spotless/` | Java only; `spotless:off/on` appears around large registries |
| CI workspace prep | `.github/actions/ctnh_prepare_workspace/action.yml` | Checks out CTNH-Modules plus module repos, then sets up Java 17 and Gradle |
| CI release artifact | `.github/workflows/build.yml` | Builds CTNH-Core; renames non-release jars with PR/short-SHA suffixes; uploads `modules/CTNH-Core/build/libs/*.jar` |
| Shared Ponder framework | `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/client/ponder/CTNHPonderSceneBuilder.java` | Core/Energy keep module adapters and scene/tag registrations |

## DOMAIN GUIDE ROUTING
Each CTNH module is a separate Gradle subproject and git submodule. Fetch the matching guide from CTNH-Docs via webfetch before editing the corresponding module.

| Module | Guide (webfetch) | Read before |
|--------|-------------------|-------------|
| CTNH-Core (aggregate/core mod, CI release target) | `docs/CTNH-Core/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Core/AGENTS.md` | Core gameplay systems, GTCEu integration, large machine registries, cross-mod recipes, Core Ponder scenes |
| CTNH-Lib (shared support library) | `docs/CTNH-Lib/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Lib/AGENTS.md` | Shared registrate builders, dynamic datapack, Jade priority, Ponder framework, lang annotations |
| CTNH-Bio (Biomancy/living machines) | `docs/CTNH-Bio/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Bio/AGENTS.md` | Living-machine systems, entity/recipe capabilities, biological machines |
| CTNH-Energy (AE2/energy) | `docs/CTNH-Energy/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Energy/AGENTS.md` | EU storage/keys/P2P, pattern buffer, quantum computer, AE2 mixins |
| CTNH-Mana (Botania/Blood Magic) | `docs/CTNH-Mana/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Mana/AGENTS.md` | Mana multiblocks, rituals, magic integrations, radial UI |
| CTNH-Astral (astral/worldgen) | `docs/CTNH-Astral/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTNH-Astral/AGENTS.md` | Astral materials, enchantments, custom worldgen/dimensions |
| CTPP (Create/GregTech compatibility) | `docs/CTPP/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTPP/AGENTS.md` | Kinetic/electric machines, fan catalyst recipes, recipe builders, recipe type tables |
| Create-Enough-Items (EMI experience) | `docs/Create-Enough-Items/AGENTS.md` → `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/Create-Enough-Items/AGENTS.md` | EMI sidebar/search/recipe-page customization, mixins, static rule JSON |

## OPERATING CONTRACT
1. Read this file and the routed module guide (webfetch from CTNH-Docs) before editing module source.
2. Keep implementation changes in the module's git submodule; keep guidance changes in the `CTNH-Docs` repository, never in this worktree.
3. Treat `src/main/resources` as authored input and `src/generated/resources` as generated output; regenerate with `runData` instead of hand-editing generated files.
4. Run the narrowest relevant Gradle task (`:modules:<Module>:runData`, `spotlessCheck`, or `:modules:CTNH-Core:build`), then inspect generated-resource diffs when applicable.
5. Do not claim a behavior change is verified from compilation alone when the matching runtime surface is available.
6. Never commit `modules/*` subrepository pointer updates from the root repository, even if a task asks for it.

## CODE CLEANUP SCOPE
- Default cleanup scope is source code in the branch diff, not `src/generated/resources/`, `build/`, `run/`, or tool indexes. Module guides are not in this worktree; they live in the `CTNH-Docs` repository.
- Read the routed module guide (webfetch from CTNH-Docs) before evaluating cleanup candidates in a module file.
- Lock behavior with existing or new focused validation before removing non-obvious code; CTNH currently has no test suites outside GregTech-Modern.
- Preserve reflective entry points, GT addon callbacks, Forge event subscribers, generated-data inputs, and compatibility shims unless their replacement is proven.
- Report skipped cleanup candidates and pre-existing issues instead of broadening scope silently.

## CODE MAP
| Symbol | Type | Location | Role |
|--------|------|----------|------|
| `CTNHCore` | mod entry | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/CTNHCore.java` | Main aggregation/core mod entry |
| `CTNHCoreGTAddon` | GT addon | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/CTNHCoreGTAddon.java` | GTCEu integration hook for CTNH-Core; dispatches `data/recipe/**` |
| `CTNHCoreRecipeAddition` | recipe entry | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/data/recipe/CTNHCoreRecipeAddition.java` | `addRecipes()` target; roots all Core recipe generation |
| `CommonProxy` | proxy | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/common/CommonProxy.java` | Registers config, registrate, machines, recipe types, datagen, creative tabs |
| `ProvidableNetHandler` | network trait | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/common/machine/trait/providable_net/ProvidableNetHandler.java` | Providable network handling across Core machines |
| `WideParticleAccelerator` | machine | `modules/CTNH-Core/src/main/java/io/github/cpearl0/ctnhcore/common/machine/multiblock/electric/WideParticleAccelerator.java` | Flagship multiblock; old variant `WPA_old` is a legacy leftover |
| `CTNHLib` | mod entry | `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/CTNHLib.java` | Shared library and registrate helpers |
| `CNRegistrate` | registrate | `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/registrate/CNRegistrate.java` | Extends GTCEu `GTRegistrate`; shared CTNH builders |
| `CTNHDynamicDataPack` | runtime pack | `modules/CTNH-Lib/src/main/java/tech/vixhentx/mcmod/ctnhlib/data/CTNHDynamicDataPack.java` | Runtime pack carrying GT/GMT recipes via `GTDynamicPackContents` |
| `CTPP` | mod entry | `modules/CTPP/src/main/java/com/mo_guang/ctpp/CTPP.java` | Create/GregTech compatibility mod |
| `CTNHAstral` | mod entry | `modules/CTNH-Astral/src/main/java/com/ctnh/ctnhastral/CTNHAstral.java` | Astral materials/worldgen module |
| `OxygenEnvironmentService` | oxygen system | `modules/CTNH-Astral/src/main/java/com/ctnh/ctnhastral/common/oxygen/OxygenEnvironmentService.java` | Oxygen/atmosphere environment handling |
| `CTNHBio` | mod entry | `modules/CTNH-Bio/src/main/java/com/moguang/ctnhbio/CTNHBio.java` | Biomancy/living-machine module |
| `CTNHEnergy` | mod entry | `modules/CTNH-Energy/src/main/java/tech/luckyblock/mcmod/ctnhenergy/CTNHEnergy.java` | AE2/energy integration module |
| `QuantumComputerMultiblockMachine` | machine | `modules/CTNH-Energy/src/main/java/tech/luckyblock/mcmod/ctnhenergy/common/quantumcomputer/machine/QuantumComputerMultiblockMachine.java` | Quantum computer multiblock |
| `CTNHMana` | mod entry | `modules/CTNH-Mana/src/main/java/com/moguang/ctnhmana/CTNHMana.java` | Botania/Blood Magic/magic module |
| `CreateEnoughItems` | mod entry | `modules/Create-Enough-Items/src/main/java/com/ctnh/cei/CreateEnoughItems.java` | EMI sidebar/search/recipe-page integration module |
| `GTAddon` / `IGTAddon` | upstream API | `modules/GregTech-Modern/src/main/java/com/gregtechceu/gtceu/api/addon/` | GTCEu addon API used by CTNH modules |

## CONVENTIONS
- Java 17, UTF-8, Forge 47.4.1, Minecraft 1.20.1, Parchment mappings from root `gradle.properties`.
- CTNH modules apply `net.neoforged.moddev.legacyforge`, Sponge Mixin, Lombok, Spotless, `com.ctnhlang.langprovider`, and Forge access transformers from root `build.gradle`.
- Every CTNH module has `gradle.properties` with `mod_id` / `mod_name`, `src/main/resources/META-INF/mods.toml`, and `${mod_id}.mixins.json`.
- `sourceSets.main.resources` includes both `src/main/resources` and `src/generated/resources`.
- Module `README.md` files mirror root build snippets; prefer root scripts for truth.
- Recipe placement policy: most new recipes should live in CTNH-Core; Core may depend on other CTNH modules, but other modules must not depend on Core.
- Ponder placement policy: reusable scene builder/text helpers live in CTNH-Lib; Core/Energy keep only module adapters, plugins, scene/tag registrations, and module-specific helpers.
- No CTNH test suites are present; only `modules/GregTech-Modern/src/test/java` has tests.
- GT/GMT recipes are runtime dynamic-pack data, not static datagen output: recipes registered through `*GTAddon.addRecipes(Consumer<FinishedRecipe>)` are serialized into the GTCEu runtime dynamic pack (`GTDynamicPackContents` via CTNH-Lib's `CTNHDynamicDataPack`) and injected into the recipe manager at runtime. `runData` therefore produces NO JSON for GT/GMT recipes, and a clean generated-resources tree does not indicate missing recipes. The static `src/generated/resources` tree covers only tags, lang, models, worldgen, and non-GT recipe types; verify GT recipes in-game or via the GTCEu dev recipe dump (`ConfigHolder.dev.dumpRecipes`), not by inspecting `src/generated/resources`.
- Item/block/fluid references MUST use direct registration objects, never string-matching: point at items/blocks/fluids through their static registry fields (`GTMaterials.Iron`, `CTNHBlocks.MY_BLOCK`, `TagPrefix.ingot`, `AEItems.X`, `CBBlocks.X`, `CEItems.X`, `CMItems.X`, `CABlocks.X`, `CTPPBlocks.X`) or their registered `Item`/`Block`/`Fluid`/`ItemLike` values. Do NOT use `ResourceLocation` string parsing (`new ResourceLocation(...)`, `ResourceLocation.parse("mod:id")`) with `ForgeRegistries.ITEMS/BLOCKS/FLUIDS.getValue(...)` or `RegistryObject`-style lookups to point at items, blocks, or fluids. String identifiers are allowed only for ids that have no registration object in this workspace (upstream-mod-only ids, recipe ids, tag keys, dimension ids, and similar non-item/block/fluid lookups).

## ANTI-PATTERNS (THIS PROJECT)
- Do not hand-edit `build/`, `.gradle/`, `run/`, or `src/generated/resources` outputs unless intentionally patching generated artifacts; normally change datagen Java then run `runData`.
- Do not treat `GregTech-Modern` like ordinary CTNH code; it is vendored upstream and should be changed only when the task explicitly targets GTCEu internals.
- Do not assume one package namespace: modules use `io.github.cpearl0`, `com.moguang`, `tech.luckyblock`, `tech.vixhentx`, `com.mo_guang`, and `com.ctnh`. Create-Enough-Items uses `com.ctnh.cei` (not `com.moguang.cei`).
- Do not add new modules only in Gradle; README requires adding content under `modules/` and including it in `settings.gradle`.
- Do not introduce dependencies from feature modules back to CTNH-Core; dependency direction is Core -> modules, not modules -> Core.
- Do not move module-specific Ponder content into CTNH-Lib: scene/tag registrations stay in feature modules, and Energy's `AE2CablePonderHelper` stays in CTNH-Energy.
- Do not commit `modules/*` subrepository pointer updates from the root repository under any circumstances, even if the task asks for it.

## COMMANDS
```text
./gradlew :modules:CTNH-Core:build
./gradlew :modules:CTNH-Core:runData
./gradlew :modules:CTNH-Core:spotlessCheck
./gradlew :modules:<ModuleName>:runData
./gradlew :modules:<ModuleName>:spotlessApply
```

Use Java 17. IntelliJ builds require the bundled JetBrains Runtime as the Gradle JVM.

## CTPP RECIPE TYPES
CTPP registers 8 custom GT recipe types (kinetic/electric via `StressRecipeCapability`), 3 Create fan processing recipes, and 23 wrapped Create/addon recipe builders (vanilla, Diesel Generators, Metallurgy, Vintage). Full table and infrastructure details → webfetch `https://raw.githubusercontent.com/CTNH-Team/CTNH-Docs/main/docs/CTPP/AGENTS.md`.

## NOTES
- CI path filter rebuilds only for `modules/**`, `gradle/**`, Gradle files, wrapper files, and `.github/**`.
- Root `ctnhSubprojects` excludes `:modules:GregTech-Modern`; shared CTNH scripts do not configure GTCEu.
- `moddevgradle.gradle` passes existing mods `gtceu`, `ctpp`, `biomancy`, `expatternprovider`, `botania`, `bloodmagic`, `create_new_age`, `immersive_aircraft`, and `sophisticatedstorage` to datagen.
- Several modules are dirty or untracked in the current worktree; preserve user/submodule changes.
- Module scale (Java files): CTNH-Core 394, CTNH-Mana 253, CTPP 219, CTNH-Bio 175, CTNH-Energy 171, CTNH-Astral 86, CTNH-Lib 57, Create-Enough-Items 31. Guides below use these counts as complexity hints.
- Spelling quirks to preserve: Core mixin package is `mixin/dategen` (not `datagen`); CTPP fan-processing package is `data/recipe/fanprocessing` (no underscore); Mana multiblock dirs are correctly spelled `multiblock` (no legacy `Mutiblock`); Core `data/recipe/tconstruct/` is an empty leftover directory.
- CEI namespace is `com.ctnh.cei`, not `com.moguang.cei`; it has no `*GTAddon` and no GT recipe registration.
