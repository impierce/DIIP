Decentralized Identity Interop Profile v4
==================

**Profile Status:** Draft

**Latest Draft:**
[https://FIDEScommunity.github.io/DIIP](https://FIDEScommunity.github.io/DIIP)

Editors:
~ [Eelco Klaver](https://www.linkedin.com/in/eklaver/) (Credenco)
~ [Harmen van der Kooij](https://www.linkedin.com/in/harmenvanderkooij/) (FIDES Labs)
~ [Niels Klomp](https://www.linkedin.com/in/niels-klomp/) (Sphereon)
~ [Niels van Dijk](https://www.linkedin.com/in/creativethings/) (SURF)
~ [Samuel Rinnetmäki](https://www.linkedin.com/in/samuel/) (Findynet)

Contributors and previous editors:
~ [Adam Eunson](https://www.linkedin.com/in/adameunson/) (Auvo)
~ [Jelle Millenaar](https://www.linkedin.com/in/jellefm/) (Impierce Technonologies)
~ [Maaike van Leuken](https://www.linkedin.com/in/maaike-van-leuken-0b1b7011a/) (TNO)
~ [Timo Glastra](https://www.linkedin.com/in/timoglastra/) (Animo Solutions)
~ [Thierry Thevenet](https://www.linkedin.com/in/thierrythevenet/) (Talao)

**Special Thanks:**

This profile is based on a lot of work done by the Decentralized Identity community. Given that this profile is largely based on and uses sections of the [DIF JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/), we would like to give special thanks to the editors and contributors of that profile.

Participate:
~ [GitHub repo](https://github.com/FIDEScommunity/DIIP.git)
~ [File a bug](https://github.com/FIDEScommunity/DIIP.git/issues)
~ [Commit history](https://github.com/FIDEScommunity/DIIP.git/commits/main)

------------------------------------

## Abstract

The Decentralized Identity Interop Profile, or DIIP for short, defines requirements against existing specifications to enable the interoperable issuance and presentation of [[ref: Digital Credential]]s between [[ref: Issuer]]s, [[ref: Wallet]]s, and [[ref: Verifier]]s.

| Purpose                                                                  | Specification                                                 |
| ------------------------------------------------------------------------ | ------------------------------------------------------------- |
| Credential format                                                        | W3C Verifiable Credentials Data Model ([[ref: W3C VCDM]])     |
| Signature scheme                                                         | SD-JWT as specified in [[ref: VC-JOSE-COSE]]                  |
| Signature algorithm                                                      | [[ref: ES256]]                                                |
| Identifying [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s   | [[ref: did:jwk]] and [[ref: did:web]]                         |
| Issuance protocol                                                        | OpenID for Verifiable Credentials Issuance ([[ref: OID4VCI]]) |
| Presentation protocol                                                    | OpenID for Verifiable Presentations ([[ref: OID4VP]])         |
| Revocation mechanism                                                     | [[ref: IETF Token Status List]]                               |
| Trust establishment                                                      | [[ref: OpenID Federation]]                                    |

<!-- - [[ref: SIOPv2]] -->

The [Normative References](#normative-references) section links to the versions of specifications that DIIP-compliant implementations must support.

This document is not a specification but a **profile**. It outlines existing specifications required for implementations to interoperate with each other. 
It also clarifies mandatory features for the options mentioned in the referenced specifications.

The main objective of this profile is to allow for easy adoption and use the minimum amount of functionality for a working [[ref: Digital Credential]] ecosystem.

### Status of This Document

The Decentralized Identity Interop Profile v4 is a DRAFT specification under development.

The latest published DIIP profile can be found at [https://FIDEScommunity.github.io/DIIP/latest](https://FIDEScommunity.github.io/DIIP/latest)

### Audience

The audience of this document includes organisations aiming to issue or verify [[ref: Digital Credential]]s, as well as the implementers of [[ref: Digital Credential]] solutions ([[ref: Wallet]]s and [[ref: Agent]]s). 

## Structure of this Document

The [Goals](#goals) section explains the design  of the DIIP profile.

The [Profile](#profile) section defines the requirements for compliant solutions and explains the choices.

The [References](#references) section defines the specifications and their versions.

The [Terminology](#terminology) section explains the key terms used in this profile.

## Goals

The [[ref: W3C VCDM]] specification defines a data model for [[ref: Digital Credential]]s but does not prescribe standards for transport protocol, key management, authentication, query language, etc. 

The ([[ref: OID4VCI]]) and ([[ref: OID4VP]]) protocols define the interaction between [[ref: Wallet]]s and [[ref: Agent]]s but don't specify a data model or a credential format.

This interoperability profile makes selections by combining a set of specifications. It chooses standards for credential format, signature algorithm, identifying actors, and issuance and presentation protocols. Instead of saying, *"We use [[ref: W3C VCDM]] credentials signed with [[ref: VC-JOSE-COSE]] using [[ref: ES256]] as the signature algorithm, [[ref: OID4VCI]] as the issuance protocol, and [[ref: OID4VP]] as the presentation protocol, and [[ref: OpenID Federation]] for trust establishment,"* you can just say, *"We use DIIP."*

In addition, the DIIP profile makes selections *within* the specifications. When a standard allows multiple ways of implementing something, DIIP makes one of those ways mandatory. As an implementer, you don't need to fully support all specifications to be DIIP-compliant. DIIP makes these choices to accelerate adoption and interoperability – defining the minimum required functionality.

DIIP does not exclude anything. For example, when DIIP says that compliant implementations MUST support [ref: did:jwk] as an identifier of the [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s, it doesn't say that other identifiers cannot be used. The [[ref: Wallet]]s and [[ref: Agent]]s can support other identifiers as well and still be DIIP-compliant.

Trust ecosystems can also easily extend DIIP by saying, "We use the DIIP profile *and allow `mDocs` as an additional credential format.*" They can also switch requirements by saying, "We use the DIIP profile *but use [[ref: VC-DATA-INTEGRITY]] as an embedded proof mechanism*."

The design goal for DIIP is to ensure interoperability between [[ref: Agent]]s and [[ref: Wallet]]s in cases where device binding of [[ref: Digital Credential]]s is not required and the [[ref: Wallet]] doesn't need to be trusted. Issuing, holding, and presenting certifications, diplomas, licenses, permits, etc., fit into the scope of DIIP. Using a [[ref: Wallet]] for strong customer authentication or for sharing Person Identification Data (PID) is out of DIIP's scope, and you should look into [[ref: HAIP]] instead.

### Relationship to eIDAS regulation and HAIP profile

In the context of the European eIDAS regulation ([[ref: eIDAS]]) and its Architecture and Reference Framework ([[ref: ARF]]), the DIIP profile is a profile for "regular" digital credentials, "non-qualified electronic attestations of attributes". The OpenID4VC High Assurance Interoperability Profile ([[ref: HAIP]]) is targeted for high-assurance use cases where it is important to bind the credentials to the [[ref: Holder]]'s private key (device binding). DIIP is the profile for other use cases.

The standards used in the DIIP profile are the same ones that the [[ref: ARF]] uses, but the DIIP profile makes different choices to [[ref: HAIP]] in many areas where [[ref: OID4VCI]] and [[ref: OID4VP]] provide optionality. DIIP aims to keep the selected OpenID4VCI and OpenID4VP Draft versions in sync with HAIP to lower implementation overhead.

### Future Work
DIIP describes technologies that are relatively easy to implement. DIIP makes choices within those standards, attempting to set the minimum functionality required for interoperability in the use cases in DIIP's scope.

When standards mature and more and more solutions have full support for all the optional functionality in the standards, there may no longer be a need for DIIP. The authors believe that this development will take years and that there is a need for DIIP now.

## Profile
In this section, we describe the interoperability profile.

### Credential Format
The W3C Verifiable Credential Data Model ([[ref: W3C VCDM]]) defines structure and vocabulary well suited for [[ref: Digital Credential]]s in DIIP's scope. For example, the [[ref: Open Badges 3]] credentials use [[ref: W3C VCDM]] as the data format.

[[ref: W3C VCDM]] recommends using Securing Verifiable Credentials using JOSE and COSE ([[ref: VC-JOSE-COSE]]) as an *enveloping proof* mechanism and 
Verifiable Credential Data Integrity 1.0 ([[ref: VC-DATA-INTEGRITY]]) as an *embedded proof* mechanism. Many [[ref: Agent]]s and [[ref: Wallet]]s already support `SD-JWT` as a way to encode [[ref: Digital Credential]]s. Using `SD-JWT` to secure [[ref: W3C VCDM]] [[ref: Digital Credential]]s should be relatively easy to implement, even though there are differences with the `SD-JWT-VC` mechanism required by [[ref: HAIP]].

**Requirement: DIIP-compliant implementations MUST support [Securing JSON-LD Verifiable Credentials with SD-JWT](https://www.w3.org/TR/vc-jose-cose/#secure-with-sd-jwt) as specified in ([[ref: VC-JOSE-COSE]]).**

### Signature Algorithm

There are many key types and signature methods used with JWTs. The table below lists some of the most common ones that implementations may want to support.

|Key types | Signature Method|
|----------|-----------------|		
|Ed25519   | ECDSA     		 |	
|(x25519)  |                 | 
|Secp256r1 | ES256           |			
|Secp256k1 | ES256K          |	
|RSA       | RSA256          |

However, the DIIP profile does not force everyone to support everything, but chooses one key type [[ref: Secp256r1]] and one signature method [[ref: ES256]] that all implementations must support.

**Requirement: DIIP-compliant implementations MUST support [[ref: ES256]] (`ECDSA` using [[ref: Secp256r1]] curve and `SHA-256` message digest algorithm).**

### Identifiers
DIIP prefers decentralized identifiers ([[ref: DID]]s) as identifiers. An entity identified by a [[ref: DID]] publishes a [DID Document](https://www.w3.org/TR/did-1.0/#dfn-did-documents), which can contain useful metadata about the entity, e.g., various endpoints. There are many DID Methods defined. The DIIP profile requires support for two of them: [[ref: did:jwk]] and [[ref: did:web]]. In many use cases, organizations are identified by [[ref: did:web]], and the natural persons are identified by [[ref: did:jwk]].

**Requirement: DIIP-compliant implementations MUST support [[ref: did:jwk]] and [[ref: did:web]] as the identifiers of the [[ref: Issuer]]s, [[ref: Holder]]s, and [[ref: Verifier]]s**

**Note: A near-future version of DIIP will probably require support for [[ref: did:webvh]] instead of [[ref: did:web]].**

### Trust Establishment
Signatures in [[ref: Digital Credential]]s can be used to verify that the content of a credential has not been tampered with. But anyone can sign a credential and put anything in the issuer field. [[ref: Digital Credential]] ecosystems require that there is a way for a [[ref: Verifier]] to check who the [[ref: Issuer]] or a [[ref: Digital Credential]] is. Equally, a user might want to be informed about the trustworthiness of a [[ref: Verifier]] before choosing to release credentials.

DIIP uses [[ref: OpenID Federation]] as the trust infrastructure protocol. [[ref: Issuer]]s and [[ref: Verifier]]s publish their own Entity Configurations, which include pointers to Trust Anchors. These Trust Anchors are trusted third parties that publish Entity Statements that allow for verification of the identity and the roles of the organizations. The [[ref: OIDF Wallet Architectures]] specification specifies how to use [[ref: OpenID Federation]] with Wallets.

**Requirement: DIIP-compliant [[ref: Issuer]] [[ref: Agent]]s MUST support publishing the [[ref: Issuer]]'s Entity Configurations as specified in [[ref: OIDF Wallet Architectures]].**

(Simplified explanation: sign the [[ref: OID4VCI]] issuer metadata as a JWT and publish it in the `.well-known` path.)

**Requirement: DIIP-compliant [[ref: Verifier]] [[ref: Agent]]s MUST support publishing the [[ref: Verifier]]'s Entity Configurations as specified in [[ref: OIDF Wallet Architectures]].**

 (Simplified explanation: sign the [[ref: OID4VP]] verifier metadata as a JWT and publish it in the `.well-known` path.)

**Requirement: If a [[ref: Digital Credential]] contains a [termsOfUse](https://www.w3.org/TR/vc-data-model-2.0/#terms-of-use) object with an attribute `federations`, a DIIP-compliant Wallet MUST warn the user before sharing [[ref: Digital Credential]]s or Verifiable Presentations with a [[ref: Verifier]] for which a trust chain cannot be resolved using the Trust Anchor in the value of the `federations` attribute.**

### Issuance
The issuance of [[ref: Digital Credential]]s from the [[ref: Issuer]] to the [[ref: Holder]]'s [[ref: Wallet]] is done along the [[ref: OID4VCI]] specification. Other protocols exist, but [[ref: OID4VCI]] is very broadly supported and also required by [[ref: HAIP]].

#### OID4VCI
OpenID for Verifiable Credential Issuance ([[ref: OID4VCI]]) defines an API for the issuance of [[ref: Digital Credential]]s.
OID4VCI [issuance flow variations](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html#name-issuance-flow-variations) leave room for optionality.

In many situations, [[ref: Digital Credential]]s are issued on the [[ref: Issuer]]'s online service (website). This online service may have already authenticated and authorized the user before displaying the credential offer. Another authentication or authorization is not needed in those situations.

Authorization Code Flow provides a more advanced way of implementing credential issuance. 

**Requirement: DIIP-compliant implementations MUST support both *Pre-Authorized Code Flow* and *Authorization Code Flow*.**

**Requirement: DIIP-compliant implementations MUST support the Transaction Code when using *Pre-Authorized Code Flow*.**

**Requirement: DIIP-compliant implementations MUST support the `trust_chain` claim when using *Pre-Authorized Code Flow*.**

**Requirement: DIIP-compliant implementations MUST NOT assume the Authorization Server is on the same domain as the [[ref: Issuer]].**

It should be noted that various [Security Considerations](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html#name-pre-authorized-code-flow-2) have been described in the [[ref: OID4VCI]] specification with respect to implementing *Pre-Authorized Code Flow*. Parties implementing DIIP are strongly suggested to implement mitigating measures, like the use of a Transaction Code.

[[ref: OID4VCI]] defines *Wallet-initiated* and *Issuer-initiated* flows. *Wallet-initiated* means that the [[ref: Wallet]] can start the flow without any activity from the [[ref: Issuer]]. The *Issuer-initiated* flow seems to be more common in many use cases and seems to be supported more widely. It also aligns better with the use cases where the [[ref: Holder]] is authenticated and authorized in an online service before the credential offer is created and shown.

**Requirement: DIIP-compliant implementations MUST support the *Issuer-initiated* flow.**

[[ref: OID4VCI]] defines *Same-device* and *Cross-device* Credential Offer. People should be able to use both their desktop browser and their mobile device's browser when interacting with the [[ref: Issuer]]'s online service.

**Requirement: DIIP-compliant implementations MUST support both *Same-device* and *Cross-device* Credential Offer.**

[[ref: OID4VCI]] defines *Immediate* and *Deferred* flows. *Deferred* is more complex to implement and not required in most use cases.

**Requirement: DIIP-compliant implementations MUST support the *Immediate* flow.**

### Presentation
The presentation of claims from the [[ref: Holder]]'s [[ref: Wallet]] to the [[ref: Verifier]] is done along the [[ref: OID4VP]]. Other protocols exist, but [[ref: OID4VP]] is very broadly supported and also required by [[ref: HAIP]].

#### OID4VP
Using [[ref: OID4VP]], the [[ref: Holder]]s can also present cryptographically verifiable claims issued by third-party [[ref: Issuer]]s, such that the [[ref: Verifier]] can place trust in those [[ref: Issuer]]s instead of the subject ([[ref: Holder]]).

There are two query languages defined in [[ref: OID4VP]]: *Presentation Exchange* (`PE`) and *Digital Credentials Query Language* (`dcql`). The support for `PE` has already been dropped from the next [[ref: OID4VP]] draft version and can be considered deprecated.

**Requirement: DIIP-compliant implementations MUST support the `dcql_query` in the [Authorization Request](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html#name-new-parameters).**

[[ref: OID4VP]] defines many [Client Identifier Schemes](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html#name-defined-client-identifier-s). One way to identify [[ref: Verifier]]s is through [[ref: OpenID Federation]]. Since DIIP uses [[ref: OpenID Federation]] as the trust infrastructure, it is natural to identify parties using the same protocol.

**Requirement: DIIP-compliant implementations MUST support the `https` *Client Identifier Scheme*.**

***Note: The next [[ref: OID4VP]] draft versions may require that the `https` *Client Identifier Scheme* be prefixed in some way in the *presentation request*. See https://github.com/openid/OpenID4VP/pull/401.***

<!--
'#### SIOP
Using [[ref: SIOPv2 D13]], [[ref: Holder]]s can authenticate themselves with self-issued ID tokens and present self-attested claims directly to [[ref: Verifier]]s (Relying Parties). The OpenID provider (OP) as specified in [[ref: OpenID Connect Core]] are under the subject's local control.
-->

### Digital Credentials API
[[ref: DC API]] is a new W3C specification. The next versions of the DIIP protocol will most likely require compliant solutions to support [[ref: DC API]]. If DIIP v4 compliant implementations support [[ref: DC API]], they should try to use that for credential issuance and verification and fall back to custom URI schemes if required.

### Validity and Revocation Algorithm
Expiration algorithms using [validFrom](https://www.w3.org/TR/vc-data-model-2.0/#defn-validFrom) and [validUntil](https://www.w3.org/TR/vc-data-model-2.0/#defn-validUntil) are a powerful mechanism to establish the validity of credentials. Evaluating the expiration of a credential is much more efficient than using revocation mechanisms. While the absence of `validFrom` and `validUntil` would suggest a credential is considered valid indefinitely, it is recommended that all implementations set validity expiration whenever possible to allow for clear communication to [[ref: Holder]]s and [[ref: Verifier]]s.

**Requirement: DIIP-compliant implementations MUST support checking the validity status of a [[ref: Digital Credential]] using `validFrom` and `validUntil` when they are specified.**

The [[ref: IETF Token Status List]] defines a mechanism, data structures, and processing rules for representing the status of [[ref: Digital Credential]]s (and other "Tokens"). The statuses of Tokens are conveyed via a bit array in the Status List. The Status List is embedded in a Status List Token.

The [[ref: Bitstring Status List]] is based on the same idea as the [[ref: IETF Token Status List]] and is simpler to implement since it doesn't require signing of the status list. The [[ref: IETF Token Status List]] may gain more support since it is recommended by [[ref: HAIP]].

**Requirement: DIIP-compliant implementations MUST support [[ref: IETF Token Status List]] as a status list mechanism.**

## Terminology

This section consolidates in one place common terms used across open standards that this profile consists of. For the details of these, as well as other useful terms, see the text within each of the specifications listed in [References](#references).


[[def: Agent]]
~ A software application or component that an [[ref: Issuer]] uses to issue [[ref: Digital Credential]]s or that a [[ref: Verifier]] uses to request and verify them.

[[def: Holder]]
~ An entity that possesses or holds [[ref: Digital Credential]]s and can present them to [[ref: Verifier]]s.

[[def: DID]]
~ Decentralized Identifier as defined in [[ref: DID Core]].

[[def: Issuer]]
~ A role an entity can perform by asserting claims about one or more subjects, creating a [[ref: Digital Credential]] from these claims, and transmitting the [[ref: Digital Credential]] to a [[ref: Holder]], as defined in [[ref: W3C VCDM]].

[[def: Digital Credential]]
~ A set of one or more Claims made by an [[ref: Issuer]] that is tamper-evident and has authorship that can be cryptographically verified.

<!--
[[def: Relying Party]]
~ See [[ref: Verifier]].
-->

<!--
[[def: Verifiable Presentation]]
~ A Presentation that is tamper-evident and has authorship that can be cryptographically verified.
-->

[[def: Verifier]]
~ An entity that requests and receives one or more [[ref: Digital Credential]]s for processing.

[[def: Wallet]]
~ A software application or component that receives, stores, presents, and manages credentials and key material of an entity. 

## References

### Normative References

[[def: did:jwk]]
~ [did:jwk Method Specification](https://github.com/quartzjer/did-jwk/blob/main/spec.md). Status: Draft.

[[def: did:web]]
~ [did:web Method Specification](https://w3c-ccg.github.io/did-method-web/). Status: Unofficial working group draft.

[[def: ES256]]
~ `ECDSA` using `P-256` ([[ref: Secp256r1]]) and `SHA-256` as specified in [RFC 7518 JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518). Status: RFC - Proposed Standard.

[[def: IETF Token Status List]]
~ [Token Status List - draft 10](https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/10/). Status: Internet-Draft.

[[def: OID4VCI]]
~ [OpenID for Verifiable Credential Issuance - draft 15](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html). Status: Second Implementer's Draft.

[[def: OID4VP]]
~ [OpenID for Verifiable Presentations - draft 23](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html). Status: Third Implementer's Draft.

[[def: OIDF Wallet Architectures]]
~ [OpenID Federation Wallet Architectures 1.0 - draft 03](https://openid.net/specs/openid-federation-wallet-1_0-03.html). Status: Draft.

[[def: OpenID Federation]]
~ [OpenID Federation 1.0 - draft 42](https://openid.net/specs/openid-federation-1_0-42.html). Status: draft.

[[def: Secp256r1]]
~ `Secp256r1` curve in [RFC 5480 ECC SubjectPublicKeyInfo Format](https://datatracker.ietf.org/doc/html/rfc5480). Status: RFC - Proposed Standard.
~ This curve is called `P-256` in [RFC 7518 JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518). Status: RFC - Proposed Standard.

[[def: W3C VCDM]]
~ [Verifiable Credentials Data Model v2.0](https://www.w3.org/TR/vc-data-model-2.0/). Status: W3C Proposed Recommendation.

[[def: VC-JOSE-COSE]]
~ [Securing Verifiable Credentials using JOSE and COSE](https://www.w3.org/TR/vc-jose-cose/). Status: W3C Proposed Recommendation.

### Non-Normative References

[[def: ARF]]
~ [Architecture and Reference Framework](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/architecture-and-reference-framework-main/). Status: Draft.

[[def: Bitstring Status List]]
~ [Bitstring Status List v1.0](https://www.w3.org/TR/vc-bitstring-status-list/). Status: W3C Proposed Recommendation.

[[def: DC API]]
~ [Digital Credentials](https://wicg.github.io/digital-credentials/). Status: Draft Community Group Report.

[[def: DID Core]]
~ [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-1.0/). Status: W3C Recommendation.

[[def: did:webvh]]
~ [The did:webvh DID Method v0.5](https://identity.foundation/didwebvh/). Status: CURRENT STABLE.

[[def: eIDAS]]
~ [Regulation (EU) No 910/2014 of the European Parliament and of the Council of 23 July 2014 on electronic identification and trust services for electronic transactions in the internal market and repealing Directive 1999/93/EC](https://eur-lex.europa.eu/eli/reg/2014/910). Status: In force.

[[def: HAIP]]
~ [OpenID4VC High Assurance Interoperability Profile](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html). Status: Draft.

[[def: Open Badges 3]]
~ [Open Badges Specification, Spec Version 3.0, Document Version 1.2](https://www.imsglobal.org/spec/ob/v3p0). Status: This document is made available for adoption by the public community at large.

[[def: VC-DATA-INTEGRITY]]
~ [Verifiable Credential Data Integrity 1.0](https://www.w3.org/TR/vc-data-integrity/). Status: W3C Proposed Recommendation.