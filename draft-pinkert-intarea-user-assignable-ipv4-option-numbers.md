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
category: std

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
  IANA-IP-Option-Numbers:
    target: https://www.iana.org/assignments/ip-parameters/ip-parameters.xhtml#ip-parameters-1
    title: IP Option Numbers
    author:
      -
        org: IANA

informative:
  BCP186: #RFC7126
  RFC8799:

...

--- abstract

The use of IPv4 options on the public internet is limited due to the fact that many currently defined IPv4 options have issues, as indicated in [BCP186].
This serves as an argument to refuse new option definitions for IPv4, even if the proposed IP options have no such issues, and have valid use cases on limited domains, or for limited end-host domains communicating over private networks or the public internet.
This I-D defines a limited range of N IPv4 option numbers for variable assignment, by the user, to types of IP options to be used on limited domains and for limited end-host domains on the public internet.
This will enable the use of IP options in two major use cases, without the need to fully standardize IP options, or register numbers for them in the IANA IP option numbers table.

--- middle

# Introduction

This I-D defines a limited range of N IP option numbers for variable assignment by the user, to zero or more types of IP options.
Such variable assignment of IP options limits the number of world wide unique IP option numbers in the IANA tables that would otherwise have to be handed out for IP options used on limited domains, or by limited end-host domains, and opens up the room for easier experimenting with and use of new IP options without the need of full standardization of these at IETF.
The definition is compliant with [BCP186], [STD5], and [STD3], and could also be applied for IPv6 [STD86].
A limited number of requirements is defined for proper interoperability of end-hosts and routers.
Guidelines for the handling of non-compliant IP options by end-hosts are provided.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

A **limited end-host domain**, is a domain of 2 or more end-hosts connected through private or public networks, that have a common understanding of certain network properties, by assignment, agreement, or convention. E.g., which IP option type is coupled to which IP option number.
In case of a single owner of the end-hosts, properties can be assigned; in case of multiple owners of end-hosts, properties can be agreed upon; in case of general acceptance, properties can be by convention.
The latter is not encouraged.

# Variably assignable IP option numbers

There are N IP option numbers of Class C reserved in [IANA-IP-Option-Numbers] for variable IP option type assignment.
These are IP option numbers X to Y of Class C, corresponding to value range K to L with an unser copy bit (0) and O to P with a set copy bit (1)..
The user can freely choose the value of the copy bit as needed according to the IP option type definition.
(As an example, number 0 to 16 of Class 3 would correspond to value range 96 to 111 with an unset copy bit (0), and 224 to 239 with a set copy bit (1).)

When used in a limited domain as defined in [RFC8799], the IP option types may define editable contents, since both routers and hosts in the domain share knowledge about how to change particular values of an IP option type.
When used in a limited end-hosts domain, IP option types can not define editable contents, since such options are to be left alone, or are to be encapsulated when entering limited domains using IP options.

1. Any option type to be assigned one of the variably assignable IP option numbers SHALL be of the the Case 2 type defined in [RFC791], and have an option-type, an option-length, and (optionally) option data defined.
   This serves the purpose of interoperability with internet routers which may treat the options as unknown, but may want to read further IP options in the IP header.
1. IP packets with variably assignable IP options SHALL be routed over the pubic internet without modification as specified in [BCP186].
1. Users of variably assignable IP options in a limited domain SHALL encapsulate IP packets with variably assignable IP options from foreign domains upon reception at the entry node.
1. Users of variably assignable IP options in a limited domain SHALL decapsulate previously encapsulated IP packets with variably assignable IP options upon transmisison at the exit node.
1. IP option types COULD be registered with a unique name in the IANA IP option type table (TO BE DEFINED BY IANA), whith a publicly available specification.

Note that also existing IP options, e.g. those defined in [RFC791], could be assigned to a variable IP option number, and may be listed in the table with a unique name.

## Treatment of malformed variably assigned IP options on the public internet

IP packets with malformed variably assigned IP options shall be handled as specified in this section.
The definition of malformed IP options includes valid but unknown Case 1 type options.
Systems cannot distinguish if such options are present.
The following issues may arise for unknown variable IP options:

1. The IP option length (with a Case 1 option this is the wrongly interpreted next option's number) exceeds the IHL.
   The IP packet SHALL be dropped.
1. The length is in range of the IHL as stored in the IP header, but the next read IP option type (this may be data of another option) is unknown.
   Interpretation of the IP options SHALL be stopped. The IP packet SHALL NOT be dropped, but SHALL be further relayed according to [BCP186] or SHALL be handled as specified in [STD3] by the end-host.
1. Users of variably assignable IP options in a limited domain encounter unknown IP options on incomming packets.
   Incoming IP packets with unknown IP options, and no variably assignable IP options, SHALL be transported according to [BCP186].

In case of properly formed variably assigned IP options, the normal rules for IP option interpretation shall be followed as indicated in [BCP186].

## Examples

The variable assignment of IP option numbers can now be performed within limited (end-host) domains as follows.
Under the assumption that some option types A to E are to be used, each machine / router in the limited (end-host) domain can be configured with the same option table.
An example is given in Figure 1 below.

     +--------+-----------+
     | Option | Option    |
     | number | type name |
     +--------+-----------+
     |     96 |         A |
     |    100 |         D |
     |    101 |         E |
     |    228 |         B |
     |    239 |         C |
     +--------+-----------+

     Figure 1: Option assignment table

An example for the IANA table requested is given here:

    +-------------+--------------------+-------------------------------+
    | Option type | Option type        | Internet reference            |
    |   name      |   specification    |                               |
    +-------------+--------------------+-------------------------------+
    | IOAM-IPv4   | draft-gafni-ippm-  | https://datatracker.ietf.org/ |
    |             | ioam-ipv4-options- | doc/draft-gafni-ippm-ioam-    |
    |             | 00                 | ipv4-options/00/              |
    +-------------+--------------------+-------------------------------+

    Figure 2: Example how the IP option type registry could look.

# Security Considerations

Security assessment for IP option types to be defined, may be followed after [BCP186].

# IANA Considerations

This document requests IANA to assign a range of IP option numbers X to Y of class C for variable assignment.

This document requests IANA to initiate a table for the registration of unique IP option names to enable assignment by agreement or convention.
The table should at least have columns for:

1. Unique IP option names,
1. Publication name(s), and
1. (when availble) permanent links to the publication.

The requirements for addition to this table SHALL be:

1. A unique, non-discriminating, IP option name, changable upon descretion by IANA upon entry,
1. A valid IP option definition for the foreseen IP protocol, and
1. A reference to the IP option definition that can be assumed to be long-lived, e.g., a scientific publication, or an internet publication on an archive.
1. The referenced publication is stored for long term archiving, is publicly available and is available free of charge.

--- back

# Acknowledgments
{:numbered="false"}

The Author wants to thank Siemens Mobility GmbH for enabling this work.
