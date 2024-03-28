---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
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
