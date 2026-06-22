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
title: "User assignable IPv4 option numbers"
abbrev: "UAIPV4ON"
category: info

docname: draft-pinkert-intarea-user-assignable-ipv4-option-numbers-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Internet"
workgroup: "Internet Area Working Group"
keyword:
 - IPv4 options
 - private number range
venue:
  group: "Internet Area Working Group"
  type: "Working Group"
  mail: "int-area@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/int-area/"
  github: "DutchyatWork/rfc-draft-user-assignable-ipv4-option-numbers"
  latest: "https://DutchyatWork.github.io/rfc-draft-user-assignable-ipv4-option-numbers/draft-pinkert-intarea-user-assignable-ipv4-option-numbers.html"

author:
 -
    fullname: Tjeerd J. Pinkert
    organization: Siemens Mobility GmbH
    street: Ackerstrasse 22
    code: '38126'
    city: Braunschweig
    email: tjeerd.pinkert@siemens.com
    uri: https://www.mobility.siemens.com

normative:
  STD5: #RFC791
  STD3: #RFC1122
  STD86: #RFC8200

informative:
  BCP186: #RFC7126

...

--- abstract

The use of IPv4 options on the public internet is limited due to the fact that many currently defined IP options have issues, as indicated in [BCP186].
This serves as an argument to refuse new option definitions for IPv4, even if the proposed IP options have valid use cases on limited domains, or for limited end-host domains.
This I-D proposes to reserve a limited range of 16 IPv4 option numbers for variable assignment to types of IP options on limited domains, and for limited end-host domains on the public internet.
This will enable the use of IP options in two major use cases, without the need to fully standardize IP options, or register numbers for them in the IANA IP option numbers table.

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
