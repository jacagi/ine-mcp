# ine-estadistica: Claude plugin for Spain's official statistics

An **unofficial** Claude Code plugin that bundles the open-source MCP server published by
Spain's Instituto Nacional de Estadística (INE) at <https://github.com/es-ine/mcp>.
It gives Claude read-only access to official demographic, economic and social statistics
through the INE public JSON API.

> This plugin is a community wrapper. It is **not** made, affiliated with or endorsed by the INE.
> The data and the MCP server belong to their authors; see the upstream repository.

## Install

```
/plugin install ine-estadistica --marketplace jacagi/ine-mcp
```

Or in two steps:

```
/plugin marketplace add jacagi/ine-mcp
/plugin install ine-estadistica@jacagi-ine
```

### Requirements

- [uv](https://docs.astral.sh/uv/) on your `PATH` (`uvx` runs the server).
- Python 3.11+ (uv can install it for you).
- Network access to github.com (first install) and servicios.ine.es / www.ine.es (INE API).

The plugin runs the upstream server locally over stdio with
`uvx --from git+https://github.com/es-ine/mcp.git@<pinned commit> ine-mcp`.
It installs from git on purpose: the `ine-mcp` name on PyPI belongs to an unrelated project.

## What this plugin runs, sends and stores

- **Runs:** one local MCP server over stdio, started with `uvx` from the INE's public repository
  `github.com/es-ine/mcp`, pinned to commit `325742ed0222d306e0094b077d034f1fec375a4d`.
  The plugin contains no code of its own besides this configuration.
- **Fetches on first start:** the server's source from github.com and its Python dependencies
  from PyPI (via uv). They are cached locally by uv.
- **Sends:** only the statistical queries Claude builds (operation, table and variable IDs,
  periods, search terms) to the INE public API at `servicios.ine.es` / `www.ine.es`.
  No API keys, credentials or personal data are needed or sent.
- **Stores:** nothing. The tools are read-only and the plugin keeps no data of its own.
- **Does not:** use hooks, change Claude settings or permissions, or contact any other service.

## Tools

The upstream server exposes five read-only tools, called in order:

1. `ine_select_operation`: find a statistical operation.
2. `ine_select_multiple_tables`: list its tables.
3. `ine_select_multiple_table_groups`: list the variables of the tables.
4. `ine_select_multiple_values`: list the values of those variables.
5. `ine_select_multiple_table_data`: fetch the data.

Example prompt: *"¿Cuál fue la tasa de paro en Andalucía en el último trimestre según la EPA?"*

## Updating

The upstream commit is pinned in `.mcp.json`. To move to a newer upstream version,
update the commit hash there and bump `version` in `.claude-plugin/plugin.json`.

## License

EUPL-1.2, the same as the upstream server.
