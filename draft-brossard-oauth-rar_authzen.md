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
    RFC2119:
    RFC7519:
    RFC7662:
    RFC8174:
    RFC8414:
    RFC8628:
    RFC8707:
    RFC9396:
    AUTHZEN:
        target: https://openid.github.io/authzen/
        title: OpenID AuthZEN Authorization API
        date: July 2024
        author:
        -
            name: Omri Gazitt
            ins: O. Gazitt
            org: Aserto
        -
            name: David Brossard
            ins: D. Brossard
            org: Axiomatics
        -
            name: Atul Tulshibagwale
            ins: A. Tulshibagwale
            org: SGNL
    BOXCAR:
        target: https://openid.github.io/authzen/authorization-api-1_1#name-access-evaluations-api
        title: OpenID AuthZEN Authorization API
        date: July 2024
        author:
        -
            name: Omri Gazitt
            ins: O. Gazitt
            org: Aserto
        -
            name: David Brossard
            ins: D. Brossard
            org: Axiomatics
        -
            name: Atul Tulshibagwale
            ins: A. Tulshibagwale
            org: SGNL

informative:
    XACML:
        target: https://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html
        title: eXtensible Access Control Markup Language (XACML) Version 3.0, OASIS Standard
        date: January 2013
        author:
        -
            name: Erik Rissanen
            ins: E. Rissanen
            org: Axiomatics AB
    ABAC:
        target: https://doi.org/10.6028/NIST.SP.800-162
        title: Guide to Attribute Based Access Control (ABAC) Definition and Considerations - NIST Special Publication 800-162
        date: January 2014
        author:
        -
            name: Vincent Hu
            ins: V. Hu
            org: NIST
        -
            name: David Ferraiolo
            ins: D. Ferraiolo
            org: NIST

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

~~~~language-json
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
~~~~
{: title='Source Authorization Request' sourcecode-markers="false"}

Using AuthZEN as a format for authorization_details will increase the usability and the interoperability of [RFC9396]. In particular, it will be possible for the AS to forward the contents of the authorization_details parameter to an AuthZEN-conformant Policy Decision Point (PDP).

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This specification uses the terms "access token", "refresh token", "authorization server" (AS), "resource server" (RS), "authorization endpoint", "authorization request", "authorization response", "token endpoint", "grant type", "access token request", "access token response", and "client" defined by "The OAuth 2.0 Authorization Framework" [RFC6749].
This specification uses the terms "PDP" and "PEP" defined by [ABAC] and [XACML].

# Request Parameter "authorization_details"

In [RFC9396], the request parameter authorization_details contains, in JSON
notation, an array of objects.  Each JSON object contains the data to
specify the authorization requirements for a certain type of
resource. This specification defines the format for each one of these objects
such that it conforms to [AUTHZEN] and [RFC9396].

[AUTHZEN] groups JSON datastructures into 4 JSON objects:
- subject: A Subject is the user or robotic principal about whom the Authorization API is being invoked. The Subject may be requesting access at the time the Authorization API is invoked.
- resource: A Resource is the target of an access request. It is a JSON ([RFC8259]) object that is constructed similar to a Subject entity.
- action: An Action is the type of access that the requester intends to perform. Action is a JSON ([RFC8259]) object that contains at least a name field.
- context: The Context object is a set of attributes that represent environmental or contextual data about the request such as time of day. It is a JSON ([RFC8259]) object.

Note: the aforementioned is indicative only. Always refer to [AUTHZEN] for the formal definition of each element.

## "authorization_details" Structure

Because **type** is **REQUIRED**, the new _authorization\_details_ structure is as follows:

- type:
An identifier for the authorization details type as a string. The value for this profile is "authzen". The value is case-insensitive. This field is **REQUIRED**.

- request:
this field contains the entire AuthZEN-conformant authorization request. This field is **REQUIRED**.

## Authorization Details Types

This profile declares a new value for the _type_ field as stated in the previous section. The value for this profile is "authzen". This indicates there will be a field called request and its value will be an AuthZEN-conformant authorization request.

AuthZEN also defines a _type_ field in the Subject and Resource categories. This field is meant to describe the type of user and/or resource required.

## Common Data Fields

No field other than type and authzen shall be allowed in authorization_details when the type is "authzen". All other fields such as the ones mentioned in [RFC9396] shall be inserted inside the AuthZEN request in the relevant object (Subject, Resource, Action, or Context).

# Authorization Request

Conformant to [RFC9396], the authorization_details authorization request parameter can be used to specify authorization requirements in all places where the scope parameter is used for the same purpose, examples include:

- authorization requests as specified in [RFC6749]
- device authorization requests as specified in [RFC8628]
- backchannel authentication requests as defined in [OID-CIBA]

Parameter encoding follows the exact same rules as [RFC9396].


## Example (non-normative)

~~~~language-json
{
  "type": "authzen",
  "request":
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
}
~~~~
{: title='Source Authorization Request' sourcecode-markers="false"}


# Support for Multiple Authorization Requests

[AUTHZEN] supports a profile that allows the expression of multiple authorization requests in a single JSON object. As a result, this profile recommends the use of a single `authorization_details` object containing _boxcarred_ requests as described in [BOXCAR] when possible and the use of the `authorization_details` array otherwise.



## Example (non-normative)

This example is based on the one in [RFC9396] under section 3.  Authorization Request.

~~~~language-json
[
  {
    "type": "authzen",
    "request": 
    {
      "subject": {
        "id": "alice@acmecorp.com",
        "type": "user"
      },
      "resource":{
        "id": "123",
        "type": "account_information",
        "location": "https://example.com/accounts"
      },
      "evaluations": {
        "eval-1": {
          "action": {
            "name": "list_accounts"
          }
        },
        "eval-2": {
          "action": {
            "name": "read_balances"
          }
        },
        "eval-3": {
          "action": {
            "name": "read_transactions"
          }
        }
      }
    }
  },
  {
    "type": "authzen",
    "request":
    {
      "subject": {
        "id": "alice@acmecorp.com",
        "type": "user"
      },
      "resource":{
        "id": "123",
        "type": "payment_initiation",
        "location": "https://example.com/payments",
        "recipient": {
            "creditorName": "Merchant A",
            "creditorAccount": {
                "bic": "ABCIDEFFXXX",
                "iban": "NL02RABO2228161411"
            }
        }
      },
      "context":{
        "remittanceInformationUnstructured": "Ref Number Merchant"
      },
      "evaluations": {
        "eval-1": {
          "action": {
            "name": "initiate",
            "instructedAmount": {
            "currency": "EUR",
            "amount": "123.50"
            }
          }
        },
        "eval-2": {
          "action": {
            "name": "status"
          }
        },
        "eval-3": {
          "action": {
            "name": "cancel"
          }
        }
      }
    }
  }
]
~~~~

# Security Considerations

The Security Considerations of [RFC9396], [RFC6749], [RFC7662], and [RFC8414] all apply.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

   We would like to thank members of the OpenID AuthZEN Working Group for their
   valuable feedback during the preparation of this specification. In particular
   our thanks go to Gerry Gebel and Allan Foster.

   We would also like to thank Justin Richer and Pieter Kasselman for their
   guidance on this spec and the overall IETF process.

