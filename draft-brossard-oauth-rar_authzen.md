---
title: "AuthZEN Request/Response Profile for OAuth 2.0 Rich Authorization Requests"
abbrev: "AuthZEN RAR Profile"
category: info

docname: draft-brossard-oauth-rar_authzen-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Web Authorization Protocol"
keyword:
 - authorization
 - abac
 - authzen
 - openid
venue:
  group: "Web Authorization Protocol"
  type: "Working Group"
  mail: "oauth@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/oauth/"
  github: "davidjbrossard/authzen-rar-profile"
  latest: "https://davidjbrossard.github.io/authzen-rar-profile/draft-brossard-oauth-rar_authzen.html"

author:
 -
    fullname: David Brossard
    organization: Axiomatics
    email: david.brossard@axiomatics.com
 -
    fullname: Omri Gazitt
    organization: Aserto
    email: omri@aserto.com
 -
    fullname: Alex Babeanu
    organization: 3Edges
    email: alex@3edges.com

normative:

informative:


--- abstract

This specification defines a profile of OAuth 2.0 Rich Authorization Requests leveraging the OpenID AuthZEN authorization request/response formats within the authorization_details JSON object. Authorization servers and resource servers from different vendors can leverage this profile to request and receive relevant authorization decisions from an AuthZEN-compatible PDP in an interoperable manner.

--- middle

# Introduction

OpenID AuthZEN is a Working Group under the OpenID Foundation which aims to increase interoperability and standardization in the authorization realm. In particular, AuthZEN aims to:
- build standards-based authorization APIs
- define standard design patterns for authorization
- produce educational material to help raise awareness of externalized authorization.

The aim of this profile is to define an AuthZEN-conformant profile of the OAuth 2.0 Rich Authorization Requests [RFC9396]. [RFC9396] introduces a new parameter authorization_details that allows clients to specify their fine-grained authorization requirements using the expressiveness of JSON [RFC8259] data structures.

This specification introduces a more structured format for the authorization_details parameter. The new format is also JSON [RFC8259] as a result of which this specification is conformant with [RFC9396] and is merely a stricter profile.

For example the authorization request for a credit transfer mentioned in [RFC9396] would now be structured as follows

````
{
    "subject": {
        "type": "user",
        "id": "Alice"
    },
    "resource": {
        "type": "payment_initiation",
        "id": "123",
        "recipient": {
            "creditorName": "Merchant A",
            "creditorAccount": {
                "bic": "ABCIDEFFXXX",
                "iban": "DE02100100109307118603"
            }
        }
    },
    "action": {
        "name": "transfer",
        "instructedAmount": {
            "currency": "EUR",
            "amount": "123.50"
        }
    },
    "context": {
        "remittanceInformationUnstructured": "Ref Number Merchant"
    }
}
````

Using AuthZEN as a format for authorization_details will increase the usability and the interoperability of [RFC9396]. In particular, it will be possible for the AS to forward the contents of the authorization_details parameter to an AuthZEN-conformant PDP.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This specification uses the terms "access token", "refresh token", "authorization server" (AS), "resource server" (RS), "authorization endpoint", "authorization request", "authorization response", "token endpoint", "grant type", "access token request", "access token response", and "client" defined by "The OAuth 2.0 Authorization Framework" [RFC6749].
This specification uses the terms "PDP" and "PEP" defined by "eXtensible Access Control Markup Language (XACML) Version 3.0" [XACML].


# Security Considerations

The Security Considerations of [RFC9396], [RFC6749], [RFC7662], and [RFC8414] all apply.

# References

## Normative References

[RFC2119]
Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, DOI 10.17487/RFC2119, March 1997, <https://www.rfc-editor.org/info/rfc2119>.

[RFC7519]
Jones, M., Bradley, J., and N. Sakimura, "JSON Web Token (JWT)", RFC 7519, DOI 10.17487/RFC7519, May 2015, <https://www.rfc-editor.org/info/rfc7519>.

[RFC7662]
Richer, J., Ed., "OAuth 2.0 Token Introspection", RFC 7662, DOI 10.17487/RFC7662, October 2015, <https://www.rfc-editor.org/info/rfc7662>.

[RFC8174]
Leiba, B., "Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words", BCP 14, RFC 8174, DOI 10.17487/RFC8174, May 2017, <https://www.rfc-editor.org/info/rfc8174>.

[RFC8414]
Jones, M., Sakimura, N., and J. Bradley, "OAuth 2.0 Authorization Server Metadata", RFC 8414, DOI 10.17487/RFC8414, June 2018, <https://www.rfc-editor.org/info/rfc8414>.

[RFC8628]
Denniss, W., Bradley, J., Jones, M., and H. Tschofenig, "OAuth 2.0 Device Authorization Grant", RFC 8628, DOI 10.17487/RFC8628, August 2019, <https://www.rfc-editor.org/info/rfc8628>.

[RFC8707]
Campbell, B., Bradley, J., and H. Tschofenig, "Resource Indicators for OAuth 2.0", RFC 8707, DOI 10.17487/RFC8707, February 2020, <https://www.rfc-editor.org/info/rfc8707>.

[RFC9396]
Campbell, B., Richer, J., and T. Lodderstedt, "OAuth 2.0 Rich Authorization Requests", RFC 9396, DOI 10.17487/RFC9396, May 2023, <https://www.rfc-editor.org/info/rfc9396>.

## Informative References

[XACML]
Rissanen, E., "eXtensible Access Control Markup Language (XACML) Version 3.0", OASIS Standard, January 2013, <https://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html>

[ABAC]
Hu, V., Ferraiolo, D., Kuhn, R., Schnitzer, A., Sandlin, K., Scarfone, K., "Guide to Attribute Based Access Control (ABAC) Definition and Considerations", NIST Special Publication 800-162, DOI 10.6028/NIST.SP.800-162, January 2014, <https://doi.org/10.6028/NIST.SP.800-162>

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
