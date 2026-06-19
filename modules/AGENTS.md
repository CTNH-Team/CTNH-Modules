# MODULES KNOWLEDGE BASE

## OVERVIEW
`modules/` contains one Gradle subproject per CTNH mod plus the vendored `GregTech-Modern` upstream dependency. CTNH modules share root build logic; each keeps its own mod id, resources, registries, datagen, and optional GTCEu addon.

## STRUCTURE
```text
modules/
├── CTNH-Core/        # aggregate/core mod and release artifact
├── CTNH-Lib/         # shared CTNH helpers and lang provider annotations
├── CTNH-Bio/         # Biomancy/living-machine content
├── CTNH-Energy/      # AE2/energy/quantum computer content
├── CTNH-Mana/        # Botania/Blood Magic/magic content
├── CTNH-Astral/      # astral/worldgen content
├── CTPP/             # Create/GregTech compatibility
├── Create-Enough-Items/ # EMI sidebar/search/recipe-page integration
├── GregTech-Modern/  # vendored GTCEu upstream
└── libs/             # local flatDir jars
```

## WHERE TO LOOK
- Pick a module: `modules/<Module>/AGENTS.md`. Read the child file before editing module code.
- Mod metadata: `modules/<Module>/gradle.properties`, `src/main/resources/META-INF/mods.toml`. `mod_id` drives mixin name, datagen args, jar manifest.
- Main entry: `src/main/java/**/<ModName>.java`. Each CTNH module has one mod entry class.
- GTCEu integration: `*GTAddon.java`. Most CTNH modules register materials/recipes/machines through GT addon hooks.
- Generated data: `src/generated/resources`. Regenerate via `:modules:<Module>:runData`.
- Static assets/data: `src/main/resources`. Hand-authored resources live here.

## REGISTRATION ENTRYPOINTS
- Cross-module rule: common recipes usually belong in `CTNH-Core`; feature modules expose registries/content that Core recipes can consume.
- Standard CTNH pattern: `<Prefix>Registrate` owns the module registrate, `<Prefix>Blocks` / `<Prefix>Items` hold block/item entries, `<Prefix>Machines` / `<Prefix>Multiblock*` hold GTCEu machine entries, `<Prefix>RecipeTypes` and recipe data classes drive generated recipes.
- Ponder pattern: shared `CTNHPonderSceneBuilder` lives in CTNH-Lib; modules provide thin adapters plus their own `PonderPlugin`, scene registration, tag registration, and concrete scene classes.
- Module-specific details live in each child `AGENTS.md`; use this file only to pick the right module.

## CONVENTIONS
- Child modules may have standalone `settings.gradle` / `build.gradle`, but root `settings.gradle` is authoritative for this workspace.
- Package names are not normalized across modules; follow the module's existing namespace.
- Most new recipes should be implemented in `modules/CTNH-Core`; feature modules may expose items/machines/data that Core recipes consume.
- Registry classes tend to use module prefixes: `CTNH*`, `CB*`, `CE*`, `CM*`, `CA*`, `CTPP*`; Create-Enough-Items uses `CEI*`.
- Large multiblock registry files often use `spotless:off/on`; preserve the local formatting boundary.

## ANTI-PATTERNS
- Do not move code between modules without checking Gradle dependencies and mod runtime load order.
- Do not make feature modules depend on CTNH-Core; Core is allowed to depend on modules for aggregate recipes/content, not the reverse.
- Do not put feature Ponder scenes, tags, plugins, or AE2-specific helpers in CTNH-Lib; only shared builder/text framework belongs there.
- Do not edit `modules/libs` as source code; it is for local jar resolution.
- Do not duplicate parent build instructions in child docs; root Gradle scripts are the source of truth.
