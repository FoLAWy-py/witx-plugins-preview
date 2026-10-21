# WitX Plugins Preview

简体中文 | [English](README.en.md)

[WitX Developer Preview](https://github.com/FoLAWy-py/WitX-Preview) 的 Plugin marketplace。Plugin 是一组 Skill：某类任务的操作方法，Agent 在合适时加载。Plugin 只含文字，不运行代码，也不授予权限；输入仍受 WitX 的策略、审批和执行检查约束。

## 安装

在运行 WitX Worker 的机器上，显式添加这个 Preview marketplace：

```sh
witx plugin marketplace add https://github.com/FoLAWy-py/witx-plugins-preview.git
witx plugin install basics@witx-preview
witx plugin list
```

安装前会展示全文，确认后才装。这里的 marketplace 名称是 `witx-preview`，与内置的 `witx` 分开。两边的 Plugin 都叫 `basics`，所以同一份安装中只能保留其中一份；切换前请查看来源。

```sh
witx plugin update basics    # 从已安装版本记录的 marketplace 更新，先展示差异
witx plugin remove basics
```

Plugin 位于 `WITX_PLUGINS_DIR`（默认 `~/.witx/plugins`），锁定 commit 与文件 SHA-256。任务运行时不自动拉取更新，文件与锁文件不符的 Plugin 不加载。请从可信来源安装，并在更新时检查全文或差异。

## 包含的 Plugin

| Plugin | 版本 | Skill | 用途 |
| --- | --- | --- | --- |
| `basics` | 1.1.1 | `basics:test-records` | 创建、编辑并验证明确要求的测试记录，避免改动用户的其他数据 |

仅在测试设备上、有人监督的情况下使用 Preview。Skill 是指导文字，不能保证任务结果；敏感信息、支付验证与最终结果仍需人工检查。

## 编写 Plugin

```text
plugins/<plugin>/
  .witx-plugin/plugin.json
  skills/<skill>/SKILL.md
  README.md                 # 可选
  LICENSE                   # 可选；公开分发时请携带适用许可
```

`plugin.json` 包含 `name`、`version`、`description`。Plugin 只能包含上述文件；脚本、钩子、MCP 服务和其他文件会被拒绝。不要在 Plugin 子目录加入 NOTICE 或额外的文档文件，当前加载器不接受它们；必要的版权声明可以放在 README.md 中。

Skill 以 `---` 包围的头部开始：

| 字段 | 含义 |
| --- | --- |
| `name` | 与目录同名，使用小写字母、数字和短横线；调用名是 `<plugin>:<name>` |
| `description` | 一句话，最多 300 字符，用于判断何时加载 |
| `apps` | 可选，逗号分隔的 App 标识 |
| `tools` | 依赖的 WitX 工具，如 `ask_user, complete_task` |
| `effects` | 依赖的效果，如 `change_data, delete_data` |
| `invoke` | 可选，`auto` 为默认；`user` 表示仅在用户点名时加载 |

正文写方法、核验条件与失败后的恢复方式，不写固定坐标、密钥、个人数据或可执行代码。反引号包围的小写工具和效果名必须在头部声明，且必须在当前 WitX 中存在。改动任何 Plugin 文件都应提升版本。

## 引用其他仓库的 Plugin

marketplace 条目可以使用本仓库的相对路径，也可以指向另一个仓库，但后者必须锁定完整 commit：

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

更新时检查对应 commit 之间的差异，并确认你有权分发相关内容。

## 验证与贡献

在 WitX-Preview 的仓库里运行：

```sh
uv run --locked witx plugin check /path/to/witx-plugins-preview
```

CI 使用公开的 `FoLAWy-py/WitX-Preview` main 检查，无需私有仓库 deploy key。本地路径中的 Plugin 会检查完整内容；外部仓库条目仅检查声明格式，引用的 Plugin 需另外取出并验证。提交前检查版本、声明、许可和隐私，不要提交运行数据或真实设备、账户信息。

## 许可证

本仓库的原创内容采用 [Apache License 2.0](LICENSE)，版权信息见 [NOTICE](NOTICE)。可安装的 `basics` Plugin 自带 LICENSE 与版权声明。软件和 Skill 按“现状”提供，保证与责任条款以 LICENSE 为准。除另有明确约定外，拟合入本项目的贡献同样按 Apache-2.0 提供；提交者应有权提供相应内容。
