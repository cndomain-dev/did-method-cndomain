# did:cndomain Method Specification

**Editors**
did:cndomain Draft Editors

**Status**
Working Draft

**Latest Version**
Repository working draft

**This Version**
Repository snapshot dated 2026-04-13

**Date**
2026-04-13

**Publication Note**
A stable public URL is required before submission to the DID Method Registry.


# 1. Introduction

This specification defines the `did:cndomain` DID method.

The `did:cndomain` method defines decentralized identifiers anchored to the `.cn` domain namespace. It is intended for scenarios in which domain control, domain-bound service discovery, and domain-governed trust relationships are expressed through DID Documents.

The method is designed for domain-related subjects and resources. It is not intended to serve as a general-purpose national identity method or as a universal DID method for subjects unrelated to the `.cn` domain namespace.

Under this method:

1. the `.cn` domain namespace serves as the primary naming anchor
2. DNS MAY be used as a discovery mechanism for DID Document publication endpoints
3. HTTPS is used as the publication and retrieval channel for DID Documents
4. current legitimate control of the corresponding `.cn` domain remains central to authority, lifecycle, and trust interpretation.

The purpose of this specification is to define:

- the syntax of `did:cndomain` identifiers
- the semantics of domain-level and object-level identifiers
- lifecycle operations and control requirements
- DID resolution behavior
- DID Document representation requirements
- domain control binding rules
- lifecycle status interpretation
- governance, security, privacy, implementation, and conformance requirements.

This specification is designed to be self-contained for DID method registration and baseline interoperability.

Companion `did:cndomain` profiles MAY define stricter or deployment-specific operational behavior (for example resolution/discovery refinements, control proof policies, or governance overlays), but conforming baseline method behavior is defined in this specification.

## 1.1 Relationship to DID Core

The `did:cndomain` method is a DID method specification within the meaning of DID Core.

Accordingly, this specification defines method-specific rules for:

- method syntax
- method-specific identifier semantics
- creation, resolution, update, suspension, freezing, deactivation, and possible reactivation
- DID Document interpretation
- authorization and control-sensitive operations
- method-specific security and privacy considerations.

Where DID Core defines general DID architecture, terminology, or representation requirements, this specification applies those requirements to the `did:cndomain` method.

## 1.2 Method Design Overview

The `did:cndomain` method adopts a domain-anchored design.

A `did:cndomain` identifier is rooted in a `.cn` domain or valid subdomain and MAY identify a subordinate service or resource object beneath that domain.

The method intentionally separates:

- discovery, which may rely on DNS
- trust and control expression, which is represented through the DID Document and associated lifecycle semantics.

This design allows a `did:cndomain` identifier to preserve familiar domain-based discoverability while expressing method-specific authority, verification material, delegation, service binding, and lifecycle state through DID mechanisms.

## 1.3 Method Positioning

The `did:cndomain` method is best understood as a domain-governed namespace DID method.

Its defining feature is not merely that DNS may be used in discovery. Its defining feature is that the `.cn` domain namespace acts as the method’s primary naming and authority anchor, and that current legitimate domain control remains relevant to control-sensitive DID operations.

Accordingly, this method is narrower than a general-purpose DID method and more specific than a generic DNS-based identifier deployment pattern.

## 1.4 Document Structure

Sections 5–6 define syntax and identifier semantics.

Sections 7–11 define lifecycle operations, resolution, representation, domain control binding, and lifecycle status.

Sections 12–14 define governance, security, and privacy considerations.

Sections 15–17 define implementation considerations, examples, and conformance.

Detailed operational behavior MAY be further refined by companion `did:cndomain` profiles, but conformance to this method specification MUST NOT depend on the existence of a separate profile document.

# 2. Scope and Non-Goals

This section defines the intended scope of the `did:cndomain` method and clarifies what the method does not attempt to provide.

## 2.1 Scope

The `did:cndomain` method applies to decentralized identifiers anchored to the `.cn` domain namespace.

A `did:cndomain` identifier MAY be used to represent:

1. a `.cn` domain
2. a valid subdomain under `.cn`
3. a domain-bound service identified beneath such a domain
4. a domain-bound resource object identified beneath such a domain.

The method is intended for scenarios in which the following are relevant:

- domain control as part of DID authority
- domain-bound service discovery
- domain-governed trust relationships
- subordinate identifiers for services or resources beneath a parent domain
- lifecycle behavior influenced by domain events and governance conditions.

## 2.2 Intended Subjects

Subjects represented by `did:cndomain` identifiers MAY include, depending on the applicable deployment or governance profile:

- domain-level subjects
- subdomain-level subjects
- web services
- API services
- resource access endpoints
- application components
- domain-bound infrastructure objects
- other objects whose authority and interpretation remain anchored to the corresponding `.cn` domain.

This specification does not require all deployments to support all such subject categories.

## 2.3 Non-Goals

The `did:cndomain` method is not intended to:

1. serve as a universal DID method for all subjects in China
2. represent a sovereign national identity framework
3. replace general-purpose DID methods for subjects not meaningfully anchored to a `.cn` domain
4. define legal ownership of domains
5. replace domain registration, registry, registrar, judicial, or regulatory processes
6. guarantee trust solely by virtue of DNS publication or HTTPS reachability.

This specification defines method-level technical and governance-aware interpretation rules. It does not, by itself, determine legal rights, institutional authority, or regulatory jurisdiction.

## 2.4 Scope Boundaries

A conforming implementation of `did:cndomain` MUST treat the `.cn` domain anchor as central to the method.

An implementation MUST NOT interpret this method as a general-purpose namespace for arbitrary non-domain subjects.

If a subject cannot be meaningfully anchored to a `.cn` domain or subordinate object under that domain, it is outside the intended scope of this method.

# 3. Design Goals

This section defines the principal design goals of the `did:cndomain` method.

The `did:cndomain` method is designed to provide a domain-anchored DID method with clear semantics for authority, lifecycle, and discovery in the context of the `.cn` domain namespace.

## 3.1 Domain-Anchored Naming

The method is designed to ensure that a `did:cndomain` identifier is rooted in a `.cn` domain or valid subordinate context beneath such a domain.

The domain anchor is intended to provide:

- stable namespace structure
- recognizable naming root
- linkage to domain control
- basis for subordinate identifier inheritance.

## 3.2 Domain-Governed Authority

The method is designed so that authority over a DID is not determined solely by historical key possession or syntactic publication.

Instead, authority is intended to remain tied to current legitimate control of the corresponding `.cn` domain, especially for control-sensitive operations such as update, delegation, deactivation, and reactivation.

## 3.3 Separation of Discovery and Trust Representation

The method is designed to separate:

1. discovery of a DID Document publication endpoint
2. expression of trust, control, and verification material.

Under this design:

- DNS may assist in endpoint discovery
- the DID Document, associated metadata, and lifecycle rules carry the authoritative trust semantics.

## 3.4 Support for Domain-Level and Object-Level Subjects

The method is designed to support both:

- domain-level identifiers
- object-level identifiers defined beneath a parent domain.

This allows a single domain anchor to support multiple subordinate subjects, such as services or resources, while preserving a coherent parent-child authority model.

## 3.5 Lifecycle Sensitivity to Domain Events

The method is designed so that DID lifecycle interpretation may respond to domain-related events, including:

- expiration
- transfer
- cancellation
- dispute-related restriction
- governance-imposed freeze
- loss of current legitimate control.

This allows DID status to remain consistent with materially changing domain authority conditions.

## 3.6 Governance Compatibility

The method is designed to permit governance-aware deployment.

This means that deployments may define profiles addressing:

- proof requirements
- restricted states
- dispute handling
- exceptional actions
- continuity after transfer
- auditability and transparency.

At the same time, the core method semantics remain defined by this specification.

## 3.7 Interoperable DID Usage

The method is designed to remain compatible with DID ecosystem concepts, including:

- DID syntax and DID Documents
- verification relationships
- resolvers and resolution metadata
- lifecycle interpretation
- profile-based deployment refinement.

This specification does not seek to replace DID architecture, but to define a method-specific application of it to the `.cn` domain namespace.

# 4. Terminology

This section defines method-specific terminology used in this specification.

Unless otherwise specified, terms inherited from DID Core retain their DID Core meaning. The terms below define additional meanings specific to `did:cndomain`.

## 4.1 `.cn` Domain Anchor

The `.cn` domain anchor is the normalized `.cn` domain or valid subdomain extracted from the method-specific identifier.

The `.cn` domain anchor serves as:

- the primary naming root
- the primary authority root
- the basis for lifecycle-sensitive interpretation under this method.

## 4.2 Domain Portion

The domain portion is the portion of the method-specific identifier that encodes the `.cn` domain or subdomain before any optional object-level segments.

The domain portion is primarily relevant in syntax, parsing, normalization, and resolution contexts.

## 4.3 Domain-Level DID

A domain-level DID is a `did:cndomain` identifier whose subject is the `.cn` domain itself or a valid subdomain under `.cn`.

Examples:

```text
did:cndomain:example.cn
did:cndomain:sub.example.cn
```

## 4.4 Object-Level DID

An object-level DID is a `did:cndomain` identifier derived from a parent `.cn` domain and extended with an object classification and object-local name.

Examples:

```text
did:cndomain:example.cn:service:web
did:cndomain:example.cn:resource:gpu0
```

## 4.5 Object Classification

An object classification is the method-defined segment that indicates the semantic category of an object-level identifier.

Examples include:

- `service`
- `resource`

An object classification is interpreted according to this specification.

If a deployment adopts a companion profile, object classification handling MAY be further constrained within that profile’s declared scope.

## 4.6 Object-Local Name

An object-local name is the identifier segment that distinguishes a specific subordinate object within the relevant object classification beneath a parent domain.

Its interpretation remains subordinate to the authority and naming rules of the parent domain.

## 4.7 Current Legitimate Control

Current legitimate control refers to the currently valid control relationship recognized by the applicable technical and governance procedures of this method for the corresponding `.cn` domain.

This specification does not define legal ownership in the abstract. Rather, it defines how current legitimate control is interpreted for the purposes of DID authority, lifecycle operations, and trust-sensitive behavior.

## 4.8 Domain Control Proof

A domain control proof is a technical proof that demonstrates current legitimate control over the corresponding `.cn` domain in a manner accepted by this specification.

Companion control-proof profiles, if adopted, MAY define stricter admissibility and verification procedures.

Examples MAY include:

- DNS challenge proof
- HTTPS challenge proof
- registrar-backed proof
- governance-authorized proof.

## 4.9 DID Update Key

A DID update key is verification material authorized to approve control-sensitive modifications to a `did:cndomain` DID Document or lifecycle state, subject to the method’s domain control requirements.

Possession of a DID update key alone does not automatically establish sufficient authority under this method.

## 4.10 Resolver

A resolver is a component that accepts a `did:cndomain` identifier, applies this specification and any applicable profile, retrieves or constructs the corresponding DID resolution result, and returns:

- a DID Document, where applicable
- DID document metadata
- DID resolution metadata.

## 4.11 Discovery Record

A discovery record is a DNS record used to indicate or assist in locating the HTTPS publication endpoint for a `did:cndomain` DID Document.

A discovery record is part of the discovery plane. It is not, by itself, a complete DID Document representation.

## 4.12 Lifecycle State

A lifecycle state is the currently applicable method-level status of a `did:cndomain` identifier.

Primary lifecycle states under this specification include:

- `active`
- `suspended`
- `frozen`
- `deactivated`

Lifecycle state affects how a DID may be interpreted, resolved, updated, or relied upon.

## 4.13 Status Value

A status value is a metadata value used to indicate the lifecycle state of a DID in a DID document metadata or DID resolution metadata context.

A status value is one possible means of expressing lifecycle state, but it is not itself the entirety of lifecycle interpretation.

## 4.14 Governance Profile

A governance profile is a deployment- or ecosystem-specific set of rules that refines how this method handles proof requirements, eligibility, restricted states, disputes, continuity, and auditability.

A governance profile MAY refine operational details, but MUST NOT contradict the method-level semantics defined by this specification.

## 4.15 Control-Sensitive Operation

A control-sensitive operation is an operation whose validity depends on authoritative control over the DID and corresponding domain anchor.

Examples include:

- create
- update
- key rotation
- controller change
- delegation
- suspension
- freezing
- deactivation
- reactivation.

## 4.16 Parent Domain Authority

Parent domain authority refers to the default authority relationship by which an object-level DID remains subordinate to the authority of its parent domain-level DID, unless a valid delegation model explicitly defines otherwise.

This concept is central to the inheritance semantics of `did:cndomain`.

# 5. Method Syntax

This section defines the syntax of `did:cndomain` identifiers.

A `did:cndomain` identifier MUST conform to the general DID syntax defined by DID Core, and its method-specific identifier MUST be interpreted according to this specification. Under this method, the method-specific identifier is rooted in the `.cn` domain namespace and MAY include a subordinate object structure.

A `did:cndomain` identifier is formally defined by the following ABNF, using the ABNF notation of RFC 5234:

```abnf
did-cndomain = "did:cndomain:" domain [ ":" object-type ":" object-name ]
domain        = domain-label *( "." domain-label ) ".cn"
domain-label  = alnum / ( alnum *( alnum / "-" ) alnum )
object-type   = segment
object-name   = segment
segment       = 1*( %x61-7A / DIGIT / "-" )
alnum         = %x61-7A / DIGIT
```

This ABNF defines the minimum method-specific syntax. It is complemented by the normalization and validity rules in Sections 5.3 through 5.7, including lowercase normalization, A-label handling for internationalized domain names, and rejection of empty or non-normalized segments.

A `did:cndomain` identifier therefore takes one of the following forms:

```text
did:cndomain:<domain>
did:cndomain:<domain>:<object-type>:<object-name>
```

Where:

- `<domain>` identifies a normalized `.cn` domain or subdomain
- `<object-type>` identifies a method-defined object classification
- `<object-name>` identifies a domain-local object name under the authority of the corresponding domain.

## 5.1 General Form

A conforming `did:cndomain` identifier MUST begin with:

```text
did:cndomain:
```

The portion following `did:cndomain:` is the method-specific identifier.

The method-specific identifier MUST contain a valid normalized `.cn` domain or subdomain.

The method-specific identifier MAY additionally contain an object-type and object-name pair.

A domain-level identifier has the following form:

```text
did:cndomain:<domain>
```

An object-level identifier has the following form:

```text
did:cndomain:<domain>:<object-type>:<object-name>
```

No additional method-specific segments are defined by this version of the specification.

An implementation MUST reject an identifier that does not conform to one of the above forms.

## 5.2 Examples

The following are examples of valid `did:cndomain` identifiers:

```text
did:cndomain:example.cn
did:cndomain:sub.example.cn
did:cndomain:example.cn:service:web
did:cndomain:example.cn:service:resolver
did:cndomain:example.cn:resource:gpu0
```

The following are examples of identifiers that are not valid under this specification:

```text
did:cndomain:example.com
did:cndomain:Example.cn
did:cndomain:example.cn:service
did:cndomain:example.cn:service:web:extra
did:cndomain:
```

These examples are invalid because, respectively:

1. the domain is not under `.cn`
2. the domain is not normalized to lowercase form
3. the object-level structure is incomplete
4. the identifier contains unsupported additional segments
5. the method-specific identifier is empty.

## 5.3 Domain Portion

The domain portion of a `did:cndomain` identifier:

1. MUST refer to a name under the `.cn` namespace
2. MUST be represented in lowercase ASCII form
3. MUST be normalized before DID construction and resolution
4. MUST use the A-label form for internationalized domain names where applicable.

For avoidance of encoding ambiguity, a conforming `did:cndomain` string representation MUST use the normalized A-label form whenever internationalized labels are involved. An implementation MAY accept Unicode domain input as a local convenience, but it MUST convert that input to normalized A-label form before DID construction, comparison, publication, or resolution.

A domain portion MAY represent either:

- a second-level `.cn` domain
- a valid subdomain under a `.cn` domain.

Examples:

```text
example.cn
sub.example.cn
xn--example-9d0b.cn
```

An implementation MUST reject a method-specific identifier whose domain portion is not a `.cn` domain or subdomain.

## 5.4 Object Portion

The object portion is optional.

When present, it MUST consist of exactly two additional segments:

1. `<object-type>`
2. `<object-name>`.

Accordingly, a conforming object-level identifier MUST have the following structure:

```text
did:cndomain:<domain>:<object-type>:<object-name>
```

The object portion MUST NOT appear unless both `<object-type>` and `<object-name>` are present.

The object portion is interpreted under the authority of the corresponding domain anchor.

This specification initially recognizes the following object-type values:

- `service`
- `resource`

An implementation MAY define additional object-type values only if such values are permitted by an applicable profile or governance rule.

For interoperability, producers SHOULD prefer registered or profile-defined object-type values and SHOULD avoid private ad hoc values in public-facing identifiers.

## 5.5 Character Rules

The domain portion MUST follow applicable domain normalization rules.

For `<object-type>` and `<object-name>`, this specification defines the following minimum syntax constraints:

1. they MUST NOT be empty
2. they MUST be represented in lowercase form
3. they MUST be limited to lowercase letters (`a-z`), digits (`0-9`), and hyphen (`-`)
4. they MUST NOT contain additional colon (`:`) characters
5. they MUST be interpreted as method-specific segments under this specification.

An implementation MAY impose stricter naming requirements for object-level identifiers, including:

- length limits
- reserved names
- profile-specific object classes
- governance-defined prohibited terms.

For interoperability, producers SHOULD avoid unstable, ambiguous, or overly sensitive object-local names.

## 5.6 Normalization Rules

Before constructing, publishing, or resolving a `did:cndomain` identifier, an implementation MUST normalize the identifier according to the following rules:

1. the DID method name MUST be `cndomain`
2. the domain portion MUST be converted to lowercase
3. internationalized domain names, where applicable, MUST be converted to their normalized A-label form
4. object-type and object-name MUST be expressed in lowercase
5. no empty segment may appear in the method-specific identifier.

Two identifiers that differ only by disallowed case variation in the domain portion MUST NOT be treated as distinct identifiers under this specification.

An implementation SHOULD reject non-normalized identifiers rather than silently rewriting them, unless an applicable profile explicitly allows normalization prior to validation.

## 5.7 Syntax Validity Rules

A `did:cndomain` identifier is syntactically valid only when all of the following conditions are satisfied:

1. the DID method name is exactly `cndomain`
2. the method-specific identifier is not empty
3. the domain portion is a valid normalized `.cn` domain or subdomain
4. the identifier contains either:
   - no object portion
   - exactly one object-type and one object-name
5. all segments conform to the syntax constraints defined by this specification.

An implementation MUST reject an identifier that violates any of the above conditions.

## 5.8 Parsing Rules

A conforming parser for `did:cndomain` MUST interpret the method-specific identifier in the following order:

1. identify the domain anchor
2. determine whether additional object-level segments are present
3. if no additional segments are present, treat the identifier as a domain-level DID
4. if exactly two additional segments are present, treat the identifier as an object-level DID
5. otherwise, reject the identifier as invalid.

Colons appearing in the method-specific identifier are interpreted according to this specification only. They do not imply any generic semantics shared across other DID methods.

## 5.9 Relationship to Method Semantics

The syntax of `did:cndomain` is not merely representational. It reflects the method’s semantic model.

In particular:

- the domain portion establishes the root naming anchor
- the object-type establishes the semantic class of a subordinate identifier
- the object-name distinguishes a specific subject within that class.

Accordingly, syntactic parsing and semantic interpretation are closely related under this method. A syntactically valid identifier MUST still be interpreted in accordance with the method-specific authority, inheritance, and lifecycle rules defined elsewhere in this specification.

# 6. Method-Specific Identifier Semantics

This section defines the semantics of `did:cndomain` method-specific identifiers.

A `did:cndomain` identifier is rooted in the `.cn` domain namespace. Its method-specific meaning is determined by:

1. the domain anchor under `.cn`
2. where applicable, the object structure defined beneath that domain anchor.

A `did:cndomain` identifier MAY therefore represent either:

- a domain-level DID
- an object-level DID defined under the authority of a `.cn` domain.

Unless otherwise specified by an applicable governance profile, the parent domain serves as the primary authority anchor for all identifiers defined beneath it.

## 6.1 Domain-Level Identifiers

A domain-level DID is a `did:cndomain` identifier whose subject is the `.cn` domain itself, or a valid subdomain under `.cn`.

A domain-level DID MUST be directly derived from a normalized `.cn` domain name.

Examples:

```text
did:cndomain:example.cn
did:cndomain:sub.example.cn
```

A domain-level DID is unique only when all of the following conditions are satisfied:

1. the underlying domain name is unique within the `.cn` namespace
2. the identifier is constructed in accordance with the normalization and syntax rules defined by this specification.

The authority of a domain-level DID is rooted in the current legitimate control of the corresponding `.cn` domain.

A domain-level DID MAY act as the parent authority anchor for one or more object-level DIDs defined beneath that domain.

## 6.2 Object-Level Identifiers

An object-level DID is a `did:cndomain` identifier derived from:

- a parent `.cn` domain
- an object classification
- an object-local name.

Examples:

```text
did:cndomain:example.cn:service:web
did:cndomain:example.cn:resource:gpu0
```

An object-level DID is unique only when all of the following conditions are satisfied:

1. the parent domain is unique within the `.cn` namespace
2. the object-local name is unique under the naming rules enforced by the authority of that domain for the relevant object classification.

The meaning of an object-level DID is determined by the combination of:

- the parent domain
- the object classification
- the object-local name.

Unless a valid delegation model is explicitly defined, the authority of an object-level DID is subordinate to and derived from the authority of the corresponding parent domain-level DID.

An implementation MAY define additional constraints for object-level identifiers, including naming conventions, reserved object classifications, or governance-specific validation rules.

## 6.3 Authority Model

The `did:cndomain` method adopts a domain-rooted authority model.

Under this model:

1. authority over a domain-level DID is based on current legitimate control of the corresponding `.cn` domain
2. authority over an object-level DID is, by default, derived from the authority of its parent domain.

A subject MUST NOT be treated as authorized to create, update, or control a `did:cndomain` identifier solely on the basis of identifier syntax or historical association.

For object-level DIDs, the parent domain authority MAY delegate limited control to another controller, provided that:

1. such delegation is explicitly expressed in a valid DID Document or an applicable governance mechanism
2. the delegation remains subordinate to valid current legitimate control of the parent `.cn` domain.

Loss of current legitimate control over the parent domain SHOULD invalidate control-sensitive authority over subordinate object-level DIDs unless an applicable governance profile explicitly defines a continuity mechanism.

## 6.4 Namespace and Inheritance Semantics

The `.cn` domain namespace serves as the root naming anchor for all `did:cndomain` identifiers.

For a domain-level DID, the identifier semantics are anchored directly in the corresponding `.cn` domain.

For an object-level DID, the identifier semantics are inherited from the parent domain and refined by the object classification and object-local name.

This means that:

- the domain anchor establishes the root of naming and authority
- the object classification establishes the semantic category of the subordinate identifier
- the object-local name distinguishes a specific subject within that category.

An object-level DID MUST NOT be interpreted independently of its parent domain context.

## 6.5 Continuity and Change Sensitivity

Because `did:cndomain` identifiers are anchored to the `.cn` domain namespace, their semantic continuity is sensitive to material domain lifecycle events.

Such events MAY include:

- expiration
- cancellation
- deletion
- transfer
- dispute-related restriction
- judicial or regulatory control action
- another governance-recognized status change.

A material change affecting the legitimacy of domain control SHOULD trigger re-evaluation of the semantic authority and lifecycle status of the corresponding `did:cndomain` identifier.

For object-level DIDs, a material change affecting the parent domain SHOULD also trigger re-evaluation of subordinate identifiers.

Further operational consequences of such events are defined in Section 10 and Section 11.

Companion governance profiles MAY define stricter or deployment-specific procedures consistent with this specification.

## 6.6 Method-Specific Interpretation Constraints

A `did:cndomain` identifier MUST be interpreted according to this specification.

If a deployment adopts a companion profile, interpretation MUST additionally remain consistent with that profile for the claimed scope.

In particular:

1. the domain portion MUST be treated as the primary naming and authority anchor
2. the object portion, when present, MUST be interpreted as subordinate to the domain portion
3. colons appearing in the method-specific identifier are interpreted according to `did:cndomain` rules and not according to any generic cross-method convention
4. the semantics of object classifications such as `service`, `resource`, or other allowed values are method-specific.

An implementation MUST NOT assume that object-level identifiers from this method are semantically equivalent to identifiers from another DID method merely because they use superficially similar segment structures.

# 7. Method Operations

This section defines the lifecycle operations for `did:cndomain`, including creation, resolution, update, suspension, freezing, deactivation, and possible reactivation.

Unless otherwise specified by an applicable governance profile, control-sensitive operations under this method MUST require both:

1. proof of authorization by a valid DID control, initial creation, update, or recovery mechanism appropriate to the operation
2. proof of current legitimate control over the corresponding `.cn` domain.

For object-level DIDs, authority is derived from the corresponding parent domain unless a valid delegation mechanism is explicitly defined in the applicable DID Document.

Further requirements relating to domain control binding and continuity are defined in Section 10.

Companion control-proof profiles MAY further refine operational procedures.

## 7.1 Create

A requester MAY initiate creation of a `did:cndomain` identifier only when all of the following conditions are satisfied:

1. the subject corresponds to a valid `.cn` domain, or to a valid object defined under a `.cn` domain
2. the requester can demonstrate current legitimate control over the corresponding `.cn` domain
3. the requester can demonstrate possession of the initial control material to be authorized by the DID Document
4. the requester is authorized to publish or cause the publication of a conforming DID Document for that identifier.

For a domain-level DID, the subject MUST correspond to the `.cn` domain itself.

For an object-level DID, the subject MUST correspond to an object that is defined under the authority of the parent `.cn` domain and named in accordance with the naming rules of that domain authority.

Because a newly created DID might not yet have an existing DID Document, initial creation proof is distinct from post-creation update proof. For initial creation, the requester MUST prove control of the verification material that will be authorized by the initial DID Document, and the proof MUST be bound to the requested DID, domain anchor, operation, challenge, and expiration time.

Creation of a `did:cndomain` identifier is considered complete only when all of the following conditions are satisfied:

1. a conforming DID Document has been published at an authorized HTTPS endpoint
2. the DID Document is discoverable through the resolution and discovery procedures defined by this specification
3. the identifier can be successfully resolved to a DID Document whose `id` matches the requested DID exactly.

A governance profile MAY impose additional requirements for creation, including registration approval, policy review, eligibility checks, or higher-assurance control proof.

## 7.2 Read / Resolve

A `did:cndomain` identifier is resolved by performing the following steps:

1. parse the DID and confirm that the method name is `cndomain`
2. extract and normalize the domain anchor
3. determine the authoritative DID Document publication endpoint using the DNS discovery rules and fallback behavior defined by this specification
4. retrieve the DID Document and associated metadata over HTTPS
5. verify that the returned DID Document is consistent with the requested DID and the authoritative domain anchor
6. return the DID Document, `didDocumentMetadata`, and `didResolutionMetadata`.

Resolution MUST use HTTPS as the publication retrieval mechanism.

DNS, when used, serves primarily as a discovery mechanism and not as the authoritative carrier of the complete DID Document.

A resolver MUST reject a retrieved DID Document if:

1. the `id` value does not exactly match the requested DID
2. the domain anchor in the DID is inconsistent with the domain used in discovery or fallback construction
3. the document is retrieved from a non-authoritative or invalid endpoint according to this specification or an applicable profile
4. the DID has entered a lifecycle state that prohibits the requested trust use.

Detailed discovery and retrieval behavior for baseline conformance is defined in Section 8.

Companion resolution profiles MAY define stricter or deployment-specific refinements.

## 7.3 Update

A `did:cndomain` DID Document MAY be updated only when all of the following conditions are satisfied:

1. the requester proves authorization using a valid DID update key
2. the requester proves current legitimate control over the corresponding `.cn` domain
3. the requested modification is permitted by the current lifecycle state and applicable governance rules.

Permitted updates MAY include:

- key rotation
- addition, modification, or removal of verification methods
- addition, modification, or removal of service entries
- controller changes
- object-level delegation changes
- updates to method-specific metadata
- status transitions where permitted.

An implementation SHOULD ensure that update requests are bound to:

- the target DID
- the corresponding domain
- the requested operation
- a verifier-issued nonce or challenge
- a timestamp or expiration time.

An implementation SHOULD record update-related metadata, including:

- update timestamp
- version identifier
- update actor or submitting party, where available
- proof method used
- resulting lifecycle state.

Further requirements relating to domain control binding are defined in Section 10.

Companion control-proof profiles MAY further refine operational procedures.

## 7.4 Suspend / Freeze

A `did:cndomain` identifier MAY enter a temporary restricted state prior to final deactivation.

This specification recognizes two temporary restricted states:

1. `suspended`
2. `frozen`

A `did:cndomain` identifier MAY enter `suspended` or `frozen` state when triggered by events such as:

- domain status anomalies
- dispute handling
- judicial or regulatory orders
- compliance review
- loss of control investigation
- registry or registrar action
- other governance-defined events.

When a DID is in a restricted state:

- updates MAY be blocked or limited according to policy
- resolution MAY continue, but returned metadata SHOULD indicate the applicable restricted state
- relying parties SHOULD interpret the DID consistently with the applicable lifecycle rules.

The method-level meaning of lifecycle states is defined in Section 11.

## 7.5 Deactivate

A `did:cndomain` identifier MAY be deactivated when one or more of the following conditions occur:

1. the authorized subject or controller requests final deactivation
2. the corresponding `.cn` domain expires and is no longer considered under valid control
3. the domain is cancelled, deleted, or otherwise ceases to exist in a manner that invalidates the DID anchor
4. the domain is transferred or otherwise removed from the control of the entity previously authorized to manage the DID, and no continuity rule preserves the DID state
5. a dispute, judicial decision, regulatory action, or governance determination requires final deactivation
6. another method-defined or governance-defined condition for final deactivation is met.

Once a DID is deactivated:

- it MUST NOT be treated as active for authentication, authorization, or control purposes
- further updates MUST be rejected unless a governance profile explicitly defines a reactivation procedure
- resolution results SHOULD indicate deactivation through appropriate metadata or structured error signaling.

A deactivated DID Document MAY still be resolvable for historical, audit, or transparency purposes, provided that the returned metadata clearly indicates that the DID is deactivated and no longer valid for active trust decisions.

## 7.6 Reactivation

By default, deactivation SHOULD be treated as final.

A governance profile MAY define a reactivation procedure, but only under narrowly scoped and explicitly governed circumstances, such as:

- administrative correction of erroneous deactivation
- recovery after verified interruption of control
- continuity restoration following approved dispute resolution.

If reactivation is supported, it MUST require:

1. proof of authorization by a valid recovery or update mechanism recognized by governance policy
2. proof of current legitimate control over the corresponding `.cn` domain
3. explicit compliance with the applicable governance rules for restoration.

A reactivated DID MUST return metadata that makes the restoration event auditable.

Detailed technical procedures for such authorization MAY be further refined in the `did:cndomain Control Proof Profile`, if adopted.

## 7.7 Lifecycle Consistency

Implementations of `did:cndomain` MUST ensure that lifecycle operations remain consistent with the status of the underlying `.cn` domain anchor.

At a minimum, implementations SHOULD account for the following domain events:

- initial registration
- expiration
- cancellation
- deletion
- transfer
- registrar or registry hold
- dispute lock
- judicial or regulatory freeze.

A material change in domain control or domain validity SHOULD trigger re-evaluation of DID lifecycle state.

If a domain event invalidates current legitimate control, the DID MUST NOT continue to accept control-sensitive operations on the basis of stale or previously valid authorization alone.

The method-level meaning of lifecycle states is defined in Section 11, and further requirements relating to domain control binding and continuity are defined in Section 10.

Companion control-proof profiles MAY further refine operational procedures.

# 8. DID Resolution

This section defines the resolution model for the `did:cndomain` method.

A `did:cndomain` identifier is resolved by combining:

1. DNS-based discovery of the DID Document publication endpoint
2. HTTPS-based retrieval of the DID Document and associated metadata.

Under this method, DNS serves primarily as a discovery plane, while HTTPS serves as the publication and retrieval plane for the DID Document.

This specification defines baseline discovery procedures, endpoint selection rules, metadata structures, and error signaling conventions for method conformance.

Companion resolution profiles, if adopted, MAY define stricter or deployment-specific refinements.

## 8.1 Resolution Model

A conforming `did:cndomain` resolver MUST process a DID in a manner consistent with the following model:

1. parse the DID
2. validate that the DID conforms to the `did:cndomain` syntax
3. extract and normalize the domain anchor
4. determine the authoritative DID Document publication endpoint
5. retrieve the DID Document over HTTPS
6. validate consistency between the requested DID and the retrieved DID Document
7. return the DID Document together with applicable resolution metadata.

A resolver MUST NOT treat DNS records alone as a complete authoritative DID Document representation under this specification.

## 8.2 Resolution Inputs

A resolver MUST accept a `did:cndomain` identifier as input.

A resolver MAY additionally accept method-appropriate resolution options, including:

- preferred representation media type
- version identifier
- version time
- fallback behavior controls
- profile-specific policy options.

If resolution options are provided, the resolver MUST apply them consistently with this specification and any applicable profile.

## 8.3 DID Parsing and Domain Extraction

Before attempting discovery or retrieval, the resolver MUST:

1. confirm that the DID method name is exactly `cndomain`
2. parse the method-specific identifier according to this specification
3. determine whether the DID is domain-level or object-level
4. extract the normalized `.cn` domain anchor.

If the DID is syntactically invalid, the resolver MUST reject it and return an appropriate resolution error.

If the extracted domain is not under the `.cn` namespace, the resolver MUST reject the DID as invalid under this method.

## 8.4 Discovery of the Publication Endpoint

A resolver MUST determine the authoritative HTTPS endpoint for the DID Document using one of the following mechanisms:

1. DNS discovery records under `_did.<domain>`
2. a default HTTPS path construction rule defined by this specification.

When DNS discovery is supported, a resolver SHOULD attempt records in the following priority order:

1. `_did.<domain>` URI record
2. `_did.<domain>` TXT record containing a `did-url=` or `did-base=` HTTPS value.

For URI-based discovery:

1. the URI target MUST be HTTPS
2. non-HTTPS targets MUST be rejected
3. a URI target ending in `/` is interpreted as a base publication endpoint
4. a URI target not ending in `/` is interpreted as an exact DID Document URL.

For TXT-based discovery:

1. only TXT entries containing `did-url=` or `did-base=` are eligible for discovery
2. the extracted `did-url` or `did-base` value MUST be HTTPS
3. `did-url=` identifies an exact DID Document URL
4. `did-base=` identifies a base publication endpoint
5. TXT entries without a valid HTTPS `did-url=` or `did-base=` item MUST be ignored.

An exact DID Document URL is authoritative only for the requested DID whose DID Document is retrieved from that URL. If an exact URL returns a DID Document whose `id` does not exactly match the requested DID, the resolver MUST reject the result.

A base publication endpoint is a HTTPS URL prefix from which this specification's path construction rules are applied. When a base publication endpoint is discovered, a resolver MUST construct the final retrieval URL as follows:

- domain-level DID: `<base>/cndomain.json`
- object-level DID: `<base>/cndomain/<object-type>/<object-name>.json`

For this construction, the resolver MUST normalize a base endpoint to exactly one trailing `/` before appending the method-specific path.

If both URI and TXT records are present and inconsistent, the resolver MUST treat discovery as a conflict and MUST return a resolution error. In this case, the resolver MUST NOT silently continue with default fallback path construction.

If no acceptable DNS discovery record is found, the resolver MAY fall back to default HTTPS path construction unless policy requires DNS discovery.

Default fallback paths under this method are:

- domain-level DID: `https://<domain>/.well-known/did/cndomain.json`
- object-level DID: `https://<domain>/.well-known/did/cndomain/<object-type>/<object-name>.json`

For deterministic fallback path construction, a resolver MUST:

1. validate and normalize `<object-type>` and `<object-name>` according to Sections 5.5 and 5.6 before path construction
2. insert normalized object segments verbatim in the fallback path without additional transcoding
3. reject fallback construction if either object segment violates the Section 5.5 character constraints.

## 8.5 HTTPS Retrieval

Once the publication endpoint has been determined, the resolver MUST retrieve the DID Document over HTTPS.

When performing HTTPS retrieval, the resolver:

1. MUST validate the TLS certificate according to normal HTTPS validation rules
2. MUST reject insecure redirection to HTTP
3. MAY follow HTTPS-to-HTTPS redirects only where permitted by policy
4. SHOULD limit redirect depth.

A resolver MAY send representation preferences using a suitable request header or equivalent mechanism where supported.

## 8.6 DID Document Consistency Validation

After retrieving a candidate DID Document, the resolver MUST verify at least the following:

1. the `id` property of the DID Document exactly matches the requested DID
2. the domain anchor represented by the DID is consistent with the domain used for discovery or fallback construction
3. for an object-level DID, the object portion is consistent with the requested identifier
4. the publication endpoint is consistent with this specification or an applicable profile.

A resolver MUST reject the retrieved DID Document if any of the above validations fail.

A resolver SHOULD additionally validate any lifecycle state or policy signals that affect whether the DID may be relied upon for the intended purpose.

## 8.7 Resolution Output

A successful resolution result SHOULD include:

1. a DID Document
2. DID document metadata
3. DID resolution metadata.

The DID document metadata MAY include information such as:

- `created`
- `updated`
- `versionId`
- `deactivated`, when the DID is deactivated
- `cndomainLifecycleState`, for method-specific lifecycle states such as `active`, `suspended`, `frozen`, or `deactivated`
- `nextUpdate`, where applicable.

The DID resolution metadata MAY include information such as:

- `contentType`
- `discoverySource`
- `retrievalUrl`
- `redirect`
- `warning` or `warnings`
- `error`.

The precise metadata fields MAY be defined more strictly by deployment policy or companion profiles.

## 8.8 Lifecycle-Aware Resolution

A resolver MUST take DID lifecycle state into account during resolution.

At a minimum, a resolver SHOULD distinguish among the following states where such information is available:

- `active`
- `suspended`
- `frozen`
- `deactivated`

If a DID is `deactivated`, the resolver MUST NOT present it as an active identifier for trust decisions.

When a deactivated DID is successfully resolved as a deactivated DID, the resolver SHOULD indicate this through DID document metadata, including `deactivated: true`, rather than treating deactivation itself as an ambiguous retrieval failure.

If a DID is `suspended` or `frozen`, the resolver MAY still return a DID Document, but the returned metadata SHOULD clearly indicate the restricted state.

A governance profile MAY define stricter behavior for restricted states, including partial resolution, flagged resolution, or rejection.

This section defines the operational consequences of lifecycle state during resolution. The method-level meaning of lifecycle states is defined in Section 11.

## 8.9 Version-Aware and Time-Aware Resolution

A resolver MAY support version-aware or time-aware resolution.

If such support is provided, the resolver SHOULD define how the following inputs are processed:

- `versionId`
- `versionTime`

Where historical or versioned resolution is supported, the resolver MUST ensure that the returned result is clearly distinguished from the currently active representation.

A historical or previous representation MUST NOT be confused with current active authority.

## 8.10 Resolution Failure

If resolution fails, the resolver SHOULD return structured failure information.

Resolution failure MAY result from one or more of the following:

- invalid DID syntax
- invalid method-specific identifier
- non-`.cn` domain anchor
- failure to discover a publication endpoint
- HTTPS retrieval failure
- invalid DID Document
- mismatch between the requested DID and the returned DID Document
- restricted lifecycle state
- profile or governance policy restriction.

A resolver SHOULD signal such failures through structured resolution metadata rather than through ambiguous or implementation-specific behavior.

A resolver SHOULD support a stable, documented error vocabulary.

Recommended error values include:

- `invalidDid`
- `invalidMethodSpecificId`
- `domainNotCn`
- `dnsDiscoveryNotFound`
- `httpsRetrievalFailed`
- `invalidDidDocument`
- `idMismatch`
- `statusSuspended`
- `statusFrozen`
- `notFound`

## 8.11 Caching and Freshness

A resolver MAY cache discovery results and retrieved DID Documents, subject to applicable policy.

When caching is used, the resolver SHOULD consider:

- DNS TTL values for discovery records
- HTTPS caching directives, where safe and applicable
- lifecycle sensitivity to domain status changes
- the risk of stale authority after domain transfer, expiry, dispute, or freeze.

A resolver SHOULD avoid long-lived reuse of cached discovery or document data when current domain control may have materially changed.

## 8.12 Relationship to Resolution and Discovery Profile

This section defines the method-level resolution principles for `did:cndomain`.

This section also defines the baseline operational requirements required for standalone method conformance.

The `did:cndomain Resolution and Discovery Profile`, if used, MAY define stricter or deployment-specific procedures.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics and baseline behavior, while the applicable profile governs additional operational behavior, unless otherwise explicitly stated.

# 9. DID Document Representation

This section defines the representation requirements for DID Documents associated with the `did:cndomain` method.

A DID Document for `did:cndomain` MUST conform to the applicable DID Core representation requirements. Under this method, the DID Document serves as the authoritative expression of identifier control, verification material, service binding, and, where applicable, delegated authority and lifecycle-related metadata.

A `did:cndomain` DID Document is retrieved over HTTPS in accordance with this specification. Companion profiles MAY define stricter publication and retrieval conventions.

Examples in this section that use only DID Core terms use only the DID Core context. Examples that use extension terms include additional JSON-LD contexts for those terms.

## 9.1 General Representation Requirements

A DID Document associated with `did:cndomain`:

1. MUST be a valid DID Document representation
2. MUST contain an `id` property
3. MUST use an `id` value that exactly matches the DID being represented
4. MAY contain additional DID Core properties where appropriate
5. MAY contain method-specific properties or metadata where permitted by this specification or an applicable profile
6. MUST include an additional JSON-LD context that defines those terms when method-specific extension terms are present in DID Document properties.

A relying party or resolver MUST reject a retrieved DID Document if its `id` property does not exactly match the requested DID.

Method-specific terms that appear only in DID document metadata or DID resolution metadata do not, by themselves, require extension terms in the DID Document representation.

## 9.2 Minimum Required Properties

At a minimum, a conforming active `did:cndomain` DID Document SHOULD include:

- `@context`
- `id`

A conforming `did:cndomain` DID Document intended for active trust use SHOULD additionally include, where applicable:

- `controller`
- `verificationMethod`
- `authentication`
- `assertionMethod`
- `service`

The omission of one or more of these properties does not automatically make a DID Document invalid, but it MAY limit the purposes for which the DID can be used.

## 9.3 Domain-Level DID Documents

A domain-level DID Document represents a DID whose subject is the `.cn` domain itself or a valid subdomain under `.cn`.

For a domain-level DID Document:

1. the `id` MUST be the exact domain-level DID
2. the authority expressed by the DID Document MUST be consistent with the corresponding `.cn` domain anchor
3. any listed controller, verification method, or service entry MUST be interpreted as being anchored to that domain-level subject.

Example:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suite/jws-2020/v1",
    "https://identity.foundation/.well-known/did-configuration/v1"
  ],
  "id": "did:cndomain:example.cn",
  "controller": "did:cndomain:example.cn",
  "verificationMethod": [
    {
      "id": "did:cndomain:example.cn#key-1",
      "type": "JsonWebKey2020",
      "controller": "did:cndomain:example.cn",
      "publicKeyJwk": {
        "kty": "OKP",
        "crv": "Ed25519",
        "x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ"
      }
    }
  ],
  "authentication": [
    "did:cndomain:example.cn#key-1"
  ],
  "assertionMethod": [
    "did:cndomain:example.cn#key-1"
  ],
  "service": [
    {
      "id": "did:cndomain:example.cn#did-document",
      "type": "LinkedDomains",
      "serviceEndpoint": "https://example.cn"
    }
  ]
}
```

## 9.4 Object-Level DID Documents

An object-level DID Document represents a DID whose subject is defined beneath a parent `.cn` domain.

For an object-level DID Document:

1. the `id` MUST be the exact object-level DID
2. the object-level subject MUST be interpreted in the context of the parent domain anchor
3. the authority of the object-level DID is, by default, subordinate to the corresponding parent domain-level DID unless a valid delegation model explicitly defines otherwise.

Example:

```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:cndomain:example.cn:service:web",
  "controller": "did:cndomain:example.cn"
}
```

An object-level DID Document MAY use the parent domain-level DID as its controller, or MAY identify another controller where such delegation is valid under this specification and any applicable governance profile.

## 9.5 Controller Semantics

A `did:cndomain` DID Document MAY contain a `controller` property.

Where present, the `controller` property identifies the DID or DIDs authorized to control the subject represented by the DID Document.

For `did:cndomain`:

1. a domain-level DID MAY identify itself as its own controller
2. an object-level DID MAY identify the parent domain-level DID as its controller
3. an object-level DID MAY identify another DID as controller only where such delegation is valid and remains subordinate to the current legitimate control of the parent `.cn` domain.

A `controller` declaration in a DID Document MUST NOT, by itself, override the domain-anchored authority and lifecycle rules defined by this specification.

## 9.6 Verification Material

A `did:cndomain` DID Document MAY contain one or more verification methods.

Where verification material is used for control-sensitive operations, implementations SHOULD distinguish among keys intended for different purposes, including:

- authentication
- assertion
- key agreement, where relevant
- capability invocation or update authorization, where relevant.

A DID Document SHOULD NOT rely on ambiguous key usage where more specific relationships can be expressed.

Where a key is intended to authorize updates or lifecycle operations, such use SHOULD be clearly identified through the applicable verification relationships or method-specific profile rules.

The presence of a verification method in the DID Document does not, by itself, prove current authority unless the lifecycle and control proof requirements of this specification are also satisfied.

## 9.7 Authentication and Assertion Relationships

A `did:cndomain` DID Document MAY include `authentication` and `assertionMethod` relationships.

Where these are present:

1. `authentication` indicates verification methods that may be used to authenticate as the DID subject
2. `assertionMethod` indicates verification methods that may be used to make claims or assertions on behalf of the DID subject.

Applications using `did:cndomain` SHOULD evaluate these relationships consistently with the domain-anchored trust and lifecycle model of this method.

## 9.8 Service Entries

A `did:cndomain` DID Document MAY include one or more `service` entries.

Service entries are especially relevant for this method because `did:cndomain` is intended to support domain-bound service discovery and domain-governed trust relationships.

A service entry:

1. MUST be consistent with the DID subject identified by the DID Document
2. SHOULD use a service type that is meaningful for the intended application context
3. MUST NOT be interpreted as independently authoritative outside the control and lifecycle rules of this method.

Service entries MAY identify, for example:

- DID publication endpoints
- API service endpoints
- resource access endpoints
- trust or attestation services
- transparency or audit services
- domain-bound application services.

Example:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://identity.foundation/.well-known/did-configuration/v1"
  ],
  "id": "did:cndomain:example.cn",
  "controller": "did:cndomain:example.cn",
  "service": [
    {
      "id": "did:cndomain:example.cn#linked-domain",
      "type": "LinkedDomains",
      "serviceEndpoint": "https://example.cn"
    }
  ]
}
```

## 9.9 Delegation and Subordinate Authority

A `did:cndomain` DID Document MAY express delegated control or subordinate authority relationships, particularly for object-level identifiers.

Where delegation is expressed:

1. the delegation MUST be explicit
2. the delegation MUST remain subordinate to valid current legitimate control of the parent `.cn` domain
3. the delegation MUST NOT be interpreted as surviving loss of legitimate current control over the parent domain unless an applicable governance profile explicitly defines such continuity.

A delegated controller MAY be used to operate an object-level DID, but such delegation MUST remain subject to method-level lifecycle, suspension, freezing, and deactivation rules.

## 9.10 Lifecycle-Related Metadata

A `did:cndomain` DID Document MAY be accompanied by lifecycle-related metadata.

Such metadata is typically returned as DID document metadata or DID resolution metadata rather than being embedded directly as authoritative subject data in the DID Document itself.

Relevant lifecycle-related values MAY include:

- creation time
- update time
- version identifier
- status state
- deactivation indicator
- governance restriction indicator.

Implementations SHOULD distinguish clearly between:

1. the DID Document itself
2. DID document metadata
3. DID resolution metadata.

An implementation MUST NOT treat non-authoritative transport metadata as if it were part of the DID subject’s authoritative cryptographic control state unless such interpretation is explicitly defined.

## 9.11 Representation Consistency

A `did:cndomain` DID Document representation is valid only when all of the following conditions are satisfied:

1. the `id` matches the DID being represented exactly
2. the document is retrieved from a publication endpoint determined in accordance with this specification or an applicable profile
3. the represented authority is consistent with the corresponding `.cn` domain anchor
4. any controller or delegated authority information is interpreted consistently with the parent domain authority model
5. the lifecycle state does not prohibit the intended use of the DID for the relevant trust decision.

A representation that is syntactically well-formed but inconsistent with the governing domain anchor or lifecycle state MUST NOT be treated as authoritative for active control-sensitive use.

## 9.12 Historical and Restricted Representations

A `did:cndomain` DID Document MAY be returned for historical, audit, transparency, or limited inspection purposes even where the DID is no longer fully active.

This may occur, for example, when the DID is:

- `suspended`
- `frozen`
- `deactivated`

In such cases:

1. the returned representation SHOULD be accompanied by metadata clearly indicating the relevant lifecycle state
2. the representation MUST NOT be treated as equivalent to a currently active DID for authentication, authorization, or control purposes.

## 9.13 Relationship to Profiles

This section defines the method-level representation requirements for `did:cndomain`.

An applicable profile MAY further define:

- preferred representation media types
- profile-specific service type conventions
- method-specific verification method usage constraints
- lifecycle metadata field names
- additional validation requirements for domain-level and object-level representations.

Profiles are optional for baseline method conformance unless explicitly adopted by a deployment.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics and baseline representation behavior, while the applicable profile governs additional operational behavior, unless otherwise explicitly stated.

# 10. Domain Control Binding

This section defines how `did:cndomain` identifiers are bound to current legitimate control of the corresponding `.cn` domain anchor.

The `did:cndomain` method is domain-anchored by design. Accordingly, control over a `did:cndomain` identifier is not determined solely by possession of cryptographic key material. It is also dependent on current legitimate control of the corresponding `.cn` domain.

This domain control binding is a core method property and applies to both:

1. domain-level DIDs
2. object-level DIDs derived from a parent `.cn` domain.

## 10.1 Binding Principle

A `did:cndomain` identifier MUST be interpreted as anchored to the corresponding `.cn` domain.

For a domain-level DID, the domain anchor is the domain identified directly by the DID.

For an object-level DID, the domain anchor is the parent `.cn` domain from which the object-level identifier is derived.

A control-sensitive lifecycle operation MUST NOT be authorized solely on the basis of historical association, prior publication, or continued possession of an update key, unless current legitimate control of the corresponding `.cn` domain can also be established in accordance with this specification.

## 10.2 Dual Control Requirement

For operations that create, update, delegate, suspend, freeze, deactivate, or reactivate a `did:cndomain` identifier, an implementation MUST require both:

1. proof of authorization through a valid DID control, initial creation, update, or recovery mechanism appropriate to the operation
2. proof of current legitimate control over the corresponding `.cn` domain.

An applicable governance profile MAY impose stronger requirements for sensitive operations, including:

- multiple proof methods
- registrar-backed confirmation
- administrative approval
- dispute review
- auditable governance action.

The dual control requirement exists to ensure that cryptographic control and namespace control remain aligned.

## 10.3 Baseline Control Proof Procedure

This specification defines a baseline control proof procedure so that baseline method conformance does not depend on a separate control-proof profile.

A control proof procedure for `did:cndomain` MUST verify both DID-side authorization and domain-side control.

For DID-side authorization:

1. initial creation MUST prove possession of the verification material to be authorized by the initial DID Document
2. update, delegation, suspension, freezing, deactivation, and reactivation MUST prove authorization by a currently valid update, recovery, or governance-recognized control mechanism
3. the proof MUST be bound to the target DID, domain anchor, requested operation, verifier challenge, issuance time, and expiration time.

For domain-side control, a baseline implementation MUST support at least one of the following proof methods:

- DNS challenge proof
- HTTPS challenge proof
- registrar-backed or governance-authorized proof.

A DNS challenge proof MUST use a TXT record under `_did-challenge.<domain>` containing a verifier-issued challenge value. The challenge value MUST be bound to the requested DID, normalized domain, operation, issuance time, and expiration time.

An HTTPS challenge proof MUST publish a challenge representation at:

```text
https://<domain>/.well-known/did/cndomain-challenge.json
```

The HTTPS challenge representation MUST identify the requested DID, normalized domain, operation, challenge value, issuance time, and expiration time.

A registrar-backed or governance-authorized proof MAY be accepted only when the accepting implementation can verify the proof source, scope, freshness, and applicability to the requested operation.

All baseline control proofs MUST satisfy the following requirements:

1. the proof MUST be fresh and time-bound
2. the proof MUST be bound to the specific DID and normalized domain anchor
3. the proof MUST be bound to the requested operation
4. the proof MUST include or reference a verifier-issued challenge or nonce
5. expired, reused, mismatched, or context-incomplete proofs MUST be rejected
6. object-level DID proofs MUST bind to the parent `.cn` domain anchor.

To reduce cross-domain replay risk, challenge construction SHOULD include explicit domain-specific context, such as the normalized domain value or a verifier-derived digest bound to that value, rather than relying on an unscoped random nonce alone.

An applicable control-proof profile MAY define stricter record names, payload formats, proof suites, assurance levels, retention rules, or registrar-backed procedures, but it MUST NOT weaken the baseline dual-control semantics defined here.

## 10.4 Domain-Level Binding

For a domain-level DID, the corresponding `.cn` domain is the primary naming and authority anchor.

Examples:

```text
did:cndomain:example.cn
did:cndomain:sub.example.cn
```

For such identifiers:

1. the domain anchor MUST remain valid under the `.cn` namespace
2. the subject’s authoritative control state MUST remain consistent with current legitimate control of that domain
3. the DID MUST be re-evaluated if the domain’s control status materially changes.

A domain-level DID MUST NOT continue to be treated as fully authoritative if the corresponding domain has expired, been cancelled, been transferred, or is otherwise no longer under legitimate control of the party asserting authority over the DID.

## 10.5 Object-Level Binding

For an object-level DID, the corresponding parent `.cn` domain remains the root authority anchor.

Examples:

```text
did:cndomain:example.cn:service:web
did:cndomain:example.cn:resource:gpu0
```

For such identifiers:

1. the object-level DID is subordinate to the authority of the parent domain
2. the object-level DID MUST NOT be treated as independently authoritative outside the context of the parent domain anchor
3. loss of legitimate current control over the parent domain SHOULD trigger re-evaluation of the object-level DID’s validity, authority, and lifecycle state.

An object-level DID MAY express delegated control, but such delegation MUST remain subordinate to the continued current legitimate control of the parent `.cn` domain unless an applicable governance profile explicitly defines a continuity exception.

## 10.6 Effect of Domain Lifecycle Events

Because `did:cndomain` identifiers are domain-anchored, domain lifecycle events MAY materially affect DID validity and authority.

Relevant domain lifecycle events MAY include:

- initial registration
- renewal
- expiration
- cancellation
- deletion
- transfer
- registrar or registry hold
- dispute-related restriction
- judicial freeze
- regulatory intervention
- another governance-recognized change in control status.

An implementation SHOULD re-evaluate the corresponding DID lifecycle state when such an event occurs or is detected.

A material domain event MAY result in:

- continued active state
- temporary restriction
- frozen state
- deactivation
- another governance-defined outcome.

## 10.7 Transfer Sensitivity

Domain transfer is a particularly important case for `did:cndomain`.

If current legitimate control of the domain changes, an implementation MUST NOT assume that previously authorized DID update keys remain sufficient for future control-sensitive operations.

Unless an applicable governance profile explicitly defines a continuity procedure:

1. prior key possession alone MUST NOT authorize continued control after transfer
2. the DID SHOULD be re-evaluated for continuity, restriction, or deactivation
3. subordinate object-level DIDs SHOULD also be re-evaluated.

At minimum, transfer handling SHOULD include an explicit continuity review phase.

During this phase:

1. the DID SHOULD be placed into `suspended` or `frozen` state until a governance decision is reached
2. control-sensitive updates MUST remain blocked unless an applicable continuity procedure explicitly authorizes a narrowly scoped exception
3. if continuity for the existing DID is approved, the assuming controller MUST satisfy the dual-control proof requirements of Section 10.3 for the requested operation
4. if continuity is not approved, the DID SHOULD be restricted or deactivated according to policy, and any subsequent authority claim SHOULD use an explicit governance-defined procedure (for example, a fresh create flow where permitted).

This rule exists to prevent stale authority from surviving a real change in namespace control.

## 10.8 Loss of Control

If current legitimate control of the corresponding `.cn` domain can no longer be demonstrated, then:

1. new control-sensitive operations MUST be rejected
2. update authority MUST be treated as invalid unless restored through an applicable procedure
3. the DID SHOULD enter an appropriate restricted or deactivated lifecycle state depending on policy and circumstances.

Loss of control MAY arise from:

- expiry without effective renewal
- cancellation or deletion
- failure of control proof
- dispute or judicial restriction
- transfer to another party
- another governance-recognized event.

## 10.9 Delegation Constraints

A `did:cndomain` DID Document MAY express delegated authority for certain operations, especially for object-level DIDs.

However, a delegation:

1. MUST be explicit
2. MUST remain subordinate to the parent domain authority model
3. MUST NOT be interpreted as surviving loss of current legitimate control over the corresponding `.cn` domain unless an applicable governance profile explicitly permits such continuity.

Accordingly, delegation under this method is conditional, not absolute.

## 10.10 Relationship to Control Proof

This section defines the method-level semantics of domain control binding.

This specification defines the baseline control-proof requirements for method conformance, including:

- dual proof (authorized key control and current domain control)
- proof freshness
- replay protection
- operation and DID/domain binding.

The `did:cndomain Control Proof Profile`, if used, MAY define stricter procedures and policy levels.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics and baseline control-binding behavior, while the applicable profile governs additional operational behavior, unless otherwise explicitly stated.

## 10.11 Binding Consistency Requirement

A `did:cndomain` identifier MUST NOT be treated as authoritative for control-sensitive trust decisions unless its DID authority remains consistent with current legitimate control of the corresponding `.cn` domain.

This requirement applies even where:

- the DID Document is syntactically valid
- the DID Document remains resolvable
- an old update key is still available
- historical records suggest prior authority.

For `did:cndomain`, namespace control and DID control are intentionally bound together.

# 11. Lifecycle Status Model

This section defines the lifecycle status model for `did:cndomain`.

Because `did:cndomain` identifiers are anchored to the `.cn` domain namespace, DID lifecycle state is sensitive not only to DID-specific actions, but also to domain state, control changes, governance events, and related restrictions.

A `did:cndomain` identifier MUST be interpreted in light of its lifecycle state where such state is available.

## 11.1 Status Overview

This specification defines the following primary lifecycle states:

1. `active`
2. `suspended`
3. `frozen`
4. `deactivated`

An applicable governance profile MAY define additional operational sub-states, provided that such sub-states do not conflict with the method-level semantics defined here.

## 11.2 active

A DID in the `active` state is valid for normal use under this method.

When a `did:cndomain` identifier is `active`:

1. it MAY be resolved normally
2. it MAY be used for permitted trust decisions, subject to application policy
3. control-sensitive operations MAY be accepted, provided that required authorization and domain control proof are satisfied.

A DID SHOULD be treated as `active` only when there is no applicable restriction, freeze, or deactivation condition affecting the corresponding DID or its domain anchor.

## 11.3 suspended

A DID in the `suspended` state is temporarily restricted.

The `suspended` state is intended for situations where the DID remains identifiable and potentially recoverable, but normal control-sensitive operations should not proceed without further review or restoration.

A `did:cndomain` identifier MAY enter `suspended` state due to events such as:

- temporary domain state anomaly
- pending control verification
- operational or administrative hold
- temporary policy restriction
- incomplete renewal or control confirmation
- another governance-recognized temporary condition.

When a DID is `suspended`:

1. resolution MAY continue
2. the returned metadata SHOULD indicate the suspended state
3. updates MAY be blocked or limited according to policy
4. relying parties SHOULD treat the DID as restricted.

A `suspended` DID MUST NOT be silently treated as fully equivalent to an `active` DID.

## 11.4 frozen

A DID in the `frozen` state is subject to a stronger restriction than `suspended`.

The `frozen` state is intended for situations involving dispute, judicial action, regulatory intervention, compliance enforcement, or another governance-imposed restriction requiring stricter control.

A `did:cndomain` identifier MAY enter `frozen` state due to events such as:

- domain ownership dispute
- judicial freeze or injunction
- regulatory restriction
- registry or registrar lock associated with governance action
- serious control inconsistency requiring adjudication
- another governance-defined high-restriction event.

When a DID is `frozen`:

1. control-sensitive updates MUST be blocked unless explicitly allowed by governance policy
2. resolution MAY continue in flagged or restricted form
3. returned metadata SHOULD clearly indicate the frozen state
4. the DID MUST NOT be treated as fully active for control-sensitive trust decisions.

A `frozen` DID MAY later transition to:

- `active`, if the restriction is resolved
- `suspended`, if a lower restriction remains appropriate
- `deactivated`, if final termination is required.

## 11.5 deactivated

A DID in the `deactivated` state is no longer valid for active trust use under this method.

A `did:cndomain` identifier MAY enter `deactivated` state when, for example:

- the authorized controller requests final deactivation
- the corresponding domain expires and is no longer under valid control
- the domain is cancelled or deleted
- the domain anchor ceases to be valid for continued DID authority
- governance policy, judicial decision, or regulatory action requires final deactivation
- another method-defined or governance-defined final condition is met.

When a DID is `deactivated`:

1. it MUST NOT be treated as active for authentication, authorization, or control purposes
2. further updates MUST be rejected unless an applicable governance profile explicitly defines a restoration mechanism
3. resolution results SHOULD indicate deactivation through metadata or structured error signaling.

A deactivated DID MAY still be returned for historical, audit, transparency, or inspection purposes, but MUST NOT be presented as a currently active authority.

## 11.6 State Transitions

A `did:cndomain` identifier MAY transition between lifecycle states in accordance with this specification and any applicable governance profile.

Typical transitions MAY include:

- `active` → `suspended`
- `active` → `frozen`
- `active` → `deactivated`
- `suspended` → `active`
- `suspended` → `frozen`
- `suspended` → `deactivated`
- `frozen` → `active`
- `frozen` → `suspended`
- `frozen` → `deactivated`

By default, `deactivated` SHOULD be treated as final.

A governance profile MAY define narrowly scoped reactivation conditions, but such reactivation MUST be explicit, auditable, and policy-governed.

## 11.7 State Determination Factors

Lifecycle state determination SHOULD take into account, where applicable:

- the current state of the corresponding `.cn` domain
- whether current legitimate control can still be demonstrated
- whether control-sensitive proof procedures succeed
- whether dispute, judicial, regulatory, or compliance restrictions apply
- whether governance policy requires temporary or final restriction
- whether the DID Document remains consistent with the current authority state.

No lifecycle state decision SHOULD be based solely on stale historical publication if current domain authority has materially changed.

## 11.8 Resolution Behavior by State

A resolver SHOULD take lifecycle state into account when returning a resolution result.

Recommended behavior:

- `active`: return normal resolution output
- `suspended`: return the DID Document where allowed, with metadata indicating suspension
- `frozen`: return the DID Document only where permitted, with clear metadata or structured restriction signaling
- `deactivated`: return deactivation metadata or structured error signaling, and do not present the DID as active.

An applicable resolution profile MAY define more specific error codes, metadata field names, or policy-driven restrictions for each state.

This section defines the method-level meaning of lifecycle states during resolution. Detailed operational handling of restricted states MAY be further refined by applicable resolution and governance profiles.

## 11.9 Metadata Signaling

Lifecycle state SHOULD be signaled through DID document metadata, DID resolution metadata, or another profile-defined mechanism.

Recommended status values include:

- `active`
- `suspended`
- `frozen`
- `deactivated`

Where state metadata is returned, it SHOULD be sufficiently clear to allow relying parties to distinguish:

1. whether the DID is currently usable for trust decisions
2. whether the returned representation is current, restricted, or historical.

An implementation SHOULD avoid embedding temporary transport or operational state directly into the DID subject semantics unless such treatment is explicitly defined.

## 11.10 Governance and Auditability

Lifecycle state changes SHOULD be auditable.

An implementation SHOULD record, where applicable:

- the prior state
- the new state
- the reason for the transition
- the triggering event
- the time of transition
- the actor, authority, or process responsible for the transition.

High-impact transitions SHOULD receive stricter audit treatment, especially:

- transition to `frozen`
- transition to `deactivated`
- restoration from restricted state
- any governance-authorized override.

Further governance-specific procedures MAY be defined in an applicable governance profile.

## 11.11 Relationship to Method Operations

This section defines the method-level meaning of lifecycle states.

The operational consequences of lifecycle state, including whether updates are allowed, whether reactivation is possible, and how resolution behaves, MUST be applied consistently with:

- Section 7 (Method Operations)
- Section 8 (DID Resolution)
- Section 10 (Domain Control Binding).

An applicable governance profile MAY refine operational details, but MUST NOT contradict the method-level meaning of the lifecycle states defined here.

## 11.12 State Consistency Requirement

A `did:cndomain` identifier MUST NOT be treated as fully active if its known lifecycle state is inconsistent with current legitimate control of the corresponding `.cn` domain.

If lifecycle information is available and indicates restriction or final invalidation, relying parties, resolvers, and management systems MUST interpret the DID accordingly.

For this method, lifecycle state is not an optional cosmetic property. It is part of the authoritative interpretation of the DID within the domain-anchored trust model.

# 12. Governance Considerations

This section defines the governance considerations applicable to the `did:cndomain` method.

The `did:cndomain` method is domain-governed by design. Accordingly, its operation depends not only on syntactic validity and cryptographic proof, but also on governance rules that determine how domain control, lifecycle events, disputes, restrictions, and exceptional cases are to be interpreted.

This section does not attempt to define a single universal governance institution. Instead, it defines the categories of governance concern that a conforming deployment, profile, or ecosystem implementation of `did:cndomain` SHOULD address.

## 12.1 Governance Scope

A governance framework for `did:cndomain` SHOULD address at least the following:

1. how current legitimate control of a `.cn` domain is recognized
2. who is eligible to create, update, delegate, suspend, freeze, deactivate, or reactivate a DID
3. how disputes and conflicting claims are handled
4. how registrar, registry, administrative, judicial, or regulatory actions affect DID state
5. how lifecycle state transitions are triggered, reviewed, and recorded
6. how exceptional or override actions are constrained and audited.

The governance framework MAY be implemented by one or more technical, administrative, or policy mechanisms.

## 12.2 Governance Roles

An implementation or deployment profile MAY define governance roles, including but not limited to:

- domain controller
- delegated controller
- DID operator
- resolver operator
- registrar-associated authority
- registry-associated authority
- governance administrator
- dispute handling authority
- compliance or regulatory authority.

The existence of a role in governance documentation MUST NOT, by itself, imply unrestricted authority over a DID. Any authority claimed by such a role MUST remain consistent with the method-level rules of domain anchoring, lifecycle control, and proof requirements defined by this specification.

## 12.3 Eligibility and Authorization

A governance profile SHOULD define who may create or operate a `did:cndomain` identifier.

At a minimum, the governance framework SHOULD clarify:

1. whether any valid current controller of a `.cn` domain may create a DID for that domain
2. whether object-level DIDs may be created freely under domain authority or are subject to additional constraints
3. whether delegated operators may manage object-level DIDs
4. whether certain names, object types, or categories are reserved, restricted, or prohibited.

A governance framework MAY define higher-assurance requirements for sensitive identifiers or operations.

## 12.4 Domain Event Handling

A governance framework for `did:cndomain` SHOULD define how domain events affect DID interpretation and lifecycle.

Relevant events MAY include:

- initial registration
- renewal
- expiration
- cancellation
- deletion
- transfer
- registrar or registry hold
- lock state
- dispute
- judicial or regulatory action
- control proof failure
- another governance-recognized change affecting legitimate authority.

The governance framework SHOULD define whether each such event results in:

- no DID state change
- temporary restriction
- freezing
- deactivation
- continuity review
- another governance-defined consequence.

Under this method, governance mappings for domain-status signals (including EPP status codes or equivalent registrar/registry status indicators) SHOULD be explicit, deterministic, and auditable.

At minimum, the governance framework SHOULD define:

1. which status signals are recognized as domain events for lifecycle interpretation
2. how each recognized signal affects lifecycle state and whether control-sensitive operations are blocked, limited, or allowed
3. how unknown, conflicting, or unverifiable signals are handled conservatively (for example, temporary `suspended` or `frozen` treatment pending review).

Domain-status signals MUST NOT, by themselves, be treated as sufficient authorization for control-sensitive operations. Applicable DID-side authorization and domain-control proof requirements remain in force.

## 12.5 Continuity and Transfer Governance

Because `did:cndomain` is domain-anchored, continuity across domain transfer or control change is a governance-sensitive matter.

A governance framework SHOULD define:

1. whether a DID continues across domain transfer
2. whether transfer requires DID revalidation, restriction, or deactivation
3. whether object-level DIDs inherit the same continuity outcome as the parent domain-level DID
4. whether a new controller may assume control of an existing DID, or whether a new DID should be created.

Unless explicitly defined otherwise by governance policy, continuity SHOULD NOT be assumed automatically after a material change in domain control.

To avoid ambiguous takeover semantics, the governance framework SHOULD define transfer outcomes as explicit, auditable control-sensitive decisions rather than implicit side effects of publication timing.

## 12.6 Disputes and Conflicting Claims

A governance framework SHOULD define how disputes and conflicting authority claims are handled.

Such disputes MAY involve:

- competing controller claims
- inconsistent proof submissions
- disputed domain authority
- legal or policy restrictions
- challenges to delegation
- challenges to continuity after transfer.

Where a dispute exists, the governance framework MAY require:

- temporary suspension
- frozen state
- manual review
- registrar-backed confirmation
- administrative adjudication
- transparent logging of the decision.

A DID under unresolved dispute SHOULD NOT be treated as fully active for control-sensitive decisions.

At minimum, unresolved-dispute handling SHOULD apply explicit restricted-state treatment:

1. the DID SHOULD enter `suspended` or `frozen` state until the dispute is resolved through a defined governance process
2. control-sensitive operations MUST remain blocked unless a narrowly scoped governance exception explicitly applies
3. resolution results SHOULD surface dispute-related restriction metadata or equivalent structured signaling.

## 12.7 Exceptional and Administrative Actions

A governance framework MAY define exceptional actions, including:

- emergency freeze
- administrative deactivation
- override following judicial or regulatory direction
- correction of erroneous state transition
- controlled recovery after proven loss of access.

Such actions MUST be tightly constrained.

An exceptional action SHOULD:

1. have a defined legal, administrative, or governance basis
2. be limited in scope
3. be auditable
4. identify the responsible authority or process
5. avoid becoming a routine substitute for normal proof-based control.

## 12.8 Auditability and Transparency

A governance framework SHOULD define which events must be recorded for audit purposes.

At a minimum, auditable events SHOULD include, where applicable:

- DID creation
- key rotation
- controller change
- lifecycle state change
- deactivation
- reactivation
- delegated authority change
- registrar-backed or governance-authorized proof decision
- administrative override.

An implementation MAY use append-only logging, transparency services, signed event records, or other integrity-preserving audit mechanisms.

## 12.9 Governance Profiles

This specification permits governance profiles tailored to different deployment environments.

A governance profile MAY define:

- proof method requirements
- eligibility rules
- restricted object types
- state transition triggers
- continuity rules
- administrative review procedures
- audit and retention policies.

A governance profile MUST NOT contradict the method-level semantics defined by this specification, especially with respect to:

- domain anchoring
- dual control requirements for control-sensitive operations
- lifecycle interpretation
- the authority relationship between parent domain-level and subordinate object-level DIDs.

## 12.10 Governance Minimum Requirement

A `did:cndomain` deployment MUST NOT treat governance as an entirely external or irrelevant concern.

Because this method is domain-governed by design, a conforming deployment SHOULD define at least a minimal governance position covering:

1. who may control the DID
2. how current domain control is recognized
3. how restrictions and disputes are handled
4. how auditability is maintained for high-impact lifecycle actions.

Detailed governance procedures MAY be defined in an applicable governance profile.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics, while the applicable profile governs detailed operational behavior, unless otherwise explicitly stated.

# 13. Security Considerations

This section defines the principal security considerations for the `did:cndomain` method.

As required by DID Core, a DID method specification must describe security considerations relevant to method syntax, control, update, resolution, and lifecycle behavior. The `did:cndomain` method is domain-anchored and therefore has security properties that arise both from decentralized identifier management and from `.cn` domain control dependencies.

## 13.1 Security Model Overview

The security of `did:cndomain` depends on the integrity of all of the following:

1. the correctness of identifier syntax and normalization
2. the correctness of DNS-based discovery
3. the integrity of HTTPS-based DID Document publication
4. the authenticity and authorization of verification material
5. the correctness of lifecycle and governance state interpretation
6. the continued alignment between DID control and current legitimate domain control.

A failure in any of these areas MAY lead to incorrect trust decisions.

## 13.2 Stale Key Risk

A major security risk for `did:cndomain` arises when a previously valid DID control or update key remains available after the underlying domain authority has materially changed.

This MAY occur after:

- domain transfer
- expiration followed by loss of control
- dispute-related reassignment
- governance override
- another change in legitimate authority.

For this reason, implementations MUST NOT rely solely on historical DID key possession for control-sensitive operations. Update or lifecycle authority MUST remain conditioned on current legitimate domain control as defined by this specification.

## 13.3 Domain Hijacking and Unauthorized Control

An attacker who gains unauthorized control over a domain, or over the mechanisms used to prove domain control, MAY attempt to assume DID authority.

Risks include:

- unauthorized DNS modification
- DNS account compromise
- web hosting compromise
- TLS certificate abuse
- registrar account compromise
- control proof endpoint compromise.

Implementations SHOULD consider mitigations including:

- short-lived control proofs
- independent validation of domain control
- strong account security for DNS and HTTPS infrastructure
- auditing of proof-based operations
- rapid restriction or freezing procedures for suspected compromise.

## 13.4 DNS Discovery Risks

Because this method may use DNS for endpoint discovery, security risks include:

- forged DNS responses
- cache poisoning
- stale cached discovery records
- inconsistent discovery records
- malicious redirection to attacker-controlled publication endpoints.

Implementations SHOULD consider:

- integrity-protected DNS where available
- careful authoritative lookup strategies
- TTL-aware caching behavior
- rejecting insecure endpoint schemes
- consistency checks between discovered endpoints and the requested domain anchor.

DNS under this method is a discovery plane, not a complete trust plane. Discovery results SHOULD therefore always be validated against retrieved DID Document content and method rules.

## 13.5 HTTPS Retrieval Risks

Because this method uses HTTPS to retrieve DID Documents, security risks include:

- compromised publication hosts
- invalid TLS certificates
- unsafe redirects
- stale mirror content
- endpoint substitution attacks.

Resolvers and relying parties SHOULD:

1. validate TLS certificates properly
2. reject HTTP downgrade
3. limit redirect behavior
4. validate that the retrieved DID Document `id` matches the requested DID exactly
5. avoid assuming that HTTPS reachability alone proves legitimate authority.

Publishers and deployers of DID Document endpoints SHOULD enable HSTS and avoid deployment patterns that allow HTTPS downgrade exposure.

## 13.6 Inconsistent Authority Risk

A `did:cndomain` DID Document MAY be syntactically valid while still being inconsistent with the current domain authority state.

Examples include:

- a resolvable DID Document published after loss of current legitimate control
- a historical DID Document reused after transfer
- a delegated controller remaining listed after underlying authority has changed.

Implementations MUST account for the fact that syntactic validity and current authority are not always identical.

## 13.7 Delegation Risk

Object-level DIDs and delegated controllers introduce additional security complexity.

Risks include:

- excessive delegation scope
- forgotten delegated authority
- delegated control surviving longer than intended
- inconsistent interpretation of subordinate control.

Implementations SHOULD ensure that:

- delegation is explicit
- delegation is auditable
- delegation remains subordinate to the parent domain authority
- delegated authority is re-evaluated when domain control materially changes.

## 13.8 Replay and Proof Reuse

Where control-sensitive operations rely on proof mechanisms, replay attacks are a significant concern.

Implementations SHOULD ensure that proof procedures are:

- nonce-bound
- time-bound
- DID-bound
- domain-bound
- operation-bound.

Expired, reused, or context-mismatched proofs MUST be rejected.

## 13.9 Restricted-State Handling

A DID in `suspended`, `frozen`, or `deactivated` state presents additional security considerations.

If a relying party ignores lifecycle state, it may incorrectly accept a DID that should no longer be trusted for active decisions.

Accordingly:

- `deactivated` DIDs MUST NOT be treated as active
- `frozen` DIDs MUST NOT be treated as fully active for control-sensitive purposes
- `suspended` DIDs SHOULD be treated cautiously according to governance and application policy.

## 13.10 Historical Data and Caching Risks

Historical DID Documents, cached discovery data, or stale lifecycle metadata MAY cause incorrect trust decisions if treated as current without validation.

Implementations SHOULD:

- distinguish clearly between historical and current representations
- minimize long-lived stale caching
- re-evaluate authority after material domain events
- avoid silently trusting outdated control relationships.

## 13.11 Administrative Override Risks

Governance-authorized override or administrative action may be necessary in some environments, but it introduces additional security risks, including:

- abuse of privileged authority
- opaque decision-making
- inconsistent treatment across cases
- excessive centralization of effective control.

Where such mechanisms exist, they SHOULD be:

- narrowly scoped
- policy-bound
- auditable
- reviewable
- exceptional rather than routine.

## 13.12 Security Minimum Requirement

A conforming deployment of `did:cndomain` SHOULD implement security controls that ensure, at a minimum:

1. strict DID syntax and normalization validation
2. secure endpoint discovery and HTTPS retrieval
3. exact DID-to-document binding validation
4. protection against stale-key control after domain authority changes
5. replay-resistant proof handling for control-sensitive operations
6. lifecycle-aware trust interpretation.

## 13.13 DID Method Security Checklist

For DID method review, implementations and deployments SHOULD evaluate at least the following security categories:

- eavesdropping on DID resolution traffic, mitigated by HTTPS retrieval and privacy-aware resolver deployment
- message insertion, deletion, or modification, mitigated by TLS validation, exact DID-to-document binding, and rejection of insecure redirects
- replay of control proofs, mitigated by nonce-bound, time-bound, operation-bound, DID-bound, and domain-bound proof requirements
- man-in-the-middle attacks on discovery or retrieval, mitigated by secure DNS practices where available, HTTPS validation, and consistency checks between the requested DID and retrieved DID Document
- denial-of-service or amplification risks in DNS and HTTPS resolution, mitigated by resolver rate limits, redirect limits, cache discipline, and bounded retrieval behavior
- stale authority after domain transfer, expiry, or dispute, mitigated by lifecycle-aware resolution and dual-control requirements.

Residual risk remains where domain registration systems, registrar accounts, DNS hosting, HTTPS publication infrastructure, or governance processes are compromised. Deployments SHOULD document their operational assumptions and incident response procedures for those dependencies.

# 14. Privacy Considerations

This section defines the principal privacy considerations for the `did:cndomain` method.

As required by DID Core, a DID method specification should explain how the method may affect privacy and what measures can reduce unnecessary exposure. Because `did:cndomain` is anchored to publicly meaningful domain names and may also support object-level identifiers, privacy risks may arise from both identifier design and publication behavior.

## 14.1 Privacy Model Overview

A `did:cndomain` identifier is domain-centric.

This means that privacy characteristics of the method are influenced by:

1. the public visibility of the domain anchor
2. the stability of the identifier over time
3. the semantics of object-level naming
4. the amount of information published in the DID Document
5. the discoverability of DID deployment through DNS or HTTPS publication patterns.

In many cases, the domain anchor itself may already reveal organizational, operational, or brand-related information.

## 14.2 Correlation Risk

A stable DID anchored to a publicly recognizable domain MAY enable correlation across contexts.

This risk is especially relevant when:

- the same DID is reused broadly across applications
- object-level identifiers encode meaningful subject structure
- public service endpoints reveal relationships among internal systems.

Implementations and deployers SHOULD consider whether the same domain-level or object-level DID should be reused universally, or whether more context-limited publication and usage patterns are appropriate.

## 14.3 Structural Exposure Through Object-Level Naming

Object-level identifiers may expose internal structure, operational roles, or resource topology.

Examples of potentially revealing names include:

- `resource:gpu0`
- `service:billing`
- `service:admin`

These examples are illustrative and syntactically valid under this method. They are not prohibited by default.

In lower-sensitivity deployments, descriptive names may be operationally appropriate. Where exposure risk is higher, descriptive names MAY reveal:

- internal architecture
- resource inventories
- sensitive function names
- organizational structure.

Implementations SHOULD avoid unnecessarily descriptive object-local names where such exposure creates material risk.

For sensitive deployments, producers SHOULD prefer non-descriptive high-entropy object-local names. Where deterministic derivation is required, salted or keyed derivation SHOULD be preferred over predictable cleartext naming or unsalted hash-only naming.

## 14.4 Publication Endpoint Exposure

DNS discovery records and well-known HTTPS publication paths may reveal that a domain participates in a DID-based identity system.

This disclosure may be acceptable in many deployments, but it can still reveal:

- that a particular domain operates DID infrastructure
- the possible existence of object-level identifiers
- certain aspects of trust architecture or governance deployment.

Deployers SHOULD assess whether dedicated trust endpoints, reduced public metadata, or more selective publication strategies are appropriate for their context.

## 14.5 Metadata Exposure

A DID Document may expose more information than is strictly necessary.

Potentially sensitive information may include:

- detailed controller relationships
- extensive service endpoint lists
- operational topology
- transparency or audit service locations
- application-specific metadata embedded directly in the DID Document.

Implementations SHOULD apply data minimization. Only information necessary for intended interoperability and trust functions SHOULD be published publicly.

## 14.6 Lifecycle and Governance Disclosure

Lifecycle metadata such as `suspended`, `frozen`, or `deactivated` may itself reveal operational or governance-sensitive facts.

For example, a public `frozen` state may imply:

- dispute
- judicial intervention
- regulatory action
- compliance concern
- security incident.

Where lifecycle metadata is disclosed, implementations SHOULD ensure that the disclosure is proportionate, policy-consistent, and no more revealing than necessary for relying parties to make correct trust decisions.

## 14.7 Audit and Log Privacy

Auditability is important for this method, but audit logs may themselves contain sensitive information.

Logged data MAY reveal:

- who attempted a control-sensitive action
- when a domain control challenge occurred
- what operation was attempted
- whether a dispute or override took place.

Implementations SHOULD define retention, access control, and minimization policies for audit data.

Transparency mechanisms, where used, SHOULD avoid publishing unnecessary personal or operational detail.

## 14.8 Resolver and Query Privacy

Resolvers may observe DID queries and resolution patterns.

This MAY reveal:

- which domains are being checked
- which object-level identifiers are of interest
- the timing and frequency of trust-related operations.

Where resolver privacy is important, deployers and implementers SHOULD consider:

- minimizing unnecessary logging
- access controls for resolver logs
- policy-limited retention
- privacy-aware deployment architectures.

## 14.9 Historical Representation Privacy

Historical DID Documents and retained metadata may expose prior controllers, services, or relationships that are no longer current.

While historical records may be valuable for audit or transparency, they MAY also reveal past internal structure or control history.

Implementations SHOULD define whether historical representations are retained, how they are accessed, and how long they are made available.

## 14.10 Data Minimization Principle

A conforming deployment of `did:cndomain` SHOULD apply the principle of minimum necessary disclosure.

In particular:

1. object-level names SHOULD avoid exposing unnecessary sensitive semantics
2. DID Documents SHOULD publish only information necessary for intended trust and interoperability functions
3. publicly exposed lifecycle, governance, and audit information SHOULD be limited to what is necessary for correct interpretation
4. internal or sensitive operational metadata SHOULD NOT be exposed merely for convenience.

## 14.11 Privacy Minimum Requirement

A conforming deployment of `did:cndomain` SHOULD, at a minimum:

1. assess whether domain-level or object-level identifiers create correlation risk
2. minimize unnecessary semantics in public object-level names
3. minimize unnecessary public DID Document metadata
4. control access to audit and resolver logs
5. avoid exposing more governance-sensitive state information than required for trustworthy interpretation.

# 15. Implementation Considerations

This section provides implementation considerations for `did:cndomain`.

The purpose of this section is not to redefine the method semantics, but to clarify how the method may be implemented in a conforming and operationally coherent manner.

A practical implementation of `did:cndomain` typically involves multiple functional roles, including:

- identifier publisher
- DID Document publication service
- resolver
- control proof verifier
- lifecycle and governance manager.

An implementation MAY combine these functions into a single system or distribute them across multiple systems, provided that the overall behavior remains consistent with this specification and any applicable profile.

## 15.1 Minimum Functional Components

A minimally functional `did:cndomain` deployment SHOULD include all of the following:

1. a component capable of constructing valid `did:cndomain` identifiers
2. a component capable of publishing DID Documents over HTTPS
3. a mechanism for DNS-based discovery or profile-defined fallback retrieval
4. a resolver capable of retrieving and validating DID Documents
5. a mechanism for processing control-sensitive lifecycle operations
6. a mechanism for representing or returning lifecycle state information.

A deployment that lacks one or more of the above elements MAY still be useful for experimentation, but SHOULD NOT claim full operational support for the method.

## 15.2 Publisher Responsibilities

A publisher of `did:cndomain` DID Documents SHOULD ensure that:

1. each DID Document is published at an HTTPS endpoint consistent with this specification or an applicable profile
2. the DID Document `id` exactly matches the DID being published
3. domain-level and object-level identifiers are published in a manner consistent with the authority model of this method
4. changes to DID Documents are controlled and auditable
5. publication behavior remains consistent with lifecycle state and governance policy.

A publisher SHOULD NOT continue to publish a DID Document as if it were fully active once current legitimate control of the corresponding `.cn` domain can no longer be demonstrated.

## 15.3 Resolver Responsibilities

A `did:cndomain` resolver SHOULD ensure that it can:

1. parse and validate the DID syntax
2. normalize the `.cn` domain anchor correctly
3. apply DNS discovery and fallback rules consistently
4. retrieve DID Documents over HTTPS securely
5. validate exact DID-to-document binding
6. distinguish current, restricted, and historical states where applicable
7. return structured resolution results and structured failure information.

A resolver SHOULD treat lifecycle metadata, domain control changes, and governance-sensitive restrictions as meaningful inputs to trust interpretation, rather than as optional informational hints.

Companion resolution/discovery profiles MAY define stricter procedures, but implementations SHOULD remain conformant to the baseline rules of Section 8.

## 15.4 Control Proof Verification

An implementation that supports create, update, delegation, suspension, freezing, deactivation, or reactivation MUST implement control proof verification consistent with this specification.

At a minimum, such an implementation MUST ensure that:

1. key control proof is verified correctly
2. domain control proof is verified correctly
3. proof freshness is enforced
4. replay protection is enforced
5. proof context is bound to the DID, domain, operation, and challenge context.

A system SHOULD NOT authorize a control-sensitive operation solely on the basis of historical publication or stale proof artifacts.

Companion control-proof profiles MAY define stricter proof policy levels and additional governance-triggered checks.

## 15.5 Lifecycle Management

An implementation SHOULD include a mechanism for lifecycle management.

Such a mechanism SHOULD be capable of:

1. recording current lifecycle state
2. applying state transitions consistently with this specification
3. linking domain events to DID lifecycle review where appropriate
4. exposing state information to resolvers, relying parties, or management components in a consistent manner.

An implementation MAY realize lifecycle management through:

- embedded state metadata
- external state services
- governance-backed management systems
- signed event records
- another profile-defined mechanism.

Whatever mechanism is used, it SHOULD preserve clear distinction between:

1. DID subject data
2. DID document metadata
3. DID resolution metadata.

## 15.6 Object-Level Identifier Management

An implementation supporting object-level identifiers SHOULD define how object identifiers are created, named, and managed beneath a parent domain.

In particular, such an implementation SHOULD define:

1. allowed object classifications
2. naming rules for object-local names
3. whether delegation is allowed
4. how subordinate DID lifecycle is affected by parent domain lifecycle
5. whether object-level publication is automatic, managed, or manually approved.

An implementation SHOULD avoid ambiguous or unstable naming practices for object-level identifiers.

## 15.7 Caching and Freshness Handling

Implementations that cache discovery results or DID Documents SHOULD do so carefully.

A caching strategy SHOULD take into account:

1. DNS TTL values
2. HTTPS cache directives where appropriate
3. risk of stale authority after domain transfer, expiry, or freeze
4. lifecycle-sensitive restrictions
5. governance requirements for freshness.

An implementation SHOULD avoid treating cached resolution data as authoritative when there is reason to suspect a material change in domain control or lifecycle state.

## 15.8 Logging and Audit

An implementation SHOULD log high-impact events relevant to authority and lifecycle interpretation.

Such events MAY include:

- creation
- update
- key rotation
- controller change
- delegated authority change
- suspension
- freezing
- deactivation
- reactivation
- proof verification failure
- governance-authorized override.

Logs SHOULD be:

1. sufficiently detailed for audit purposes
2. integrity-protected where appropriate
3. access-controlled according to policy
4. retained in accordance with governance, privacy, and operational requirements.

A deployment MAY introduce an external tamper-evident audit layer, including a blockchain-based integrity log, for DID Document hash anchoring, lifecycle event traceability, and status proof publication. Such a layer is additive and MUST NOT replace the baseline HTTPS publication and resolution requirements defined by this specification.

## 15.9 Profile Compatibility

An implementation of `did:cndomain` SHOULD clearly state which profiles it supports.

This MAY include support for:

- a particular resolution and discovery profile
- a particular control proof profile
- a particular governance profile
- a restricted object-type profile
- a deployment-specific conformance subset.

An implementation SHOULD NOT claim support for the method in a way that obscures major unsupported features or materially incompatible profile behavior.

Further governance-specific procedures MAY be defined in an applicable governance profile.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics, while the applicable profile governs detailed operational behavior, unless otherwise explicitly stated.

## 15.10 Implementation Minimum Requirement

A conforming operational implementation of `did:cndomain` SHOULD, at a minimum:

1. publish or retrieve DID Documents consistently with this specification
2. enforce exact DID binding
3. apply domain-anchored authority rules
4. enforce lifecycle-aware interpretation
5. support structured handling of discovery, control, and state information.

# 16. Examples

The examples in this section are non-normative and are provided solely to illustrate possible uses of this method.

Any lowercase keywords in this section are descriptive only and do not introduce additional normative requirements.

## 16.1 Domain-Level DID

Example domain-level DID:

```text
did:cndomain:example.cn
```

This identifier represents the domain-level subject anchored to `example.cn`.

## 16.2 Subdomain DID

Example subdomain DID:

```text
did:cndomain:sub.example.cn
```

This identifier represents a subject anchored to the subdomain `sub.example.cn`.

## 16.3 Object-Level Service DID

Example object-level service DID:

```text
did:cndomain:example.cn:service:web
```

This identifier represents a service object named `web` under the authority of `example.cn`.

## 16.4 Object-Level Resource DID

Example object-level resource DID:

```text
did:cndomain:example.cn:resource:gpu0
```

This identifier represents a resource object named `gpu0` under the authority of `example.cn`.

## 16.5 Domain-Level DID Document Example

Example DID Document for a domain-level DID:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suite/jws-2020/v1",
    "https://identity.foundation/.well-known/did-configuration/v1"
  ],
  "id": "did:cndomain:example.cn",
  "controller": "did:cndomain:example.cn",
  "verificationMethod": [
    {
      "id": "did:cndomain:example.cn#key-1",
      "type": "JsonWebKey2020",
      "controller": "did:cndomain:example.cn",
      "publicKeyJwk": {
        "kty": "OKP",
        "crv": "Ed25519",
        "x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ"
      }
    }
  ],
  "authentication": [
    "did:cndomain:example.cn#key-1"
  ],
  "assertionMethod": [
    "did:cndomain:example.cn#key-1"
  ],
  "service": [
    {
      "id": "did:cndomain:example.cn#did-document",
      "type": "LinkedDomains",
      "serviceEndpoint": "https://example.cn"
    }
  ]
}
```

## 16.6 Object-Level DID Document Example

Example DID Document for an object-level DID:

```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:cndomain:example.cn:resource:gpu0",
  "controller": "did:cndomain:example.cn"
}
```

## 16.7 DNS Discovery Example

Example DNS base discovery record:

```text
_did.example.cn. IN URI 10 1 "https://trust.example.cn/.well-known/did/"
```

This indicates that a resolver may construct the final DID Document retrieval URL from the discovered base endpoint according to Section 8.4.

## 16.8 Default HTTPS Fallback Example

If no DNS discovery record is present, a resolver may use a default fallback rule.

Example domain-level retrieval URL:

```text
https://example.cn/.well-known/did/cndomain.json
```

Example object-level retrieval URL:

```text
https://example.cn/.well-known/did/cndomain/resource/gpu0.json
```

## 16.9 Resolution Result Example

Example successful resolution result:

```json
{
  "didDocument": {
    "@context": "https://www.w3.org/ns/did/v1",
    "id": "did:cndomain:example.cn"
  },
  "didDocumentMetadata": {
    "cndomainLifecycleState": "active",
    "updated": "2026-04-08T10:00:00Z"
  },
  "didResolutionMetadata": {
    "contentType": "application/did+ld+json",
    "discoverySource": "dns-uri",
    "retrievalUrl": "https://trust.example.cn/.well-known/did/cndomain.json"
  }
}
```

## 16.10 Suspended-State Example

Example resolution metadata for a suspended DID:

```json
{
  "didDocumentMetadata": {
    "cndomainLifecycleState": "suspended"
  },
  "didResolutionMetadata": {
    "warning": "The DID is currently suspended."
  }
}
```

This indicates that the DID remains identifiable, but normal trust use may be restricted.

## 16.11 Frozen-State Example

Example resolution metadata for a frozen DID:

```json
{
  "didDocumentMetadata": {
    "cndomainLifecycleState": "frozen"
  },
  "didResolutionMetadata": {
    "warning": "The DID is currently frozen and subject to governance restriction."
  }
}
```

This indicates that stronger governance restrictions apply.

## 16.12 Deactivated-State Example

Example deactivation result:

```json
{
  "didDocument": null,
  "didDocumentMetadata": {
    "deactivated": true,
    "cndomainLifecycleState": "deactivated"
  }
}
```

This indicates that the DID is no longer active for current trust decisions.

## 16.13 Control Proof Example

Example conceptual control proof context:

```json
{
  "did": "did:cndomain:example.cn",
  "domain": "example.cn",
  "operation": "update",
  "nonce": "7f31ab92",
  "issuedAt": "2026-04-08T10:30:00Z",
  "expiresAt": "2026-04-08T10:35:00Z"
}
```

A conforming implementation would require both:

1. proof of control of an authorized DID update key
2. proof of current legitimate control over `example.cn`.

## 16.14 Controller Delegation Example

This example illustrates a case in which an object-level DID remains anchored to a parent `.cn` domain, while a delegated controller is explicitly identified for limited operational purposes.

Example object-level DID:

```text
did:cndomain:example.cn:service:web
```

Example DID Document with delegated controller:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suite/jws-2020/v1"
  ],
  "id": "did:cndomain:example.cn:service:web",
  "controller": [
    "did:cndomain:example.cn",
    "did:example:ops-team-01"
  ],
  "verificationMethod": [
    {
      "id": "did:cndomain:example.cn:service:web#ops-key-1",
      "type": "JsonWebKey2020",
      "controller": "did:example:ops-team-01",
      "publicKeyJwk": {
        "kty": "OKP",
        "crv": "Ed25519",
        "x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ"
      }
    }
  ],
  "authentication": [
    "did:cndomain:example.cn:service:web#ops-key-1"
  ]
}
```

Interpretation:

1. the parent domain authority remains `did:cndomain:example.cn`
2. the delegated controller `did:example:ops-team-01` is explicitly identified for limited control purposes
3. such delegation remains subordinate to the current legitimate control of `example.cn`
4. loss of current legitimate control over `example.cn` would normally trigger re-evaluation of the delegated authority
5. delegated control is not interpreted as surviving loss of parent domain authority unless an applicable governance profile explicitly defines such continuity.

## 16.15 Non-Conforming Identifier Examples

The following are examples of identifiers that are not valid under this specification:

```text
did:cndomain:example.com
did:cndomain:Example.cn
did:cndomain:example.cn:service
did:cndomain:example.cn:service:web:extra
did:cndomain:
```

These examples are invalid because:

1. the domain is outside the `.cn` namespace
2. the domain is not normalized
3. the object-level structure is incomplete
4. the method-specific identifier contains unsupported extra segments
5. the method-specific identifier is empty.

# 17. Conformance

This section defines conformance requirements for implementations of the `did:cndomain` method.

The key words “MUST”, “MUST NOT”, “SHOULD”, “SHOULD NOT”, and “MAY” in this specification are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

An implementation claiming conformance with `did:cndomain` MUST conform to the normative requirements applicable to the functions it performs.

## 17.1 General Conformance

An implementation claiming conformance with the `did:cndomain` method MUST:

1. construct, parse, or interpret identifiers according to Section 5
2. interpret method-specific identifier semantics according to Section 6
3. apply lifecycle operations consistently with Section 7
4. apply DID resolution behavior consistently with Section 8
5. apply DID Document validation and interpretation consistently with Section 9
6. respect domain control binding as defined in Section 10
7. interpret lifecycle states consistently with Section 11
8. avoid behavior that materially contradicts the governance, security, and privacy requirements of Sections 12, 13, and 14.

An implementation MUST NOT claim conformance if it knowingly ignores the domain-anchored authority model defined by this specification.

## 17.2 Resolver Conformance

An implementation claiming conformance as a `did:cndomain` resolver MUST:

1. accept valid `did:cndomain` identifiers as input
2. reject identifiers that do not conform to the syntax rules of this specification
3. normalize and interpret the `.cn` domain anchor correctly
4. perform endpoint discovery and/or fallback behavior consistently with this specification or an applicable profile
5. retrieve DID Documents over HTTPS where retrieval is performed
6. validate exact DID-to-document binding
7. return resolution results or structured failure information in a consistent manner
8. account for lifecycle state where such information is available.

A resolver MUST NOT present a `deactivated` DID as if it were fully active.

Companion resolution/discovery profiles MAY define stricter procedures, but baseline resolver conformance is defined by this specification.

## 17.3 Publisher Conformance

An implementation claiming conformance as a DID Document publisher MUST:

1. publish a DID Document whose `id` exactly matches the DID being represented
2. publish DID Documents only through endpoints consistent with this specification or an applicable profile
3. ensure that domain-level and object-level publication is consistent with the authority model of this method
4. avoid representing a DID as fully active when doing so would conflict with known lifecycle restrictions or loss of domain control.

A publisher SHOULD ensure that publication changes are auditable.

## 17.4 Control-Sensitive Management Conformance

An implementation claiming conformance for create, update, delegation, suspension, freezing, deactivation, or reactivation functions MUST:

1. apply the lifecycle rules defined by this specification
2. require authorization appropriate to the requested operation
3. apply domain control binding consistently
4. reject control-sensitive operations when current legitimate control of the corresponding `.cn` domain cannot be established
5. reject stale, invalid, or unauthorized proof material
6. apply state transitions consistently with method semantics and any applicable governance profile.

An implementation performing such control-sensitive operations SHOULD support a control proof procedure that is compatible with the baseline requirements of this specification.

If a companion control-proof profile is adopted, the implementation MUST apply that profile consistently for the claimed scope.

## 17.5 Object-Level Identifier Conformance

An implementation supporting object-level DIDs MUST:

1. interpret object-level DIDs as subordinate to the parent `.cn` domain authority unless a valid delegation model explicitly defines otherwise
2. apply object-type and object-name rules consistently
3. re-evaluate object-level authority when parent domain control materially changes
4. avoid treating object-level DIDs as authority-independent merely because they are separately resolvable.

## 17.6 Lifecycle Conformance

An implementation claiming lifecycle-aware conformance MUST:

1. distinguish at least the method-level states `active` and `deactivated`
2. interpret `suspended` and `frozen` consistently if those states are supported
3. avoid treating restricted states as equivalent to active state
4. ensure that lifecycle state affects resolution and/or management behavior in a consistent manner.

An implementation MAY support additional sub-states, provided that they do not contradict the method-level lifecycle semantics of this specification.

## 17.7 Profile Conformance

An implementation MAY claim support for one or more `did:cndomain` profiles, including:

- resolution and discovery profile
- control proof profile
- governance profile
- deployment-specific restriction profile.

If an implementation claims support for an applicable `did:cndomain` profile, it MUST implement the normative requirements of that profile consistently and MUST clearly identify any unsupported optional features.

Claiming no companion profile support does not prevent baseline conformance to this method specification.

An implementation SHOULD clearly disclose which profiles, options, and object classifications it supports.

Where a conflict arises between this section and an applicable profile, this section governs the method-level semantics, while the applicable profile governs detailed operational behavior, unless otherwise explicitly stated.

## 17.8 Partial Implementations

A partial implementation MAY conform to a subset of `did:cndomain` functions, provided that:

1. the subset is clearly identified
2. unsupported functions are clearly disclosed
3. the implementation does not claim full method conformance if it lacks required behavior for the claimed role.

For example:

- a read-only resolver MAY conform as a resolver implementation without supporting update operations
- a publisher MAY conform as a publication implementation without operating a public resolver
- a governance-aware manager MAY conform as a lifecycle management component without performing public DID resolution directly.

## 17.9 Non-Conforming Behavior

The following are examples of non-conforming behavior:

- accepting non-`.cn` domains as valid method anchors
- silently treating malformed identifiers as valid
- returning a DID Document whose `id` does not exactly match the requested DID
- authorizing updates solely on the basis of stale historical key possession where current legitimate control is no longer established
- ignoring known deactivated state when making active trust decisions
- treating object-level DIDs as independent of parent domain authority without valid delegation semantics.

An implementation engaging in such behavior MUST NOT claim conformance with this specification.

## 17.10 Conformance Minimum Requirement

At a minimum, an implementation claiming meaningful conformance with `did:cndomain` SHOULD demonstrate that it can do all of the following for its claimed role:

1. process valid identifiers correctly
2. reject invalid identifiers correctly
3. preserve exact DID binding
4. respect domain-anchored control semantics
5. interpret lifecycle state in a non-misleading manner.
