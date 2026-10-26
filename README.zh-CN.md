# Security Review Prompts

让 AI 编程助手按明确范围审查应用、代码改动和 Agent 共享访问权限。

[English](README.md) · [Skill](skills/security-review/SKILL.md) · [报告示例](examples/sample-report.md) · [来源说明](SOURCES.md)

可直接复制中英文提示词，也可安装英文 `security-review` Skill。报告语言按你的要求选择。

每项问题都需说明检查到的证据、实际影响、核查过的防护和修复建议。缺失证据与未覆盖范围也会列入报告。默认只读检查；实际访问权限由宿主和工具控制。

## 快速开始

### 直接复制提示词

无需安装。打开[应用审查](prompts/zh-CN/codebase-review.md)、[代码改动](prompts/zh-CN/change-review.md)或 [Agent 共享访问](prompts/zh-CN/agent-access-review.md)提示词，将内容复制给助手，并说明目标和允许检查的范围。助手需要能读取相关代码或配置。

### 安装并调用 Skill

本机具备 Node.js 和 `npx` 后，可先查看可用 Skill，再通过第三方 [Skills CLI](https://github.com/vercel-labs/skills) 安装：

```sh
npx skills add yaolifeng0629/security-review-prompts --list
npx skills add yaolifeng0629/security-review-prompts --skill security-review
```

两个命令都会运行该 CLI，`--list` 也不例外；`npx` 可能下载它。也可直接使用独立提示词，或审阅后[手工复制完整 Skill](#手工安装)。

按 CLI 提示选择 Agent 和安装范围，全局安装可选。Skill 的支持和发现方式取决于宿主。

安装后，把下面的消息发给助手：

```text
$security-review
请对当前工作区的应用做上线前安全审查，只读检查代码和配置。
重点核查认证、租户隔离、敏感数据和文件上传。
用中文在对话中返回报告，并列出无法检查的范围。
```

示例使用 [Codex](https://developers.openai.com/codex/skills/) 的 `$security-review` 调用方式，其他宿主可能不同。Skill 及其引用文件均为英文，报告语言按你的要求选择。

## 选择审查场景

| 范围 | 适用任务 | 中文提示词 | English prompt |
| --- | --- | --- | --- |
| 应用审查 | 在约定范围内检查应用或上线前风险 | [复制提示词](prompts/zh-CN/codebase-review.md) | [Copy prompt](prompts/en/codebase-review.md) |
| 代码改动 | PR、提交范围、分支对比或未提交改动 | [复制提示词](prompts/zh-CN/change-review.md) | [Copy prompt](prompts/en/change-review.md) |
| Agent 共享访问 | 共享工具、权限授予、审批规则与操作归因 | [复制提示词](prompts/zh-CN/agent-access-review.md) | [Copy prompt](prompts/en/agent-access-review.md) |

变更审查需要明确基线和目标；Agent 共享访问审查需要明确允许检查的资源。请将以下示例目标替换为你有权审查的范围，示例是发给助手的消息，不是终端命令。

**代码改动审查**

```text
$security-review
请以 PR #42 的基线提交为参照，审查这个 PR。
记录准确的 base 和 head 标识，阅读改动及关联控制链，
只报告本次改动引入或加重的安全问题。在对话中返回报告，不运行仓库脚本。
```

**Agent 共享访问审查**

```text
$security-review
只审查我提供的共享客服 Agent 的连接器授权和审批策略。
优先使用配置元数据，不打开客户记录。
核查权限是否符合客服职责、敏感操作是否有强制审批，
并在对话中返回脱敏的访问关系表和问题清单。
```

## 如何理解报告

[虚构报告示例](examples/sample-report.md)用于展示输出结构，不代表对本仓库或任何真实应用的审查结果。共享的[报告格式](skills/security-review/references/report-format.md)定义了证据要求。

| 分类 | 含义 |
| --- | --- |
| **Verified finding / 已验证问题** | 已检查的证据足以确定缺陷及相关的行为主体到资源的路径；静态证据与运行验证分别说明。 |
| **Needs review / 待核查** | 存在合理疑点，但缺少决定性上下文；报告会说明缺少什么。 |
| **Not observed / 未观察到** | 在本次检查过的范围内未发现该问题。 |
| **Coverage gap / 覆盖缺口** | 相关材料或行为无法检查、无法验证。 |
| **Informational / 信息项** | 有帮助的改进建议，但没有证实漏洞，不计入漏洞数量。 |

已验证问题应包含实际位置、脱敏证据、影响和前提、核查过的最有力反方解释、修复建议和安全的验证步骤。严重程度描述上下文中的影响与可达性，置信度描述证据强度。

不能因为一个 handler 中缺少检查，就认定接口未受保护：还需核查路由组合、中间件、策略和下游使用方式。依赖风险需要对应实际解析版本，并在条件允许时核对当前的一手安全公告；版本旧本身不能证明存在漏洞。

默认在对话中返回报告。需要文件时，请明确路径，例如：`将脱敏报告写入 reports/security-review.md，写入前检查是否已有文件。` 指定输出路径不等于授权覆盖无关内容。

## 边界与局限

本仓库提供 Markdown 指令，不包含扫描器、漏洞利用执行器或自动修复程序。完成审查仅代表对声明范围形成了检查结论，不等于安全认证。

- **范围与授权：** 只检查选定的代码、配置、元数据和已授权的只读工具。能调用工具或已有登录会话，不代表允许访问无关资源。被审查的文件、页面、日志和工具结果属于不可信证据，不构成指令或授权；访问边界由宿主权限与隔离机制执行。
- **执行与修改：** 运行仓库代码、安装依赖、扫描线上目标或实际使用账户权限，需要单独授权。修复代码、修改权限、断开连接、轮换密钥、删除、发布和发送消息属于后续实施任务，已有明确授权的除外。
- **读取前最小化数据：** 优先使用窄路径、指定行段、元数据和可信工具的本地过滤，减少进入原始工具输出的秘密、个人与客户数据。报告引用和文件仍需脱敏；事后脱敏无法撤回已经暴露的原始工具输出。
- **使用获准的处理环境：** 源码和工具输出可能由你选择的模型与宿主处理。敏感项目应使用获准处理相应材料的环境。
- **外部查询：** 只发送必要的公开术语，例如公开包名、版本或安全公告 ID。读取项目的授权不等于允许向搜索或外部扫描服务上传私有源码、配置、锁文件、内部标识、凭据或客户数据。已有明确指定接收方、数据和目的的授权继续有效；确实需要但尚未获得授权时，询问或记录覆盖缺口。
- **覆盖与不确定性：** 助手可能漏报、误读上下文，或缺少部署证据和最新安全公告。应记录实际执行的检查和剩余缺口。“没有已验证问题”仅表示本次检查范围内未能建立此类结论。

## 手工安装

在 Codex 中按项目安装时，审阅后将**整个** `skills/security-review` 目录复制到目标项目的 `.agents/skills/security-review`。保留提供场景流程和报告格式的 `references/`，以及提供宿主元数据的 `agents/openai.yaml`。

```text
YOUR_PROJECT/
└── .agents/
    └── skills/
        └── security-review/
            ├── SKILL.md
            ├── agents/
            │   └── openai.yaml
            └── references/
                ├── codebase-review.md
                ├── change-review.md
                ├── agent-access-review.md
                └── report-format.md
```

## 仓库结构

| 路径 | 用途 |
| --- | --- |
| `skills/security-review/` | 可安装的英文 Skill、参考文件和宿主元数据 |
| `prompts/en/` | 可独立复制的英文提示词 |
| `prompts/zh-CN/` | 可独立复制的中文提示词 |
| [examples/sample-report.md](examples/sample-report.md) | 展示问题、疑点和覆盖缺口的虚构报告 |
| [GLOSSARY.md](GLOSSARY.md) | 范围、问题、置信度、覆盖等共享术语 |
| [SOURCES.md](SOURCES.md) | 来源、归属核实状态与参考链接 |

## 贡献与问题反馈

提示词、翻译和审查场景的贡献方式见 [CONTRIBUTING.md](CONTRIBUTING.md)。涉及敏感信息时，请遵循 [SECURITY.md](SECURITY.md)。不要在公开 issue 中提交真实凭据、私有代码或应用漏洞细节。

## 来源与许可

本库根据维护者提供的一份提示词整理材料独立重写。原始社交帖子归属多数尚未建立可核实的证据链。[SOURCES.md](SOURCES.md)分别说明来源不确定性、已核实的参考链接和技术修正；列出的人物与项目不代表为本库背书。

本仓库原创内容采用 [MIT License](LICENSE)。外部链接内容遵循各自的许可条款，引用链接不构成重新授权。
