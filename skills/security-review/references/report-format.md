# Report Format

Use this format for all review modes. Adapt the report language to the user's request.

## Evidence categories

| Category | Meaning |
| --- | --- |
| Verified finding | Inspected evidence establishes the weakness and the relevant actor-to-resource path. Explicitly distinguish static evidence from runtime verification. |
| Needs review | A plausible concern lacks decisive context. Name the missing evidence and a safe next check. |
| Not observed | The inspected portion did not show the weakness. Describe exactly what was checked. |
| Coverage gap | A relevant resource or behavior was unavailable or untested. It has not passed review. |
| Informational | A useful improvement without a demonstrated vulnerability. Exclude it from vulnerability counts. |

Do not convert an unreachable service, an unread file, or an unavailable account into a negative finding. Keep uncertain items out of verified counts.

## Severity and confidence

Use practical context rather than a keyword score. These are local review priorities, not calculated CVSS scores.

| Severity | Working definition |
| --- | --- |
| Critical | A demonstrated path to catastrophic compromise, such as broad privileged execution or cross-tenant compromise, with few practical barriers in the established context |
| High | A reachable path to substantial unauthorized access, sensitive-data disclosure, or privileged action |
| Medium | A material security impact with meaningful prerequisites or limited scope |
| Low | A small demonstrated impact or substantial prerequisites |

Use High, Medium, or Low confidence to explain evidence strength independently. Reserve Verified finding for established paths; missing decisive controls or reachability belongs in Needs review. Proposed severity for an uncertain item must remain explicitly conditional.

## Report template

~~~markdown
# Security Review

## Summary

- Mode:
- Scope and snapshot:
- Evidence basis: static inspection / authorized runtime checks / configuration metadata
- Verified findings: N (Critical: 0, High: 0, Medium: 0, Low: 0)
- Overall observation, bounded by the inspected scope:

## Verified findings

### F-01: Descriptive title

- Severity:
- Confidence and reason:
- Location: observed file:line or redacted resource/policy identifier
- Evidence: minimal redacted excerpt and relevant controls inspected
- Path: actor/input -> failed control -> affected asset or action
- Impact and prerequisites:
- Opposing explanation checked:
- Recommended fix: smallest suitable change; include a defensive code excerpt when supported
- Verification: safe next check; mark clearly as performed or proposed

If none: "No verified findings in the inspected scope."

## Needs review

- Concern:
- Evidence available:
- Missing decisive evidence:
- Safe next check:

If none, say so.

## Informational observations

If none, say so. Exclude informational items from verified counts.

## Coverage and limits

- Files/resources inspected:
- Checks actually performed and their results:
- Applicable areas where weaknesses were Not observed:
- Coverage gaps and assumptions:
- Changes or live actions performed: none, unless explicitly authorized and disclosed

## Prioritized next actions

List only actions supported by findings or coverage gaps.
~~~

For a code finding, cite real observed lines. For a policy finding without source lines, cite the visible policy/resource and the observation date instead. Never fabricate precision.

If writing a file was requested, use the user-selected location, redact before writing, and do not overwrite an unrelated existing file without permission.
