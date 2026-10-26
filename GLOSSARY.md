# Security Review Language

This project uses a shared vocabulary for evidence-backed reviews of applications, changes, and agent access.

## Language

**Review scope**:
The explicitly selected application, change, or shared agent environment being examined.
_Avoid_: Everything accessible

**Verified finding**:
A security weakness supported by inspected evidence and a concrete path from an actor to an affected resource.
_Avoid_: Suspicion, checklist hit

**Needs review**:
A plausible security concern whose decisive evidence is missing or inaccessible.
_Avoid_: Confirmed vulnerability, verified finding

**Not observed**:
A weakness that was not found in the inspected portion of the selected scope.
_Avoid_: Secure, clean, not present everywhere

**Coverage gap**:
A relevant part of the review scope that could not be inspected or validated.
_Avoid_: Passed check

**Severity**:
The consequence and practical reachability of a verified weakness in the reviewed context.
_Avoid_: Confidence

**Confidence**:
The strength of the evidence supporting a finding, independently of its severity.
_Avoid_: Severity

**Remediation suggestion**:
A proposed change offered for review, without implying authorization to apply it.
_Avoid_: Applied fix
