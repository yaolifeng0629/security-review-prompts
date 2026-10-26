# Security Policy

This repository distributes prompts and an agent skill. Sensitive concerns can include instructions that induce unauthorized actions, leak private data, confuse untrusted material with user authorization, or falsely present uncertain evidence as a verified vulnerability.

## Report safely

For ordinary prompt, translation, and documentation issues, open a public GitHub issue with a minimal fictional or thoroughly redacted example, the affected file, the expected behavior, and the observed behavior. Never include credentials, access-bearing URLs, customer data, private code, or actionable details of a real application's vulnerability.

For a concern that cannot be described safely in public, use GitHub's private vulnerability reporting option **if it is available for this repository**. This policy does not claim that the feature is enabled. If no private option is available, open a minimal public issue asking the maintainer to establish a private channel; withhold sensitive details until that channel exists.

A vulnerability found in an application using these prompts belongs with the affected application's owner and its designated private reporting channel. Do not publish it here simply because these prompts helped identify it.

## Useful report details

- The affected prompt or skill file and the version, commit, or date inspected.
- A fictional or redacted input that illustrates the concern.
- The assistant/host and relevant permission settings, without tokens or private identifiers.
- The actual behavior, expected boundary, and potential impact.
- Any uncertainty or missing context.

Submitting a report does not authorize live probing, destructive reproduction, credential access, or testing against systems you do not own. Keep reproduction within your explicitly authorized scope.

## Response and coverage

There is no published response-time guarantee, bug bounty, or independently certified security status for this repository. Prompt behavior also depends on the model, host permission system, available tools, and supplied context; instructions alone are not an enforcement boundary.

Fixes are evaluated against the affected instructions and related examples. Any review outcome should state its inspected scope and limits rather than promise that every model or host will behave identically.
