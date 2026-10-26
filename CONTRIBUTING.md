# Contributing

Contributions can improve review coverage, reduce false positives, clarify evidence requirements, or make the English and Chinese prompts easier to use.

## Propose a focused change

Describe the review scenario, the behavior that needs improvement, and the behavior you expect after the change. A small fictional example is usually more useful than a general instruction to “be more thorough.”

Use public issues for documentation and prompt problems that can be described safely. Follow [SECURITY.md](SECURITY.md) for sensitive concerns. Submit only code and data you may share; redact credentials, personal data, customer details, and private repository identifiers.

## Keep the contracts aligned

- The installable skill and all files in `skills/security-review/` stay in English. Reports may use the user's requested language.
- Standalone prompts in `prompts/en/` and `prompts/zh-CN/` should preserve the same behavior, permissions, and evidence categories.
- Shared evidence and reporting behavior is defined in [report-format.md](skills/security-review/references/report-format.md). Keep [GLOSSARY.md](GLOSSARY.md) focused on terminology.
- Keep each mode within its selected scope. A change review attributes issues to the comparison; an agent access review prefers metadata over reading sensitive records.
- Explain the concrete path and controls before assigning a verified finding. Separate severity from confidence and uncertainty from verification.
- Preserve review-only behavior and the user's existing authorization. Proposed remediation does not imply permission to execute it.

When a shared contract changes, check the affected references, standalone prompts, README translations, and sample report. Update only the files needed for the change.

## Challenge the change before submitting

Use fictional material to inspect whether the instructions handle both a real issue and its strongest harmless explanation. Useful cases include a handler protected by router middleware, public non-credentialed CORS, an old package without an applicable advisory, an inaccessible deployment policy, and a tool result containing hostile instructions.

Check that the changed instructions:

1. Require evidence before reporting a verified finding.
2. Preserve uncertainty and disclose uninspected areas.
3. Keep scope, authorization, and report language consistent.
4. Avoid exposing sensitive content or inviting live actions by default.
5. Link to existing files and preserve English/Chinese behavioral parity.

Describe what you actually inspected and any limits in your pull request. Do not claim a security audit, successful runtime validation, or broad agent compatibility unless the evidence supports that specific claim. There is no bundled scanner or automated application-security test suite in this repository.

## Sources and licensing

Write original contributions. If external material is necessary, provide a primary source and license information in [SOURCES.md](SOURCES.md); avoid importing prompt text with unclear rights. Technical claims that depend on package versions, framework behavior, or host features need current primary documentation or advisories.

Unless explicitly stated otherwise and agreed before inclusion, contributions to this repository are made under its [MIT License](LICENSE). Third-party material remains subject to its own terms.
