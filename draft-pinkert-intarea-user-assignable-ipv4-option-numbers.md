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
  RFC791:
  STD3: #RFC1122
  STD86: #RFC8200
  IP-opt-nums: https://www.iana.org/assignments/ip-parameters/ip-parameters.xhtml#ip-parameters-1

informative:
  BCP186: #RFC7126
  RFC8799:

...

--- abstract

The use of IPv4 options on the public internet is limited due to the fact that many currently defined IP options have issues, as indicated in [BCP186].
This serves as an argument to refuse new option definitions for IPv4, even if the proposed IP options have no such issues, and have valid use cases on limited domains, or for limited end-host domains communicating over the public internet.
This I-D defines a limited range of 16 IPv4 option numbers for variable assignment to types of IP options on limited domains, and for limited end-host domains on the public internet.
This will enable the use of IP options in two major use cases, without the need to fully standardize IP options, or register numbers for them in the IANA IP option numbers table.

--- middle

# Introduction

This I-D defines a limited range of 16 IPv4 option numbers for variable assignment to zero or more types of IP options.
Such variable assignment of IP options limits the number of world wide synchronized IP option numbers in the IANA tables that would otherwise have to be handed out for IP options used on limited domains, or by limited end-host domains.
The definition is compliant with [BCP186], [STD5], and [STD3], and could also be applied for IPv6 [STD86].
A limited number of requirements is defined for proper interoperability of end-hosts and routers.
Guidelines for the handling of non-compliant IP options by end-hosts are provided.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Variably assignable IP option numbers

IP option numbers [IP-opt-nums] 0 to 15 of Class 3 are reserved for variable assignment.
The user can freely choose the value of the copy bit.
This corresponds to value ranges 96 to 111 with an unset copy bit (0), and 224 to 239 with a set copy bit (1).

Variably assignable IP options SHOULD be routed over the pubic internet without modification as specified in [BCP186].

Any option type to be assigned one of the variably assignable IP option numbers SHALL be of the the Case 2 type defined in [RFC791], and have an option-type, an option-length, and (optionally) option data defined.

The latter serves the purpose of interoperability with internet routers which may treat the options as unknown, but may want to read further IP options in the IP header.

## Treatment of malformed variably assigned IP options on the public internet

IP packets with malformed variably assigned IP options shall be dropped.

The definition of malformed IP options includes valid but unknown Case 1 type options.
Systems cannot distinguish if such options are present. 
The following issues may arise for unknown variable IP options:

1. The IP option length exceeds the IHL (with a Case 1 option this is actually the next option's number).
   The IP packet SHALL be dropped.
1. The length is in range of the IHL as stored in the IP header, but the next read IP option type (this may be data of another option) is unknown.
   Interpretation of the IP options SHALL be stopped. The IP packet SHALL be further processed according to [BCP186].

In case of properly formed variably assigned IP options, the normal rules for IP option interpretation shall be followed as specified in BCP.

## Examples

The variable assignment of IP option numbers can now be performed within limited domains as follows.
Under the assumption that some option types A to E are to be used, each machine / router in the network can be configured with the same option table.
An example is given in Figure 1 below.

  +--------+--------+
  | Option | Option |
  | number | type   |
  +--------+--------+
  |     96 |      A |
  |    100 |      D |
  |    228 |      B |
  |    239 |      C |
  +--------+--------+

  Figure 1: Option assignment table

When these tables are used in a limited domain as defined in [RFC8799], the IP option types may define editable contents, since both routers and hosts in the domain share knowledge about how to change a particular option value (that is of the same type in the limited domain).

When end-hosts form a limited domain, 

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
