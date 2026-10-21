# WitX Plugins Preview

[简体中文](README.md) | English

A plugin marketplace for [WitX Developer Preview](https://github.com/FoLAWy-py/WitX-Preview). A plugin groups Skills: guidance an agent loads when it fits a task. Plugins contain text only, run no code, and grant no permission. WitX still applies its policy, approval, and execution checks.

## Install

On the machine running the WitX Worker, explicitly add this Preview marketplace:

```sh
witx plugin marketplace add https://github.com/FoLAWy-py/witx-plugins-preview.git
witx plugin install basics@witx-preview
witx plugin list
```

WitX displays the contents before asking you to confirm installation. This marketplace is named `witx-preview`, separate from the built-in `witx`. Both offer a plugin named `basics`, so an installation can keep only one of them at a time. Check the source before switching.

```sh
witx plugin update basics    # Updates from its recorded marketplace after showing the diff
witx plugin remove basics
```

Plugins live under `WITX_PLUGINS_DIR` (default: `~/.witx/plugins`), pinned to a commit and file SHA-256 digest. Tasks do not automatically fetch updates. A plugin whose files differ from its lock is not loaded. Install from trusted sources and review the contents or diff when updating.

## Included plugins

| Plugin | Version | Skill | Purpose |
| --- | --- | --- | --- |
| `basics` | 1.1.1 | `basics:test-records` | Create, edit, and verify explicitly requested test records without changing the user's other data |

Use the Preview on test devices under human supervision. Skills provide guidance and do not guarantee results. Sensitive information, payment verification, and final results still require human attention.

## Write a plugin

```text
plugins/<plugin>/
  .witx-plugin/plugin.json
  skills/<skill>/SKILL.md
  README.md                 # Optional
  LICENSE                   # Optional; include the applicable license when distributing
```

`plugin.json` contains `name`, `version`, and `description`. A plugin may contain only the files above. Scripts, hooks, MCP servers, and other files are rejected. Do not add a NOTICE or extra documentation files inside a plugin: the current loader does not accept them. Put required copyright notices in its README.md instead.

A Skill starts with a header enclosed by `---` lines:

| Field | Meaning |
| --- | --- |
| `name` | Matches the folder name; lowercase letters, digits, and dashes. Its invocation name is `<plugin>:<name>` |
| `description` | One sentence, at most 300 characters, used to decide when to load it |
| `apps` | Optional comma-separated app identifiers |
| `tools` | Required WitX tools, such as `ask_user, complete_task` |
| `effects` | Required effects, such as `change_data, delete_data` |
| `invoke` | Optional: `auto` is the default; `user` loads only when the user names the Skill |

Describe the method, verification criteria, and recovery steps. Do not include fixed coordinates, credentials, personal data, or executable code. Lowercase tool and effect names in backticks must be declared in the header and exist in the current WitX build. Bump the version whenever any plugin file changes.

## Reference plugins in other repositories

Marketplace entries can use a relative path within this repository or reference another repository. External references must pin a full commit:

```json
{
  "name": "their-plugin",
  "source": {
    "git": "https://github.com/example/their-plugins.git",
    "commit": "<40-character commit SHA>",
    "path": "plugins/their-plugin"
  }
}
```

Review the differences between commits when updating and confirm you have the right to distribute the content.

## Validation and contributions

From the WitX-Preview repository, run:

```sh
uv run --locked witx plugin check /path/to/witx-plugins-preview
```

CI checks against the public `FoLAWy-py/WitX-Preview` main branch, without a private repository deploy key. Plugins at local paths have their full contents checked. External repository entries have only their declaration format checked; fetch and validate those plugins separately. Before contributing, check versions, declarations, licenses, and privacy. Do not submit runtime data or real device or account information.

## License

Original content in this repository is licensed under the [Apache License 2.0](LICENSE). See [NOTICE](NOTICE) for copyright information. The installable `basics` plugin includes its own LICENSE and copyright notice. The software and Skills are provided on an "AS IS" basis; see LICENSE for warranty and liability terms. Unless explicitly agreed otherwise, contributions intended for inclusion are also provided under Apache-2.0. Contributors must have the right to submit the content.
