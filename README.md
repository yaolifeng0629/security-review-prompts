# Security Review Prompts

Evidence-backed security review prompts and an installable agent skill for applications, code changes, and shared AI agent environments.

[简体中文](README.zh-CN.md) · [Skill](skills/security-review/SKILL.md) · [Sample report](examples/sample-report.md) · [Sources](SOURCES.md)

Use an AI coding assistant with access to the selected code or configuration to trace plausible risks through the actual controls. Get findings with evidence, impact, an opposing explanation, and a focused remediation suggestion. Uncertain concerns and uninspected areas stay visible in the report.

This repository contains Markdown instructions. It does not include a scanner, an exploit runner, or automatic remediation. A completed review is evidence about its stated scope, not a security certification.

## Choose a review

| Scope | Use it for | English prompt | 中文提示词 |
| --- | --- | --- | --- |
| Codebase | Application-wide or pre-release inspection | [Copy prompt](prompts/en/codebase-review.md) | [复制提示词](prompts/zh-CN/codebase-review.md) |
| Change | A PR, commit range, branch comparison, or uncommitted work | [Copy prompt](prompts/en/change-review.md) | [复制提示词](prompts/zh-CN/change-review.md) |
| Agent access | Shared tools, configured grants, approval rules, and attribution | [Copy prompt](prompts/en/agent-access-review.md) | [复制提示词](prompts/zh-CN/agent-access-review.md) |

The standalone prompts work without installation. Open one file, copy its contents into your assistant, and supply the target and scope. A change review needs a baseline and target; an agent access review needs an explicit list of permitted resources. Use a tool that can inspect the relevant material, and respect its permission boundaries.

## Install the skill

With Node.js and `npx` available, inspect the skills exposed by this repository, then install the named skill using the [Skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add yaolifeng0629/security-review-prompts --list
npx skills add yaolifeng0629/security-review-prompts --skill security-review
```

Follow the CLI's prompts to select your agent and installation scope. This project does not require a global installation. Agent support and discovery depend on the host; consult its skill documentation.

For a manual project installation in [Codex](https://developers.openai.com/codex/skills/), copy the **entire** `skills/security-review` directory into your target project's `.agents/skills/security-review` directory. Preserve `references/`, because `SKILL.md` loads its mode-specific instructions and report format from there.

```text
YOUR_PROJECT/
└── .agents/
    └── skills/
        └── security-review/
            ├── SKILL.md
            └── references/
```

The skill and its references are written in English. Report language follows your request, so the same skill can produce a Chinese report. In Codex, invoke it with `$security-review`; automatic skill selection is available when the host supports it.

## Run a review

These are messages to the assistant, not shell commands. Replace the sample targets with your own authorized scope.

**Application review**

```text
$security-review
Review the application in this workspace before release. Inspect code and configuration
read-only. Focus on authentication, tenant isolation, sensitive data, and file uploads.
Return the report here in English and list any areas you could not inspect.
```

**Change review**

```text
$security-review
Review PR #42 against its base commit. Record the exact base and head identifiers,
read the changed files and connected controls, and report security issues introduced
or worsened by this change. Return the report here. Do not run repository scripts.
```

**Agent access review**

```text
$security-review
Review only the connector grants and approval policies I provide for the shared
support agent. Use configuration metadata rather than opening customer records.
Check whether the grants match its support responsibilities and whether sensitive
actions require enforced approval. Return a redacted access map and findings here.
```

The report stays in the conversation by default. To request an artifact, specify a path, for example: `Write the redacted report to reports/security-review.md; inspect any existing file before writing.` An output path does not authorize replacing unrelated content.

## What the report means

The shared [report format](skills/security-review/references/report-format.md) defines the evidence contract. The [fictional sample report](examples/sample-report.md) demonstrates the structure; it is not a report about this repository or a real application.

| Category | Meaning |
| --- | --- |
| **Verified finding** | Inspected evidence establishes a weakness and the relevant actor-to-resource path. Static evidence and runtime verification are distinguished. |
| **Needs review** | A plausible concern lacks decisive context. The report names the missing evidence. |
| **Not observed** | The weakness was not found in the inspected portion of the selected scope. |
| **Coverage gap** | Relevant material or behavior could not be inspected or validated. |
| **Informational** | A useful improvement without a demonstrated vulnerability; excluded from vulnerability counts. |

Verified findings include observed locations, redacted evidence, impact and prerequisites, the strongest opposing explanation checked, a proposed fix, and a safe verification step. Severity reflects contextual impact and reachability; confidence reflects evidence strength. They are separate judgments.

A missing check in one handler is not enough: the reviewer must inspect the composed route, middleware, policies, and downstream use. Dependency claims require a resolved version and an applicable current primary advisory where available. Version age alone does not establish a vulnerability.

## Boundaries and limitations

- Reviews use available code, configuration, metadata, and explicitly authorized read-only tools. Access to a tool or signed-in session does not authorize inspecting unrelated resources.
- Running repository code, installing dependencies, scanning live targets, and exercising account capabilities need separate authorization. The reviewer records what was actually checked.
- Fixes, permission changes, disconnections, secret rotation, deletions, publishing, and messages are proposed for a separate implementation task unless already authorized.
- Reviewed files, pages, logs, and tool results may contain hostile instructions. The skill treats them as evidence, not authority. Prompt instructions support the workflow; host permissions and isolation enforce it.
- Secrets and personal or customer data are minimized and redacted before appearing in reports. Shared-access reviews prefer grant metadata over record contents.
- An assistant can miss flaws, misread context, or lack deployment evidence and current advisories. Use the reported coverage gaps to plan further review and authorized validation. No verified findings means only that none were established in the inspected scope.

## Repository map

| Path | Purpose |
| --- | --- |
| `skills/security-review/` | Installable English skill and its mode/reference files |
| `prompts/en/` | Standalone English prompts |
| `prompts/zh-CN/` | Standalone Chinese prompts |
| [examples/sample-report.md](examples/sample-report.md) | Fictional report illustrating findings and uncertainty |
| [GLOSSARY.md](GLOSSARY.md) | Shared terms for scope, findings, confidence, and coverage |
| [SOURCES.md](SOURCES.md) | Provenance, attribution status, and reference links |

## Contribute and report issues

See [CONTRIBUTING.md](CONTRIBUTING.md) for changes to prompts, translations, and review scenarios. For sensitive concerns, follow [SECURITY.md](SECURITY.md). Keep real credentials, private code, and application vulnerability details out of public issues.

## Sources and license

The scenarios were independently rewritten from a maintainer-supplied compilation. Original social-post attribution was not established for most entries. [SOURCES.md](SOURCES.md) separates that uncertainty from verified reference links and explains the technical corrections. Listed people and projects do not endorse this repository.

Original material in this repository is available under the [MIT License](LICENSE). Linked third-party material remains subject to its own terms; linking does not relicense it.
