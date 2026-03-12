# drupal-planner

Design Drupal implementations before writing code.

drupal-planner is a set of prompt-only Claude Code skills for planning Drupal architectures, content models, taxonomy structures, theme designs, and search configurations. It produces architecture specifications precise enough that developers can implement them without guessing — entity types with relationships, field structures, permission models, cache strategies, and migration paths.

## What You Get

Five slash commands installed into `~/.claude/`:

| Command | Purpose |
|---------|---------|
| `/drupal-planner` | Full 10-phase implementation planning (entity types, config schema, modules, permissions, cache, migrations, theme, tasks) |
| `/drupal-planner.content-model` | Entity types, bundles, paragraphs, field architecture, composition patterns |
| `/drupal-planner.taxonomy` | Vocabularies, term hierarchies, faceted navigation, Views integration |
| `/drupal-planner.theme` | Base theme, components (SDC), CSS, preprocess, assets, accessibility |
| `/drupal-planner.search` | Search API, Solr/Elasticsearch, facets, Views, autocomplete, discovery |

Each sub-planner is focused — use when planning is scoped to one subsystem. Use the main planner when planning spans multiple concerns or a full feature.

## Install

```bash
# Install skills and agents into ~/.claude/
cp -r .claude/skills/* ~/.claude/skills/
cp .claude/agents/*.md ~/.claude/agents/
```

Or with npm:

```bash
npx claude-skills add https://github.com/zivtech/drupal-planner
```

## Quick Start

### Plan a Full Feature

```
/drupal-planner

> Design a new content type "Product Review" with ratings (1-5 stars),
> review text, author, and publication date. Content should be publishable
> immediately or scheduled. Include permissions so only admins approve reviews.
```

Returns a 10-phase plan with entity type definitions, field specifications, permission model, caching strategy, and implementation tasks.

### Plan Content Model Only

```
/drupal-planner.content-model

> We're redesigning our website. Currently we have a flat "Page" content type.
> We need to support Landing Pages, Blog Posts, Case Studies, and Team Member
> profiles. Plan the content model.
```

Returns entity type specifications, bundle decisions, paragraph architecture, and field relationships.

### Plan Taxonomy

```
/drupal-planner.taxonomy

> Design a taxonomy for an e-commerce site. We need to categorize products,
> support multi-level categories, and enable faceted filtering.
```

Returns vocabulary structure, term hierarchy, facet configuration, and Views integration.

### Plan Theme Architecture

```
/drupal-planner.theme

> We're building a Drupal 11 site. Should we use Starterkit, Olivero, or a custom theme?
> Plan the theme architecture including components and CSS strategy.
```

Returns base theme decision, component strategy (SDC vs traditional), CSS architecture, asset management.

### Plan Search

```
/drupal-planner.search

> Plan the search architecture for a content site with 100k+ articles.
> We need faceted search by category, author, and date. What backend?
```

Returns Search API index design, field mapping, processor selection, facet configuration, Views page layout.

## How Sub-Planners Connect to Critics

Each planner has a companion critic that reviews the implementation:

| Planner | Companion Critic | Repo |
|---------|-----------------|------|
| `/drupal-planner` | `drupal-critic` | [zivtech/drupal-critic](https://github.com/zivtech/drupal-critic) |
| `/drupal-planner.content-model` | `content-model-critic` | [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) |
| `/drupal-planner.taxonomy` | `taxonomy-critic` | [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) |
| `/drupal-planner.theme` | `drupal-theme-critic` | [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) |
| `/drupal-planner.search` | `search-discovery-critic` | [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) |

Workflow:

```
Planner designs → Engineer implements → Critic reviews →
  (if REVISE) → Planner plans fixes
```

## Architecture

```
                    /drupal-planner (main)
                Full 10-phase implementation plan
                         │
      ┌──────────────────┼──────────────────┐
      │                  │                   │
      ▼                  ▼                   ▼
.content-model      .taxonomy             .theme
Entity types,      Vocabularies,         Base theme,
bundles,           term hierarchies,     components,
paragraphs,        facets,               CSS, preprocess,
fields             Views integration     assets
      │                  │                   │
      │                  ▼                   │
      │              .search                │
      │          Search API,                │
      └─────────▶Solr/Elasticsearch◀────────┘
               Faceted search,
               content discovery
```

## Sub-Planner Selection

Use this table to pick the right command:

| You want to design... | Use this command |
|-----------------------|------------------|
| Entity types, bundles, paragraphs, field architecture | `/drupal-planner.content-model` |
| Vocabularies, classification, hierarchies, facets | `/drupal-planner.taxonomy` |
| Theme, components, CSS, preprocess, assets | `/drupal-planner.theme` |
| Search indexes, facets, discovery, Views search pages | `/drupal-planner.search` |
| A full feature (touches multiple subsystems) | `/drupal-planner` |
| A Drupal migration or module refactor | `/drupal-planner` |

## Full Documentation

See [AGENTS.md](AGENTS.md) for:
- Detailed agent descriptions
- Companion skill requirements
- Interaction patterns for multi-subsystem projects
- File locations

See [CLAUDE.md](CLAUDE.md) for:
- Architecture decisions
- Drupal-specific principles encoded in the planners
- Editing guidelines for prompt maintenance

## License

Apache License 2.0. See [LICENSE](LICENSE).

## Related Skills

- [drupal-critic](https://github.com/zivtech/drupal-critic) — Review Drupal code for architecture and maintainability
- [zivtech-meta-skills](https://github.com/zivtech/zivtech-meta-skills) — Companion critics (content-model, taxonomy, theme, search)
- [obra/superpowers](https://github.com/obra/superpowers) — Brainstorming, plan writing, TDD, verification
