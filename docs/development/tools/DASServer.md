# DAS Server

This is a simple reference implementation of a oneM2M Dynamic Authorization Server (DAS), as described in TS-0003, clause 7.3. It registers itself as an &lt;AE> with one or more CSEs and, when consulted for a dynamic authorization decision, evaluates a set of configurable access control rules to determine which privileges to grant.

The tool that is provided in the directory [tools/DAS server](https://github.com/ankraft/ACME-oneM2M-CSE/blob/master/tools/DAS%20server){target=_new} is standalone: it does not depend on the ACME CSE's own Python package and can be run separately, e.g. on a different host.

## Setup

Install the required packages from the `tools/DAS server` directory:

```bash title="Install Requirements"
pip install -r requirements.txt
```

## Running

Start the server by running the command from the `tools/DAS server` directory:

```bash title="Start the DAS Server"
python3 -m dass --config config.ini
```

By default the server reads `config.ini` from the current directory; use `--config` to point it at a different file (see [Configuration](#configuration) below).

## Command Line Arguments

| Command Line Argument                | Description                                                            |
|---------------------------------------|--------------------------------------------------------------------------|
| -h, --help                            | Show a help message and exit.                                           |
| --config &lt;file>, -c &lt;file>      | Configuration file to use (default: `config.ini`).                      |
| --no-deregister, -ndr                 | Do not deregister the &lt;AE> from the CSE(s) on shutdown.               |
| --ignore-registration-error, -ire     | Continue running even if registering with a CSE fails.                  |
| --log-level &lt;level>, -l &lt;level> | Log level: DEBUG, INFO, WARNING, ERROR, or CRITICAL (default: INFO).    |

## Configuration

The server is configured through an INI file (see the provided `config.ini` for a full example):

| Section              | Description                                                                                          |
|-----------------------|-------------------------------------------------------------------------------------------------------|
| `[general]`           | General settings, e.g. the `host` name used to build the notification and PoA addresses.             |
| `[notifications]`     | The server's own listening `interface` and `port`, and the `endpoint` path it receives requests on.  |
| `[rules]`             | Where to find the access control rule files, and how often to check them for changes (see below).    |
| `[cse.<name>]`        | One section per CSE to register with: its `cseId`, `httpAddress`, the &lt;AE> to register, etc. Add as many of these sections as needed to register with multiple CSEs. |

## Access Control Rules

Access control rules are defined in YAML files in the directory configured via `[rules] path` (`rules` by default):

- **`_common.yaml`** - rules that apply to every CSE.
- **`<cseId>.yaml`** - rules that apply only to the CSE with that CSE-ID (without its leading "/"), e.g. `id-in.yaml` for CSE-ID `/id-in`.

Both `_common.yaml` and the matching per-CSE file are evaluated for every request; every rule whose `match` conditions are satisfied contributes its `grant` block to the response. A file with a syntax error is logged and skipped without affecting the others, and all files are re-checked for changes every `[rules] reloadInterval` seconds (0 disables this).

Each rule looks like this:

```yaml title="Example Access Control Rule"
rules:
  - match:
      originator: "Csensor*"      # optional, glob pattern, default: matches any
      resourceID: "id-in/cnt-*"    # optional, glob pattern, default: matches any
      resourceType: [cnt, 4]       # optional, name and/or number, default: matches any
      operation: [R, U]            # optional, C/R/U/D/N/F, default: matches any
    grant:
      acor: ["Csensor*"]
      acop: [R, U]                 # optional, defaults to match.operation
```

`operation`/`acop` use single, case-insensitive letters for the oneM2M permissions: `C`reate, `R`etrieve, `U`pdate, `D`elete, `N`otify, `F` (discovery/"find"). `resourceType` accepts either the numeric resource type or its short name (also case-insensitive), for a small set of the most commonly used base resource types.

See the `rules/_common.yaml` and `rules/id-in.yaml` files in the tool's directory for further, complete examples.
