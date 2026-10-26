# Sources and Attribution

## Provenance

This repository independently rewrites three review scenarios from a maintainer-supplied Chinese compilation: application security, code-change security, and shared AI agent access. It does not reproduce the compilation's collected prompt bodies or the TRAP prompt.

The compilation was reviewed on **2026-10-26**. Its SHA-256 digest is:

```text
1261041E7C302D5939F66D736835EC04B554C6924DEDB31F602C44BDA182AF25
```

The digest identifies the input reviewed; it does not establish authorship, rights, completeness, or the accuracy of its claims. The source compilation is not distributed here.

## Attribution status of the supplied compilation

The names below record claims made by the input material. They are not verified author credits for this repository's rewritten text. No endorsement or affiliation is implied.

| Input label | Status as of 2026-10-26 | Treatment here |
| --- | --- | --- |
| Chinese application review attributed to `@JianingXiong` | Original post URL and authorship not established | Scenario independently rewritten; no prompt body reproduced |
| Shared Grok Bot / Agent audit attributed to Michael Fenech | Original post URL and authorship not established | Scenario independently rewritten; no prompt body reproduced |
| “Pre-Release Security Audit Prompt” | Author, original publication, and license not established | Scenario independently rewritten; no prompt body reproduced |
| Claude audit attributed to AI Edge | Original post URL and authorship not established | Scenario independently rewritten; no prompt body reproduced |
| “Guardrails for AI Coders” PR review | Original publication and license not established | Scenario independently rewritten; no prompt body reproduced |
| Change review attributed to Prompthackers / Claude Code style | Original publication and authorship not established | Scenario independently rewritten; no prompt body reproduced |
| TRAP (`bikr/TRAP`) | Repository existence, default branch `main`, and MIT license metadata checked through the GitHub API | External project reference only; no prompt body reproduced |

[TRAP on GitHub](https://github.com/bikr/TRAP) is a separate project. This repository does not repeat the input's claims that it is the “strictest” or “most complete,” nor its claim about a ten-person review committee. Those descriptions were not established during source verification.

## Reference material

These links support installation or technical review concepts. They do not establish the authorship of the collected social prompts.

| Reference | Used for |
| --- | --- |
| [Codex skills documentation](https://developers.openai.com/codex/skills/) | Skill directory layout, explicit invocation, and host-supported discovery |
| [Vercel Labs Skills CLI](https://github.com/vercel-labs/skills) | Repository skill listing and selection through `add --list` and `add --skill` |
| [npm exec documentation](https://docs.npmjs.com/cli/v11/commands/npm-exec) | Execution of npm package commands by npx, including packages fetched into the npm cache |
| [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) | Distinguishing password hashing from reversible storage of recoverable data |
| [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) | Context-dependent review of authenticated browser requests and CSRF defenses |

Installation references were checked on 2026-10-26. External documentation can change; consult the linked primary sources for current host and CLI behavior. A dependency-specific review still needs the relevant current primary advisory and resolved package version.

## Editorial and technical decisions

The rewrite retains the three practical review scopes while establishing one evidence contract:

- Passwords need suitable salted password hashing; recoverable sensitive data requires its own storage and key-management analysis.
- Browser token storage and cookie use require a threat model and CSRF context. A storage API name alone is not a finding.
- CORS risk depends on the resource, credentials, and browser behavior; a wildcard on public non-credentialed content is not automatically a vulnerability.
- Authentication and authorization must be traced through the composed control chain rather than inferred from one missing local check.
- Old dependencies require resolved-version and advisory evidence. “Outdated” is not interchangeable with “vulnerable.”
- Uncertainty, uninspected resources, and informational improvements stay separate from verified findings. An absence of findings in a bounded review does not establish overall security.
- Review requests authorize inspection within the selected scope, not automatic fixes, credential revocation, account actions, or destructive verification.

The normative instructions are in [SKILL.md](skills/security-review/SKILL.md) and its references. [GLOSSARY.md](GLOSSARY.md) defines the shared terms; this file records provenance rather than a second workflow.

## License boundaries

Original prompts, skill instructions, examples, translations, and documentation created for this repository are distributed under [MIT](LICENSE). Linked documentation, social posts, and external projects retain their own terms. This repository's license does not grant rights to third-party content, and an unverified attribution is not a license grant.

Contributions should be original or carry a clear compatible license and provenance. Suggest corrections to attribution with a primary source URL and a short explanation, without reposting material whose rights are unclear.
