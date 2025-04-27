## Appendix A: Future Directions

### Credential Formats
A future version of DIIP may require support for [SD-JWT VCLD](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-sd-jwt-vcld) defined in the draft 27 of [[ref: OID4VP]].

### Identifiers

A near-future version of DIIP will probably require support for [[ref: did:webvh]] instead of [[ref: did:web]].

### Trust Establishment
A future version of the profile will likely refer to [[ref: OpenID Federation]].
In [[ref: OpenID Federation]], [[ref: Issuer]]s and [[ref: Verifier]]s publish their own Entity Configurations, which include pointers to Trust Anchors. These Trust Anchors are trusted third parties that publish Entity Statements that allow for verification of the identity and the roles of the organizations. The [[ref: OIDF Wallet Architectures]] specification specifies how to use [[ref: OpenID Federation]] with Wallets.

One way to use [[ref: OpenID Federation]] is described here as a guidance for implementers.

- [[ref: Issuer]] [[ref: Agent]]s publish the [[ref: Issuer]]'s Entity Configurations as specified in [[ref: OIDF Wallet Architectures]]. (Simplified explanation: sign the [[ref: OID4VCI]] issuer metadata as a JWT and publish it in the `.well-known` path.)

- [[ref: Verifier]] [[ref: Agent]]s publishi the [[ref: Verifier]]'s Entity Configurations as specified in [[ref: OIDF Wallet Architectures]]. (Simplified explanation: sign the [[ref: OID4VP]] verifier metadata as a JWT and publish it in the `.well-known` path.)

- If a [[ref: Digital Credential]] contains a [termsOfUse](https://www.w3.org/TR/vc-data-model-2.0/#terms-of-use) object with an attribute `federations`, a DIIP-compliant Wallet MUST warn the user before sharing [[ref: Digital Credential]]s or Verifiable Presentations with a [[ref: Verifier]] for which a trust chain cannot be resolved using the Trust Anchor in the value of the `federations` attribute.

### Presentation

Note that the next [[ref: OID4VP]] draft versions may require that the `did` *Client Identifier Scheme* be prefixed in the *presentation request*. See https://github.com/openid/OpenID4VP/pull/401.

If a new version of DIIP uses [[ref: OpenID Federation]] as the trust infrastructure, it is natural to identify parties using the same protocol and require DIIP-compliant implementations to support the `https` *Client Identifier Scheme* in [[ref: OID4VP]].

### Digital Credentials API
[[ref: DC API]] is a new W3C specification. The next versions of the DIIP protocol will most likely require compliant solutions to support [[ref: DC API]]. If DIIP v4 compliant implementations support [[ref: DC API]], they should try to use that for credential issuance and verification and fall back to custom URI schemes if required.

### References

[[def: DC API]]
~ [Digital Credentials](https://wicg.github.io/digital-credentials/). Status: Draft Community Group Report.

[[def: did:webvh]]
~ [The did:webvh DID Method v0.5](https://identity.foundation/didwebvh/). Status: CURRENT STABLE.

[[def: OIDF Wallet Architectures]]
~ [OpenID Federation Wallet Architectures 1.0 - draft 03](https://openid.net/specs/openid-federation-wallet-1_0-03.html). Status: Draft.

[[def: OpenID Federation]]
~ [OpenID Federation 1.0 - draft 42](https://openid.net/specs/openid-federation-1_0-42.html). Status: draft.

