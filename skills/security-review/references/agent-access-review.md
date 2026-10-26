# Agent Access Review

Use this mode for a shared AI agent environment, connected tools, browser sessions, shared storage, or approval boundaries. Inspect only resources included in the user's authorization. Do not enumerate unrelated accounts because they happen to be accessible.

## Build a minimal access map

Record agent roles, the shared resource or connector, the access level, the reason it is needed, and the metadata source. Use configured scopes and policies before considering content access. Redact account identities and sensitive resource names where appropriate.

| Area | Questions grounded in visible evidence |
| --- | --- |
| Connected accounts | Which configured connectors and sessions are in scope? What read, write, publish, send, or spending capabilities are granted? |
| Sensitive information | Which shared locations expose confidential or customer information beyond its intended audience? Prefer access metadata to opening records. |
| Agent boundaries | Which agents can reach resources or actions outside their stated responsibilities? Are isolation claims backed by enforced controls? |
| Approval rules | Which actions require approval, and is it enforced for email, purchases, publishing, deletion, and permission changes? |
| Attribution | Can an important action be tied to an agent, initiating user, target, time, and outcome without logging secrets? |
| Unused access | What evidence shows that an old connector, grant, session, or credential is no longer needed? Age alone does not establish that it is unused. |

## Verify without exercising the privilege

Read available policy and grant metadata. Do not send a test message, open unrelated customer records, make a purchase, exercise a destructive permission, or provoke an approval event to prove that a capability exists. A broad configured grant can be a verified access-control finding without executing the privileged action; distinguish granted capability from demonstrated exploitation.

A prompt that says "ask first" is not evidence of an enforced approval boundary. Equally, absence of an approval rule in one inspected prompt does not prove that the host has no approval gate.

If session lists, connector scopes, IAM settings, or audit logs are unavailable, state what cannot be verified. Never invent connected accounts or imply that browser access was inspected when it was not.

## Complete this mode

Return the redacted access map, verified mismatches between required and granted access, uncertain concerns, actual approval and attribution evidence, and the highest-priority remediation suggestions. Recommend up to five next actions only when supported by the review.

Changing permissions, disconnecting accounts, deleting shared files, terminating sessions, revoking credentials, and sending messages require their own authorization. A remediation suggestion is not an executed change.
