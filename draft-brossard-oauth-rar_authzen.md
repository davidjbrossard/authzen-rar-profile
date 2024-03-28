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
area: AREA
workgroup: OAuth
keyword:
 - authorization
 - abac
 - authzen
 - openid
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

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

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
