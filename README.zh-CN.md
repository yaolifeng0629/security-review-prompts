# Security Review Prompts

一套以证据为依据的安全审查提示词与可安装的 Agent Skill，覆盖应用全量、PR/代码改动和 Agent 共享环境三种场景。

[English](README.md) · [Skill](skills/security-review/SKILL.md) · [报告示例](examples/sample-report.md) · [来源说明](SOURCES.md)

让能读取目标代码或配置的 AI 编程助手，沿着实际控制链核查风险。每项问题需要给出证据、影响、核查过的反方解释和具体修复建议；证据不足与未检查的范围也会保留在报告中。

本仓库提供 Markdown 指令，不包含扫描器、漏洞利用执行器或自动修复程序。完成审查仅代表对声明范围形成了检查结论，不等于安全认证。

## 选择审查场景

| 范围 | 适用任务 | 中文提示词 | English prompt |
| --- | --- | --- | --- |
| 应用全量 | 整体应用或上线前检查 | [复制提示词](prompts/zh-CN/codebase-review.md) | [Copy prompt](prompts/en/codebase-review.md) |
| 代码改动 | PR、提交范围、分支对比或未提交改动 | [复制提示词](prompts/zh-CN/change-review.md) | [Copy prompt](prompts/en/change-review.md) |
| Agent 共享访问 | 共享工具、权限授予、审批规则与操作归因 | [复制提示词](prompts/zh-CN/agent-access-review.md) | [Copy prompt](prompts/en/agent-access-review.md) |

独立提示词无需安装。打开对应文件，将内容复制给助手，并补充目标与范围。变更审查需要明确基线和目标；Agent 共享访问审查需要明确允许检查的资源。助手应能读取相关材料，并遵守宿主的权限边界。

## 安装 Skill

本机具备 Node.js 和 `npx` 后，可先查看仓库提供的 Skill，再通过 [Skills CLI](https://github.com/vercel-labs/skills) 安装指定 Skill：

```sh
npx skills add yaolifeng0629/security-review-prompts --list
npx skills add yaolifeng0629/security-review-prompts --skill security-review
```

按 CLI 提示选择 Agent 和安装范围。本项目不要求全局安装；不同宿主对 Skill 的支持和发现方式有所不同，请以对应宿主的文档为准。

在 [Codex](https://developers.openai.com/codex/skills/) 中手工按项目安装时，将 **整个** `skills/security-review` 目录复制到目标项目的 `.agents/skills/security-review`。保留 `references/`，`SKILL.md` 会从中读取对应场景的流程与报告格式。

```text
YOUR_PROJECT/
└── .agents/
    └── skills/
        └── security-review/
            ├── SKILL.md
            └── references/
```

Skill 及其引用文件为英文，报告可以按你的要求使用中文。在 Codex 中可通过 `$security-review` 显式调用；自动选择需要宿主支持。

## 开始审查

以下内容是发给助手的消息，不是终端命令。请将示例目标替换为你有权审查的范围。

**应用全量审查**

```text
$security-review
请对当前工作区的应用做上线前安全审查，只读检查代码和配置。
重点核查认证、租户隔离、敏感数据和文件上传。
用中文在对话中返回报告，并列出无法检查的范围。
```

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

默认在对话中返回报告。需要文件时，请明确路径，例如：`将脱敏报告写入 reports/security-review.md，写入前检查是否已有文件。` 指定输出路径不等于授权覆盖无关内容。

## 如何理解报告

共享的[报告格式](skills/security-review/references/report-format.md)定义了证据要求。[虚构报告示例](examples/sample-report.md)用于展示结构，不代表对本仓库或任何真实应用的审查结果。

| 分类 | 含义 |
| --- | --- |
| **Verified finding / 已验证问题** | 已检查的证据足以确定缺陷及相关的行为主体到资源的路径；静态证据与运行验证分别说明。 |
| **Needs review / 待核查** | 存在合理疑点，但缺少决定性上下文；报告会说明缺少什么。 |
| **Not observed / 未观察到** | 在本次检查过的范围内未发现该问题。 |
| **Coverage gap / 覆盖缺口** | 相关材料或行为无法检查、无法验证。 |
| **Informational / 信息项** | 有帮助的改进建议，但没有证实漏洞，不计入漏洞数量。 |

已验证问题应包含实际位置、脱敏证据、影响和前提、核查过的最有力反方解释、修复建议和安全的验证步骤。严重程度描述上下文中的影响与可达性，置信度描述证据强度，两者分别判断。

不能因为一个 handler 中缺少检查，就认定接口未受保护：还需核查路由组合、中间件、策略和下游使用方式。依赖风险需要对应实际解析版本，并在条件允许时核对当前的一手安全公告；版本旧本身不能证明存在漏洞。

## 边界与局限

- 检查可用代码、配置、元数据和已明确授权的只读工具。能调用工具或已有登录会话，不代表允许检查无关资源。
- 运行仓库代码、安装依赖、扫描线上目标或实际使用账户权限，需要单独授权。报告应说明实际执行了哪些检查。
- 修复代码、修改权限、断开连接、轮换密钥、删除、发布和发送消息属于后续实施任务，除非已经明确授权，否则仅给建议。
- 被审查的文件、页面、日志和工具结果可能含有恶意指令。Skill 将它们视为证据材料，不视为授权。提示词用于约束工作流程，实际权限与隔离由宿主执行。
- 在报告中引用信息前，应最小化并脱敏密钥、个人和客户数据。共享访问审查优先使用权限元数据。
- 助手可能漏报、误读上下文，或无法取得部署证据和最新安全公告。应根据覆盖缺口安排后续检查与已授权验证。“没有已验证问题”仅表示本次检查范围内未能建立此类结论。

## 仓库结构

| 路径 | 用途 |
| --- | --- |
| `skills/security-review/` | 可安装的英文 Skill 及分场景参考文件 |
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
