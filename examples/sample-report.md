# Fictional Security Review Report

This example illustrates the report contract using invented users and documents. It is not a review of this repository or a real application. File names and line numbers below label the fictional source shown here; no live system was inspected and no code was executed.

## Fictional source context

The caller's identity is verified upstream. Every document is private to its owner. The complete fictional store is an in-memory map without row-level policies or hidden ownership filtering.

The selected change removes the ownership condition from line 31:

~~~diff
-    if document is None or document.owner_id != request.user_id:
+    if document is None:
~~~

Relevant target excerpts from fictional `documents.py`:

~~~text
17 DOCUMENTS = {
18     101: Document(owner_id="alice", body="Alice private draft"),
19     202: Document(owner_id="bob", body="Bob private draft"),
20 }
21
22
23 def require_login(request: Request):
24     # Identity has already been verified upstream.
25     if request.user_id is None:
26         raise PermissionError("Authentication required")
27
28
29 def get_document(request: Request):
30     document = DOCUMENTS.get(request.document_id)
31     if document is None:
32         return None
33     return document.body
34
35
36 ROUTES = {"documents.get": (require_login, get_document)}
37
38
39 def dispatch(request: Request):
40     # The guard runs before the handler.
41     guard, handler = ROUTES[request.route]
42     guard(request)
43     return handler(request)
~~~

---

# Security Review

## Summary

- **Mode:** Change.
- **Scope and snapshot:** The fictional ownership-condition removal shown above.
- **Evidence basis:** Static inspection of the supplied source and scenario assumptions.
- **Verified findings:** 1 (Critical: 0, High: 1, Medium: 0, Low: 0).
- **Overall observation:** The selected change removes object-level authorization while preserving the route's authentication check.

## Verified findings

### F-01: Document ownership is no longer checked before returning content

- **Severity:** High within this fictional private-document scenario.
- **Confidence and reason:** High. The removed predicate, direct store lookup, dispatcher, and private-owner policy are all supplied.
- **Location:** Target `documents.py:31`, supported by lines 17-20, 29-33, and 36-43.
- **Evidence:** The lookup uses only the caller-selected document ID. The previous condition rejected documents with a different owner; the new condition rejects only a missing document.
- **Path:** Authenticated caller -> selected document ID -> dispatcher authentication guard -> map lookup -> private document body returned.
- **Impact and prerequisites:** An authenticated caller who knows or guesses another user's document ID can receive its private content in the supplied scenario.
- **Opposing explanation checked:** The handler is protected by composed authentication at lines 36 and 41-42. That guard rejects a missing identity, but it does not restrict document ownership. The supplied store has no downstream ownership control. Missing authentication is therefore not a second finding.
- **Recommended fix:** Restore the ownership condition, consistent with the baseline:

~~~python
if document is None or document.owner_id != request.user_id:
    return None
~~~

- **Verification performed:** Static comparison and control-flow inspection only.
- **Verification proposed:** In a separately authorized isolated check, confirm through the dispatcher that the owner receives the document, another authenticated user receives no content, an absent ID returns no content, and an unauthenticated request is rejected. These runtime checks were not performed.

## Needs review

None supported by an additional concrete concern in the supplied comparison.

## Informational observations

None separate from the verified finding.

## Coverage and limits

- **Inspected:** The fictional baseline predicate, target store, document handler, authentication guard, route registration, dispatcher, and scenario policy.
- **Checks performed:** Predicate comparison, caller-to-document tracing, inspection of composed authentication, and consideration of downstream ownership controls.
- **Not observed:** Unauthenticated access through the supplied dispatcher. This statement is limited to the shown static control flow.
- **Coverage gaps:** No real deployment, upstream identity-verification implementation, HTTP integration, concurrency behavior, or runtime checks were available. The finding does not establish exposure of any real service.
- **Changes or live actions performed:** None. The patch above is a suggestion.

## Prioritized next actions

1. Restore the ownership condition in the selected change.
2. After execution is authorized, perform the isolated owner, non-owner, missing-document, and unauthenticated checks.
