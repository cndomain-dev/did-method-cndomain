# did:cndomain Agent Identity Profile

**Status:** Working Draft

## Abstract

This profile defines interoperable use of the `agent` object type in `did:cndomain`. It defines an agent DID as an identifier for a logical agent subject under a `.cn` domain authority, and specifies its relationship to controllers, verification methods, service endpoints, Verifiable Credential subjects, naming continuity, and lifecycle events.

This profile is optional. It does not change the baseline `did:cndomain` syntax or require implementations that do not claim support for this profile to process `agent` identifiers.

## 1. Conformance

An implementation conforms to this profile only if it claims support for the `did:cndomain Agent Identity Profile` and identifies the roles it supports: creator, publisher, resolver, controller, or relying party. A requirement addressed to one of these roles applies only when an implementation performs that role. An implementation claiming only resolver support is not required to create DIDs or maintain allocation records unless it also performs those roles.

An implementation claiming support for this profile:

1. MUST accept `agent` as an object type when processing profile-conforming identifiers
2. MUST apply the naming and lifecycle rules in this profile to each `agent` DID it creates, publishes, or manages
3. MUST remain consistent with the `did:cndomain Method Specification`.

This profile does not make `agent` a baseline object type. A baseline implementation that does not support this profile is not required to assign `agent` any semantics beyond the baseline syntax and object-type handling rules.

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

## 2. Terms

An **agent subject** is a logical software agent identified under the authority of a `.cn` domain. It can persist across process restarts, deployment changes, service endpoint changes, and model updates when the agent registration authority maintains the same logical identity.

An **agent DID** is an object-level `did:cndomain` identifier whose object type is `agent`.

A **service endpoint** is an address or other endpoint through which an agent can be contacted or used. A service endpoint is not, by itself, an agent subject.

An **agent registration authority** is an entity that has demonstrated current legitimate control of the corresponding `.cn` domain in accordance with the Method Specification, or an entity explicitly delegated by the entity with current legitimate control of that domain under an applicable governance procedure to approve agent subject registrations. A `controller` declaration in a DID Document alone MUST NOT establish an agent registration authority. A delegated registration authority MUST act within its documented scope and remains subordinate to current legitimate domain control.

## 3. Identifier and Subject Semantics

An agent DID MUST use the object-level syntax defined by the Method Specification:

```text
did:cndomain:<domain>:agent:<object-name>
```

For example:

```text
did:cndomain:a.cn:agent:agent-001
```

The identifier MUST identify the agent subject and MUST NOT be defined solely as an API address, deployment instance, model version, or service endpoint. A deployment MAY assign separate DIDs to distinct agent subjects even when they use the same software, model, or endpoint.

An agent DID MUST be interpreted under the authority of its parent domain DID. It MUST NOT be treated as authority-independent solely because it resolves separately or has its own verification methods.

The profile does not define a universal taxonomy for agent purpose, capability, model, or implementation. Such claims, when needed, belong in application data, credentials, or another applicable profile.

## 4. Relationship to Service Identifiers and Endpoints

An `agent` DID identifies an agent subject. A `service` DID identifies a service subject. An endpoint used by an agent is service information and MUST NOT be substituted for the agent DID.

An agent DID Document MAY contain `service` entries for endpoints through which the agent is available. Each such entry MUST describe an endpoint associated with the identified agent subject and MUST NOT be interpreted as a separate agent identity.

An endpoint, deployment, or model version change alone MUST NOT require a new agent DID. A deployment MAY represent separately governed services with their own `service` DIDs and associate them with an agent through application-defined data or a profile-defined relationship. This profile does not define a new DID Document property for that association.

## 5. Control and Delegation

The parent domain DID MUST remain the authority anchor for an agent DID. The entity with current legitimate control of the parent `.cn` domain MUST retain ultimate authority to authorize creation, recovery, delegation, suspension, and deactivation of the agent DID, subject to the Method Specification. Its authority MUST be established through the method's applicable domain control proof; a DID Document `controller` declaration alone is insufficient.

The entity with current legitimate control of the parent `.cn` domain MAY delegate limited operational authority for an agent DID only under an applicable governance procedure. That procedure MUST define how a delegation is authorized and recorded, identify the delegate and permitted operations, specify how the delegation is revoked, and define how any controller declaration in the DID Document relates to the delegated permissions. A `controller` declaration alone MUST NOT be interpreted as specifying the scope or revocation rules of a delegation. A delegation MUST remain subordinate to current legitimate control of the parent `.cn` domain.

A delegated operator MUST NOT be treated as having authority beyond the operations explicitly granted. Delegation MUST NOT, by itself, establish that the agent is safe, trustworthy, or authorized for a particular action.

## 6. DID Document Requirements

An agent DID Document MUST:

1. use the exact agent DID as its `id`
2. identify the parent domain DID as a controller
3. express any delegated controller in a manner consistent with Section 5 and the Method Specification
4. include an `authentication` relationship when the agent DID is used to authenticate as the agent subject.

An agent DID Document MAY include verification methods and `assertionMethod` relationships. A verification method MUST only be used for purposes supported by its declared verification relationship and applicable application rules.

An agent DID Document MAY include `service` entries as defined in Section 4. It MAY include application-defined data that associates the agent with credentials or other records, but such data MUST NOT be treated as authoritative unless the applicable data model or profile defines its meaning.

This profile does not require a DID Document to enumerate Verifiable Credentials that refer to the agent. A Verifiable Credential MAY identify the agent as its subject by using the agent DID as the credential subject's `id`, consistent with the applicable Verifiable Credentials data model. Credential validity, issuer authority, status, and claim interpretation remain governed by that data model and the credential's rules.

## 7. Creation, Update, and Key Rotation

Creation of an agent DID MUST satisfy the creation and control-proof requirements of the Method Specification. Before creation, an agent registration authority MUST approve the agent subject under an applicable governance procedure. That procedure MUST specify the admissible evidence, the decision authority, and how the approval is recorded. The approval MUST bind the agent subject to the requested DID and parent domain. The creator MUST allocate an object-local name that has not previously been assigned to another agent subject under that parent domain. Method-level key and domain control proofs establish control of the relevant key and domain; by themselves, they do not establish that an agent subject has been approved under this profile.

The agent registration authority MUST maintain a durable, auditable allocation record for agent object-local names under each parent domain. The record MUST preserve each allocated name and its associated DID, agent subject registration reference, approval decision, approving authority, and lifecycle history, including after deactivation or domain transfer. A publisher MUST verify from this record that a name has never been allocated before creating an agent DID with that name. If the record is unavailable or does not establish that the name is unused, the publisher MUST treat the name as reserved and MUST NOT allocate it.

Updates to an agent DID Document MUST satisfy the update and domain control requirements of the Method Specification and MUST preserve the DID `id`. Replacing the agent subject requires creating a new agent DID and separately retiring or deactivating the prior DID as described in Section 8.

Key rotation MUST be performed as an authorized DID Document update. Key rotation alone MUST NOT change the agent DID. An implementation SHOULD record update information needed to audit the rotation, subject to applicable privacy and retention rules.

## 8. Identifier Stability and Non-Reassignment

An agent DID MUST remain stable while it refers to the same logical agent subject and its continuity is maintained by the agent registration authority.

An agent object-local name MUST NOT be reassigned to a different agent subject under the same parent domain. This prohibition continues after suspension, freezing, deactivation, domain transfer, or removal of the agent from service. A resolver or publisher MUST NOT present a later, different agent as the historical subject of the same agent DID.

During a domain transfer, the continuity record for agent names MUST be made available to the assuming domain controller through the applicable governance process. If the record cannot be transferred or verified, all names whose prior allocation cannot be ruled out MUST remain reserved.

The agent registration authority MUST use the agent subject registration reference in the allocation record when deciding whether an agent DID continues to identify the same logical agent subject. It MUST record each continuity decision, including the decision authority, the registration references considered, and the rationale. Changes to implementation, model, keys, hosting, deployment, endpoint, or operational personnel alone MUST NOT be treated as replacement. When the authority retires a registered agent subject and approves a distinct replacement subject, the replacement MUST receive a new agent DID. The prior DID MUST be updated to `suspended`, `frozen`, or `deactivated`, as appropriate under the Method Specification, and MUST NOT be reused for the replacement.

When a proposed change to the registered subject or its recognized identity context could affect whether the same registered agent subject continues, the agent registration authority MUST make and record a continuity decision under its governance procedure before changing the DID's subject representation or creating a replacement DID. Routine authorized DID Document updates, including key rotation and service endpoint changes that do not raise an identity-continuity question, remain subject to Section 7 and the Method Specification and do not require a separate continuity decision. This profile does not prescribe a universal agent taxonomy or require a new DID for a particular model or software change when the authority records continuity of the same registered subject.

## 9. Lifecycle and Parent Domain Changes

Agent DIDs MUST follow the creation, update, suspension, freezing, deactivation, and reactivation requirements of the Method Specification. This profile does not create separate lifecycle states.

When a material change in control of the parent domain is detected, implementations managing agent DIDs under that domain MUST trigger a continuity review before treating those agent DIDs as fully active under the new domain control. During review, implementations MUST apply the `suspended` or `frozen` state, as appropriate under the Method Specification, so that the agent DIDs are not presented as having uninterrupted active authority without review.

The continuity review MUST determine, for each affected agent DID, whether:

1. the same logical agent subject continues under an explicitly recorded continuity decision
2. the DID MUST remain `suspended` or `frozen` pending further governance action
3. the DID MUST be deactivated because continuity is not recognized or cannot be established.

A new domain controller MUST NOT reassign an existing agent DID to a different agent subject. If continuity is not recognized, the former agent DID MUST remain `suspended`, `frozen`, or `deactivated` as determined under the Method Specification, and a new agent subject MUST use a new object-local name.

Any continuation of delegated control after loss or transfer of parent domain authority MUST be handled under the continuity rules of the Method Specification and applicable governance profile. This profile does not make delegation survive a domain transfer by default.

## 10. Privacy and Security Considerations

Stable agent DIDs can enable correlation across credentials, services, and audit records. Deployments SHOULD avoid embedding sensitive personal, organizational, model, or operational information in object-local names and SHOULD assess whether a persistent public identifier is necessary for each use.

An agent DID and its DID Document establish neither the safety of the agent nor the authorization of every operation it performs. Relying parties MUST evaluate the relevant credentials, authorization evidence, lifecycle state, domain control, and application policy for the decision at hand.

Service endpoints published in a DID Document can expose operational structure and attract unsolicited traffic. Deployments SHOULD publish only endpoints needed for the intended discovery use and SHOULD apply appropriate endpoint security and abuse controls.

## 11. Relationship to the Method Specification

This profile specializes the `agent` object type under the extension mechanism in Section 5.4 of the `did:cndomain Method Specification`. It does not change the method-specific identifier syntax, resolution rules, or baseline lifecycle semantics.

Where this profile and the Method Specification differ, the Method Specification controls method-level semantics. This profile MAY impose additional requirements for implementations that claim support for it, provided those requirements do not contradict the Method Specification.
