# Change Review

Use this mode for a pull request, branch, commit, or uncommitted change.

## Establish a stable comparison

Record the requested baseline and target exactly: PR number and head/base identifiers, named branch comparison and merge base when relevant, commit range, or staged/unstaged/untracked scope. Use read-only inspection; fetching remote refs, checking out branches, or altering the index is not required by this mode and must respect the user's existing permissions.

When the target is ambiguous, clarify the comparison instead of silently reviewing a different range. If files or the PR head move during inspection, disclose which snapshot supports each finding.

## Trace the changed behavior

Read the diff and the surrounding implementation. Follow changed inputs into their consumers, including the middleware, route registration, policy, helper, configuration, or dependency that decides whether the path is reachable.

Look for newly introduced or worsened injection, broken authentication or object authorization, sensitive-data exposure, weakened cryptography, unsafe file handling, insecure deployment defaults, dependency risk, or a removed control.

A line that removes an ownership predicate is a review target even if the surrounding route is unchanged. Conversely, a handler newly added under a protected router is not automatically unauthenticated.

## Keep findings tied to the change

- Explain how the selected change introduces or worsens each verified issue.
- Cite a changed line where possible and relevant supporting locations separately. Use actual source or diff line numbers and label which snapshot each belongs to.
- Do not expand into a whole-repository audit. If an urgent pre-existing issue is encountered, label it separately as outside the selected change.
- For dependency updates, use resolved versions and a current primary advisory. Lockfile churn or version age alone is not a verified vulnerability.
- Give a minimal defensive fix consistent with the surrounding code. State whether it is only suggested or actually verified.

## Complete this mode

List the exact comparison, reviewed files and connected controls, findings introduced or worsened by the change, any out-of-scope observation, and coverage gaps. No verified findings in this comparison does not establish that the application as a whole is secure.
