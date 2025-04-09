Decentralized Identity Interop Profile v4
==================

**Profile Status:** Draft

**Latest Draft:**
[https://FIDEScommunity.github.io/DIIP](https://FIDEScommunity.github.io/DIIP)

Editors:
~ [Eelco Klaver](https://www.linkedin.com/in/eklaver/) (Credenco)
~ [Harmen van der Kooij](https://www.linkedin.com/in/harmenvanderkooij/) (FIDES Labs)
~ [Niels Klomp](https://www.linkedin.com/in/niels-klomp/) (Sphereon)
~ [Niels van Dijk](https://www.linkedin.com/in/creativethings/) (SURFnet)
~ [Samuel Rinnetmäki](https://www.linkedin.com/in/samuel/) (Findynet)

Contributors and previous editors:
~ [Maaike van Leuken](https://www.linkedin.com/in/maaike-van-leuken-0b1b7011a/) (TNO)
~ [Timo Glastra](https://www.linkedin.com/in/timoglastra/) (Animo Solutions)

**Special Thanks:**

This profile is based on a lot of work done by the Decentralized Identity community, given this profile is largely based on and uses sections of the [DIF JWT VC Presentation Profile](https://identity.foundation/jwt-vc-presentation-profile/), we would like to place special thanks to the editors and contributors of that profile.

Participate:
~ [GitHub repo](https://github.com/FIDEScommunity/DIIP.git)
~ [File a bug](https://github.com/FIDEScommunity/DIIP.git/issues)
~ [Commit history](https://github.com/FIDEScommunity/DIIP.git/commits/main)

------------------------------------

## Abstract

The Decentralized Identity Interop Profile, or DIIP for short, defines a set of requirements against existing specifications to enable the interoperable issuance and presentation of [[ref: Digital Credential]]s between [[ref: Wallet]]s and [[ref: Verifier]]s.

This document is not a specification, but a **profile**. It outlines existing specifications required for implementations to interoperate among each other. 
It also clarifies mandatory features for the optionalities mentioned in the referenced specifications.
The main objective of this interoperability profile is to allow for easy adoption, through the choice of standards that are relatively easy to implement.

The main goal for DIIP is to ensure interoperability between Agents and Wallets in cases where device binding of Digital Credentials is not required and the Wallet doesn't need to be trusted. Issuing, holding, and presenting certifications, diplomas, licenses, permits, etc. fit into the scope of DIIP. Using a Wallet for strong customer authentication or for sharing Person Identification Data (PID) are out of DIIP's scope and you should look into ([[ref: HAIP]]) instead.

The profile uses 
- W3C Verifiable Credentials Data Model ([[ref: W3C VCDM]]) as the format of Digital Credentials
- OpenID for Verifiable Credentials Issuance ([[ref: OID4VCI]]) and OpenID for Verifiable Presentations ([[ref: OID4VP]]) as the base protocols for the issuance and verification of Digital Credentials
- [[ref: IETF Token Status List]] as a revocation mechanism
- [[ref: OpenID Federation]] for trust establishment especially between [[ref: Verifier]]s and [[ref: Issuer]]s but also supporting trust decisions of [[ref: Holder]]s

### Relationship to eIDAS regulation and HAIP profile ###

In the context of the European eIDAS regulation ([[ref: eIDAS]]) and its Architecture and Reference Framework ([[ref: ARF]]), the DIIP profile is a profile for "regular" digital credentials, "non-qualified electronic attestations of attributes". The OpenID4VC High Assurance Interoperability Profile ([[ref: HAIP]]) is targeted for high-assurance use cases where it is important to bind the credentials to the [[ref: Holder]]'s private key (device binding). DIIP is the profile for other use cases.

The standards used in the DIIP profile are the same ones that the ARF uses, but the DIIP profile makes different choices than HAIP in many areas where OID4VCI and OID4VP provide optionality. 

### Audience

The audience of the document includes organisations aiming to issue or verify Digital Credentials, as well as the implementers of Digital Credential solutions (Wallets and [[ref: Agent]]s). The first few sections give an overview of the problem area and profile requirements for DIIP. Subsequent sections are detailed and technical, describing the protocol flow and request-responses.

## Status of This Document

The status of the Decentralized Identity Interop Profile v4 is a DRAFT specification under development.

The latest published DIIP profile can be found at [https://FIDEScommunity.github.io/DIIP/latest](https://FIDEScommunity.github.io/DIIP/latest)

### Description

The [[ref: W3C VCDM]] specification defines the data model of Verifiable Credentials but does not prescribe standards for transport protocol, key management, authentication, query language, etc. As a result, if implementers decide which standards to use for their implementations on their own, there is no guarantee that other companies will also support the same set of standards.

The ([[ref: OID4VCI]]) and ([[ref: OID4VP]]) protocols 

This document aims to provide a path to interoperability by standardizing the set of specifications that enable the presentation of JWT VCs between implementers. Future versions of this document will include details on issuance and Wallet interoperability. Ultimately, this profile will define a standardized approach to Verifiable Credentials so that distributed developers, apps, and systems can share credentials through common means.

The DIIP profile sets the minimum requirements that implementations (Agents and Wallets) must support to ensure interoperability. The implementations can support functionality not specified in DIIP.

### Future Work
DIIP describes technologies that are relatively easy to implement. DIIP makes choices within those standards, attempting to set the minimum set of functionality required for interoperability in the use cases in DIIP's scope.

When standards mature and more and more solutions have full support for all the optional functionality in the standards, there may no longer be need for DIIP. The authors believe that this development will take years and that there is a need for DIIP now.

## Structure of this Document

First, this profile outlines open standards required to be supported. Then, it describes the considerations for choosing these open standards. We discuss the standards specific to [issuing](#issuance) or [presentation](#presentation) separately. This document then lists security and privacy considerations, use cases and examples, implementations and testing. 

## Terminology

This section consolidates in one place common terms used across open standards that this profile consists of. For the details of these, as well as other useful terms, see text within each of the specification listed in [[References]].


[[def: Agent]]
~ A software application or component that an Issuer uses to issue Digital Credentials or a Verifier uses to request verify them.

[[def: Holder]]
~ An entity that possesses or holds Verifiable Credentials and can present them to [[ref: Verifier]]s.

[[def: Issuer]]
~ A role an entity can perform by asserting claims about one or more subjects, creating a verifiable credential from these claims, and transmitting the verifiable credential to a [[ref: Holder]], as defined in [[ref: W3C VCDM]].

<!--
[[def: Relying Party]]
~ See [[ref: Verifier]].
-->

[[def: Digital Credential]]
~ A set of one or more Claims made by an [[ref: Issuer]] that is tamper-evident and has authorship that can be cryptographically verified.

<!--
[[def: Verifiable Presentation]]
~ A Presentation that is tamper-evident and has authorship that can be cryptographically verified.
-->

[[def: Verifier]]
~ An entity that requests and receives one or more Verifiable Credentials for processing.

[[def: Wallet]]
~ A software application or component that receives, stores, presents, and manages credentials and key material of an entity. 

## Profile
In this section, we describe the interoperability profile. 

The rationale behind the profile is to increase adoption through making the profile as easy to adopt as possible. DIIP allows for the implementation of simple Decentralized Identity use cases, with less effort to implement the profile as opposed to other profiles. This boils down to using technologies that are well-specified and already have wide adoption. In the sections below, we first give an overview of the standards included in the profile, then we describe the design choices.

### Overview of the specifications

The [Normative References](#normative-references) section links to the versions of specifications that DIIP compliant implementations must support.

- W3C Verifiable Credentials Data Model ([[ref: W3C VCDM]])
- [[ref: ES256]]
- [[ref: OID4VCI]]
- [[ref: OID4VP]]
<!-- - [[ref: SIOPv2]] -->
<!-- - [[ref: Status List 2021 (First public draft)]] -->
<!-- - [[ref: did-web]] and [[ref: did-jwk]] -->
- [[ref: OpenID Federation]]

### Credential Format
The W3C Verifiable Credential Data Model ([[ref: W3C VCDM]]) defines structure and vocabulary well suited for digital credentials in DIIP's scope. The [[ref: Open Badges 3]] credentials use W3C VCDM as the data format.

The VC Data Model v2.0 recommends using Securing Verifiable Credentials using JOSE and COSE ([[ref: VC-JOSE-COSE]]) as an *enveloping proof* mechanism and 
Verifiable Credential Data Integrity 1.0 ([[ref: VC-DATA-INTEGRITY]]) as an *embedded proof* mechanism. **DIIP compliant implementations must support *Securing JSON-LD Verifiable Credentials with SD-JWT* as specified in ([[ref: VC-JOSE-COSE]]).**

### Signature Scheme
When working with JWTs, it is recommended to work with the following two signature algorithms: ES256 and RS256. The first is based on the elliptic curve discrete logarithm problem, whereas the latter is based on the integer factorization problem. Elliptic-curve cryptography can achieve the same security as RSA with much shorter keys. **DIIP compliant implementations must support ES256 (ECDSA using P-256 and SHA-256).**

### Identifiers
- **DIIP compliant implementations must support `JWK` as an identifier of the Issuers, Holders, and Verifiers**
- **DIIP compliant implementations must support [[ref: did:webvh]] as an identifier of the Issuers and Verifiers**

### Trust Establishment
Signatures in Digital Credentials can be used to verify that the content of a credential has not beem tampered with. But anyone can sign a credential and put anything in the issuer field. DIIP uses [[ref: OpenID Federation]] as the trust infrastructure protocol. Issuers and verifiers can publish their own Entity Configurations that point to Trust Authorities. These Trust Authorities publish Entity Statements that verify the identity and the roles of the organizations. The [[ref: OIDF Wallet Architectures]] specification specifies how to use OpenID Federation with Wallets.

- **DIIP compliant Issuer Agents must support publishing Issuer's Entity Configurations as specified in [[ref: OIDF Wallet Architectures]]**
- **DIIP compliant Verifier Agents must support publishing Verifier's Entity Configurations as specified in [[ref: OIDF Wallet Architectures]]**
- **If a Digital Credential contains a `termsOfUse` object with an attribute `federations`, a DIIP compliant Wallet must warn the user before sharing Digital Credentials or Verifiable Presentations with a Verifier who is not a part of the trust chain whose Trust Anchor is the value in the `federations` attribute.**

### Issuance
The issuance of credentials from the [[ref: Issuer]] to the [[ref: Holder]]'s [[ref: Wallet]] is done along the [[ref: OID4VCI]] specification.

#### OID4VCI
OpenID for Verifiable Credential Issuance [[ref: OID4VCI]] defines an API for the issuance of Digital Credentials.
OID4VCI [https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html#name-issuance-flow-variations](issuance flow variations) leaves room for optionality.
- **DIIP compliant implementations must support the *Pre-Authorized Code Flow*.**
- **DIIP compliant implementations must support the *Wallet initiated* flow.**
- **DIIP compliant implementations must support both *Same-device* and *Cross-device* Credential Offer.**
- **DIIP compliant implementations must support the *Immediate* flow.**

### Presentation
The presentation of claims from the [[ref: Holder]]'s [[ref: Wallet]] to the [[ref: Verifier]] is done along the [[ref: OID4VP]].

#### OID4VP
Using OID4VP, the [[ref: Holder]]s can also present cryptographically verifiable claims issued by third-party [[ref: Issuer]]s, such that the Verifier can place trust in those [[ref: Issuer]]s, instead of the subject ([[ref: Holder]]).
- **DIIP compliant implementations must support the `dcql_query` int the [Authorization Request](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html#name-new-parameters).**
- **DIIP compliant implementations must support the `https` [Client Identifier Scheme](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html#name-defined-client-identifier-s).**

<!--
'#### SIOP
Using [[ref: SIOPv2 D13]], [[ref: Holder]]s can authenticate themselves with self-issued ID tokens and present self-attested claims directly to [[ref: Verifier]]s (Relying Parties). The OpenID provider (OP) as specified in [[ref: OpenID Connect Core]] are under the subject's local control.
-->

### Revocation Algorithm
[[ref: IETF Token Status List]] defines a mechanism, data structures and processing rules for representing the status of Digital Credentials (and other "Tokens"). The statuses of Tokens are conveyed via a bit array in the Status List. The Status List is embedded in a Status List Token.

**DIIP compliant implementations must support IETF Token Status Lists embedded in JWT tokens.**

## References

### Normative References

[[def: did:webvh]]
~ [The did:webvh DID Method v0.5](https://identity.foundation/didwebvh/). Status: CURRENT STABLE.

[[def: ES256]]
~ ECDSA using P-256 and SHA-256 as specified in [RFC 7518 JSON Web Algorithms (JWA)](https://datatracker.ietf.org/doc/html/rfc7518). Status: RFC - Proposed Standard.

[[def: IETF Token Status List]]
~ [Token Status List - draft 10](https://datatracker.ietf.org/doc/draft-ietf-oauth-status-list/10/). Status: Internet-Draft.

[[def: OID4VCI]]
~ [OpenID for Verifiable Credential Issuance - draft 15](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID2.html). Status: Second Implementer's Draft.

[[def: OID4VP]]
~ [OpenID for Verifiable Presentations - draft 23](https://openid.net/specs/openid-4-verifiable-presentations-1_0-ID3.html). Status: Third Implementer's Draft.

[[def: OpenID Federation]]
~ [OpenID Connect Federation 1.0 - draft 17](https://openid.net/specs/openid-connect-federation-1_0-ID3.html). Status: Third Implementer's Draft.

[[def: OIDF Wallet Architectures]]
~ [OpenID Federation Wallet Architectures 1.0 - draft 03](https://openid.net/specs/openid-federation-wallet-1_0-03.html). Status: Draft.

[[def: W3C VCDM]]
~ [Verifiable Credentials Data Model v1.1](https://www.w3.org/TR/vc-data-model-2.0/). Status: W3C Proposed Recommendation.

[[def: VC-JOSE-COSE]]
~ [Securing Verifiable Credentials using JOSE and COSE](https://www.w3.org/TR/vc-jose-cose/). Status: W3C Proposed Recommendation.

### Non-Normative References

[[def: ARF]]
~ [Architecture and Reference Framework](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/architecture-and-reference-framework-main/). Status: Draft.

[[def: eIDAS]]
~ [Regulation (EU) No 910/2014 of the European Parliament and of the Council of 23 July 2014 on electronic identification and trust services for electronic transactions in the internal market and repealing Directive 1999/93/EC](https://eur-lex.europa.eu/eli/reg/2014/910). Status: In force.

[[def: HAIP]]
~ [OpenID4VC High Assurance Interoperability Profile](https://openid.net/specs/openid4vc-high-assurance-interoperability-profile-1_0.html). Status: Draft.

[[def: Open Badges 3]]
~ [Open Badges Specification, Spec Version 3.0, Document Version 1.2](https://www.imsglobal.org/spec/ob/v3p0). Status: This document is made available for adoption by the public community at large.

[[def: VC-DATA-INTEGRITY]]
~ [Verifiable Credential Data Integrity 1.0](https://www.w3.org/TR/vc-data-integrity/). Status: Proposed Recommendation.