# Agent Note: Hindsight memory Workshop entry

Status: implemented

## Problem

The Workshop memory category lists local and project-oriented memory plugins, but it does not expose the existing `dsh-hindsight-memory` package. Users who run a shared Hindsight service therefore cannot discover the pipeline-level integration from the DSH settings and Workshop flow, even though the package already provides its own top-level settings section.

## Decision

The community plugin index includes `dsh-hindsight-memory` under `knowledge / memory`, pointing to the author's repository and npm package. The catalog copy states the observable integration contract: recall is injected before the first model call of a turn, retain runs asynchronously after the turn, and the plugin owns a dedicated settings section for enablement, API URL, Bearer API key, Bank ID, and recall budget.

The repository continues to index rather than vendor the third-party implementation. Installation remains the Workshop's normal npm flow, and configuration remains owned by the installed plugin's `settings.section` surface.

## Alternatives considered

- Reimplement Hindsight inside `dsh-web`: rejected because a maintained DSH plugin already implements the pipeline hooks and settings UI; duplicating it would split ownership and security fixes.
- Add static Hindsight fields to the generic Web plugins section without installing a runtime plugin: rejected because a settings form alone cannot provide recall or retain behavior and would create misleading inert configuration.
- Use the generic Hindsight MCP endpoint only: rejected because MCP requires model tool use and does not provide automatic pre-turn recall and post-turn retain semantics.

## Consequences

- Workshop users can discover and install the Hindsight integration from the memory category.
- After installation and DSH restart, the plugin exposes its independent Hindsight settings section and persists values through the DSH settings service.
- API keys remain third-party plugin settings; this repository stores no user credential or deployment-specific URL.
- The community index, generated Workshop manifest, and tests must remain synchronized.
