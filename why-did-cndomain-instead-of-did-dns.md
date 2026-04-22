# Why did:cndomain instead of did:dns
## Working Draft Note

**Authors**  
did:cndomain Draft Editors

**Status**  
Working Draft Note

**Latest Version**  
Repository working draft

**This Version**  
Repository snapshot dated 2026-04-20

**Date**  
2026-04-20

---

## Abstract

This note explains why the proposed `did:cndomain` method is introduced as a distinct DID method rather than being treated solely as a deployment profile of a more general DNS-oriented DID method such as `did:dns`.

`did:cndomain` is scoped to the `.cn` domain namespace and ties DID naming, authority, lifecycle interpretation, and governance-aware operation to that namespace. That combination is the basis for treating it as a distinct DID method.

This note is explanatory and non-normative. It accompanies the `did:cndomain Method Specification`, which defines the normative behavior of the method. In the current balanced draft set, baseline resolution/discovery and control-proof requirements are integrated into the method specification.
For DID method registration, this note is optional supporting rationale and is not a mandatory prerequisite artifact if the method specification and registration entry are complete. This follows the current `w3c/did-extensions` registration process, where adding a DID method is performed by submitting a `methods/*.json` entry that links to the method specification ([w3c/did-extensions README](https://github.com/w3c/did-extensions)).

---

## Status of This Document

This document is a working draft note and may be updated, replaced, or obsoleted at any time.

This note is explanatory and non-normative. It does not define the normative behavior of the `did:cndomain` DID method. The normative requirements are defined by the `did:cndomain Method Specification`.
This note can support reviewer understanding, but it is not itself a required acceptance gate for registry entry.

---

## Table of Contents

1. Introduction  
2. Purpose of This Note  
3. Baseline Position  
3.1 Comparison Baseline and Scope  
4. General DNS Infrastructure vs. `.cn` Domain Namespace  
5. Why a Mere Deployment Profile Is Not Enough  
6. Distinct Method Syntax and Subject Model  
7. Distinct Authority and Update Model  
8. Distinct Lifecycle Semantics  
9. Distinct Architectural Positioning  
10. Governance as a Material Method-Level Difference  
11. What `did:cndomain` Is Not Claiming  
12. Practical Standardization Position  
13. Conclusion  
14. Informative References  

---

# 1. Introduction

The proposed `did:cndomain` method is a domain-governed namespace DID method anchored to the `.cn` domain namespace.

Because the method uses DNS-assisted discovery and HTTPS publication, a reasonable question is whether it should be treated only as a deployment profile of a more general DNS-oriented DID method. This note explains why the answer is no.

The short answer is that `did:cndomain` is not defined merely as a way to retrieve DID Documents by using DNS. It is defined as a method in which the `.cn` domain namespace acts as the naming anchor, authority anchor, and lifecycle-relevant governance context.

# 2. Purpose of This Note

The purpose of this note is to explain the rationale for introducing `did:cndomain` as a distinct DID method.

The claim is limited:

- `did:cndomain` is intended for a specific governed namespace
- the method defines a specific authority model
- the method defines lifecycle consequences tied to domain events
- those design choices justify a distinct DID method rather than a purely operational profile

# 3. Baseline Position

This note does not argue that DNS-based DID methods are invalid in general. DNS can play a valid role in DID method design, especially for discovery and publication.

This note also does not argue that every domain namespace requires its own DID method.

The narrower position is:

1. DNS may be used as a DID method substrate
2. HTTPS may be used as a DID Document publication channel
3. those technical choices alone do not determine whether a design is a distinct DID method
4. the material question is whether the design defines a distinct method-level meaning, authority model, lifecycle model, and governance model

`did:cndomain` is proposed as distinct because the answer to that question is yes.

## 3.1 Comparison Baseline and Scope

This note uses the public `did:dns` method specification as a comparison baseline for a DNS-oriented DID method design. The comparison is limited to method-level semantics that matter for registration review, including subject model, authority model, lifecycle interpretation, and governance relevance.

In that baseline, `did:dns` is specified as DNS-zone-based, ties update authority to control of underlying DNS infrastructure, and discusses domain deletion and later re-registration as an identifier continuity risk. Concretely:

- target system: DNS zone files controlled by authoritative DNS servers of the DID domain ([did:dns "Target system"](https://danubetech.github.io/did-method-dns/#target-system))
- operation authorization: authority to update DNS is treated as authority to update the DID ([did:dns "Authorization of DID Operations"](https://danubetech.github.io/did-method-dns/#authorization-of-did-operations))
- continuity risk: deleted domains can later be re-registered by another party ([did:dns "DID Document Deactivation"](https://danubetech.github.io/did-method-dns/#did-document-deactivation))

This note does not evaluate implementation quality or adoption of `did:dns`; it only explains why `did:cndomain` is proposed as a separate method instead of a deployment profile.

# 4. General DNS Infrastructure vs. `.cn` Domain Namespace

The central distinction is between:

- a DID method centered on DNS as general technical infrastructure
- a DID method centered on the `.cn` domain namespace as a governed naming and authority space

`did:cndomain` is centered on the `.cn` domain namespace itself.

Under `did:cndomain`:

- the method-specific identifier is rooted in a `.cn` domain or subdomain
- the corresponding domain anchor is the primary authority root
- object-level identifiers are subordinate to the parent domain
- lifecycle interpretation is sensitive to domain events
- control-sensitive authority depends on current legitimate domain control

Accordingly, the relevant question is not whether DNS is used. The relevant question is whether the method defines namespace-specific authority and lifecycle semantics. `did:cndomain` does.

# 5. Why a Mere Deployment Profile Is Not Enough

A deployment profile usually refines operational details such as:

- which DNS record types to use
- how to construct fallback URLs
- where to publish DID Documents
- how to optimize retrieval behavior
- which representation format to prefer

`did:cndomain` goes beyond operational refinement.

It defines a method in which:

1. the `.cn` domain namespace is the root naming anchor
2. current legitimate domain control is part of DID authority
3. domain lifecycle events may trigger restriction or deactivation
4. subordinate object identifiers inherit authority from the parent domain
5. delegated authority remains subordinate to domain-rooted control

Those are method-level rules, not only publication or retrieval preferences.

In the current draft set, these baseline method-level behaviors are defined in the method specification itself. Companion profiles may refine operations but are not intended to replace or weaken baseline method semantics.

# 6. Distinct Method Syntax and Subject Model

`did:cndomain` defines a method-specific subject model that supports both domain-level and object-level identifiers.

Examples:

```text
did:cndomain:example.cn
did:cndomain:example.cn:service:web
did:cndomain:example.cn:resource:gpu0
```

The method semantics include:

1. a domain-level DID represents the `.cn` domain or subdomain as the root subject
2. an object-level DID represents a subordinate subject beneath that root
3. the object-level DID remains subordinate to the parent domain authority unless valid delegation is explicitly defined

This is more than a serialization or lookup convention. It is part of the method-specific meaning of the identifier.

# 7. Distinct Authority and Update Model

`did:cndomain` defines an authority model in which control-sensitive operations are intentionally dual-bound:

1. DID-side authorization appropriate to the requested operation
2. current legitimate control of the corresponding `.cn` domain

This design matters in situations such as:

- domain transfer
- domain expiry
- dispute-related restriction
- reassignment of control
- loss of access to the domain
- stale key material remaining available after authority changes

Under this model, historical key possession or other stale DID-side authorization artifacts are not sufficient once current legitimate domain control can no longer be demonstrated.

# 8. Distinct Lifecycle Semantics

`did:cndomain` defines lifecycle semantics that are explicitly sensitive to domain events.

The method uses lifecycle states such as:

- `active`
- `suspended`
- `frozen`
- `deactivated`

These states are relevant because a domain-anchored DID may need to respond to events such as:

- expiration
- transfer
- cancellation
- dispute-related freeze
- judicial restriction
- governance intervention
- loss of current legitimate control

In `did:cndomain`, such events are method-relevant lifecycle inputs rather than external background facts.

# 9. Distinct Architectural Positioning

`did:cndomain` adopts a two-plane architecture:

- DNS as the discovery plane
- DID Documents over HTTPS as the trust and control plane

Under this design:

- DNS indicates where DID material may be found
- HTTPS carries the DID Document representation
- the DID Document expresses verification material, control, delegation, service bindings, and related metadata
- lifecycle and governance interpretation remain part of the DID method rather than raw DNS resolution alone

At the baseline method level, discovery conflicts are treated as meaningful signals rather than silently ignored; for example, inconsistent DNS discovery records are handled as a resolution error instead of automatically falling back as if no conflict existed.

This architecture assigns DNS a specific and limited role within a broader method-specific trust model.

# 10. Governance as a Material Method-Level Difference

`did:cndomain` is governance-aware at the method level.

Questions that matter to the method include:

- what counts as acceptable proof of current domain control
- who may create identifiers
- how object-level identifiers are managed
- how disputes are handled
- how restrictions are signaled
- whether continuity is allowed after domain transfer
- how transfer-triggered continuity review is conducted and when control-sensitive updates remain blocked pending that review
- how freezing or deactivation is triggered
- how exceptional and administrative actions are constrained and audited

This does not require the base specification to define every detailed governance process. It does mean that governance is part of the method design rather than an accidental deployment detail.

In practical terms, continuity after a material change in domain control is not assumed implicitly; it is treated as an explicit governance decision, with restricted operation during review unless a narrowly scoped exception is explicitly authorized.

# 11. What `did:cndomain` Is Not Claiming

This note does not claim that:

- general DNS-based DID methods are invalid
- every domain-related DID deployment requires a separate DID method
- `did:cndomain` is a universal DID method for all entities in China
- `.cn` anchoring automatically creates trustworthy identity without proof, governance, and lifecycle controls
- `did:cndomain` replaces legal, regulatory, registrar, or registry processes

The claim is narrower:

> Where DID naming, DID authority, DID lifecycle, and DID trust interpretation are bound directly to the `.cn` domain namespace and its governed control semantics, a distinct DID method is justified.

# 12. Practical Standardization Position

From a practical standardization standpoint:

1. a general DNS-oriented DID method may continue to exist as a broader method category
2. `did:cndomain` may be proposed as a specialized DID method for the `.cn` domain namespace
3. the justification lies in its distinct namespace-specific semantics, authority model, lifecycle model, and governance-aware interpretation
4. this note may be submitted as optional supporting rationale and is not required as a separate publication-blocking prerequisite under the current DID method registration workflow

The justification is therefore not merely a preference about DNS record choices or retrieval mechanics.

# 13. Conclusion

`did:cndomain` should be treated as a distinct DID method rather than merely as a deployment profile of a general DNS-oriented DID method because its defining feature is not DNS usage alone.

Its defining features are:

- a method-specific identifier model rooted in the `.cn` domain namespace
- a domain-rooted authority model
- a dual control requirement for control-sensitive operations
- lifecycle semantics that respond to domain events
- a two-plane architecture that separates discovery from trust representation
- governance-aware interpretation of continuity, dispute, restriction, and exceptional actions

These are method-level design choices. Accordingly, `did:cndomain` is best understood as a domain-governed namespace DID method, not merely as a DNS deployment profile.

## Informative References

- did:dns Method Specification: https://danubetech.github.io/did-method-dns/
- Decentralized Identifier Ecosystem Extensions (`w3c/did-extensions`): https://github.com/w3c/did-extensions
- DID Core v1.0: https://www.w3.org/TR/did-core/
