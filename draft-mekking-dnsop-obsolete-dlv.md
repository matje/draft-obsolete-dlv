%%%
title           = "Moving DNSSEC Lookaside Validation (DLV) to Historic Status"
abbrev          = "obsolete-dlv"
workgroup       = "DNS Operations"
area            = "Operations and Management"
submissiontype  = "IETF"
ipr             = "trust200902"
date            = 2019-07-10T10:18:00Z
keyword         = [
    "DNS",
    "DNSSEC",
    "DLV",
]

[seriesInfo]
name            = "Internet-Draft"
value           = "draft-mekking-dnsop-obsolete-dlv-01"
status          = "informational"

[[author]]
initials        = "W.M."
surname         = "Mekking"
fullname        = "Matthijs Mekking"
organization    = "ISC"
 [author.address]
 email          = "matthijs@isc.org"
  [author.address.postal]
  street        = "950 Charter St"
  city          = "Redwood City"
  region        = "CA"
  code          = "94063"
  country       = "Netherlands"

[[author]]
initials        = "D."
surname         = "Mahoney"
fullname        = "Dan Mahoney"
organization    = "ISC"
 [author.address]
 email          = "dmahoney@isc.org"
  [author.address.postal]
  street        = "950 Charter St"
  city          = "Redwood City"
  region        = "CA"
  code          = "94063"
  country       = "USA"

%%%

.# Abstract

This document obsoletes DNSSEC lookaside validation (DLV) and reclassifies
RFCs 4431 and 5074 as Historic.

{mainmatter}


# Introduction

DNSSEC Lookaside Validation (DLV) was introduced to assist with the adoption of
DNSSEC [@!RFC4033] [@!RFC4034] [@!RFC4035] in a time where the root zone
and many top level domains (TLDs) were unsigned, to help entities with signed
zones under an unsigned parent zone, or that have registrars that don't accept
DS records.  The root zone is signed since July 2010 and as of May 2019,
1389 out of 1531 TLDs have a secure delegation from the root; thus DLV has
served its purpose and can now retire.

# Discussion

One could argue that DLV is still useful because there are still some unsigned
TLDs and entities under those zones will not benefit from signing their zone.
However, keeping the DLV mechanism also has disadvantages:

* It reduces the pressure to get the parent zone signed.
* It reduces the pressure on registrars to accept DS records.
* It complicates validation code.

In addition, not every validator actually implements DLV (only BIND 9 and
Unbound) so even if an entity can use DLV to set up an alternate path to its
trust anchor, its effect is limited.  Furthermore, there was one well-known DLV
registry (dlv.isc.org) and that has been deprecated (replaced with a signed
empty zone) on September 30, 2017. With the absence of a well-known DLV
registry service it is unlikely that there is a real benefit for the protocol
on the Internet nowadays.

One other possible reason to keep DLV is to distribute trust anchors for
private enterprises.  However it was never the intention for DLV to be used
for this purpose, and DLV has some properties that do not entirely fit this
use case:

* It would be more desirable if the trust anchors for internal zones have a
  higher priority than the public trust anchors, but DLV works as a fallback.
* Since the zones are related to private networks, it would make more sense
  to make the internal network more secure to avoid name redirection, rather
  than complicate the DNS protocol.

Given these arguments, plus its fairly limited use case, and the above
disadvantages to keep DLV, it is probably not worth the effort of maintaining
the DLV mechanism.

# Moving DLV to Historic Status

There are two RFCs that specify DLV:

1. RFC 4431 [@!RFC4431] specifies the DLV resource record.
2. RFC 5074 [@!RFC5074] specifies the DLV mechanism for publishing trust
   anchors outside the DNS delegation chain and how validators can use them
   to validate DNSSEC-signed data.

This document moves both RFC 4431 [@!RFC4431] and RFC 5074 [@!RFC5074] to
Historic status.  This is a clear signal to implementers that the DLV
resource record and the DLV mechanism SHOULD NOT be implemented or deployed.

## Documents that reference the DLV RFCs

The RFCs that are being moved to Historic status are referenced by a couple
of other documents.  The sections below describe what changes when the DLV
RFCs have been reclassified as Historic.

### Documents that reference RFC 4431

One RFC and one Internet Draft make reference to RFC 4431 [@!RFC4431].

#### RFC 5074

RFC 5074 [@!RFC5074], "DNSSEC Lookaside Validation (DLV)" describes the DLV
mechanism itself, and is being moved to Historic status too.

#### I-D.lhotka-dnsop-iana-class-type-yang

The draft "YANG Types for DNS Classes and Resource Record Types"
[@!I-D.lhotka-dnsop-iana-class-type-yang] refers to RFC 4431 to describe
the DLV entry in the YANG module iana-dns-class-rr-type.  This reference
should be removed.

### Documents that reference RFC 5074

Three RFCs make reference to RFC 5074 [@!RFC5074].

#### RFC 6698

RFC 6698, "The DNS-Based Authentication of Named Entities (DANE) Transport
Layer Security (TLS) Protocol: TLSA" [@!RFC6698] specifies:

  DNSSEC forms certificates (the binding of an identity to a key) by
  combining a DNSKEY, DS, or DLV resource record with an associated RRSIG
  record.  These records then form a signing chain extending from the
  client's trust anchors to the RR of interest.

This document updates RFC 6698 to exclude the DLV resource record from
certificates.

#### RFC 6840

RFC 6840, "Clarifications and Implementation Notes for DNS Security
(DNSSEC)" [@!RFC6840] says that when trust anchors come from different
sources, a validator may choose between them based on the perceived
reliability of those sources. But in reality this does not happen in
validators (both BIND 9 and Unbound have a option for a DLV trust anchor
that can be used solely as a fallback).

This document updates RFC 6840 to exclude the DLV registries from the trust
anchor selection.

#### RFC 8198

RFC 8198, "Aggressive Use of DNSSEC-Validated Cache" [@!RFC8198] only
references RFC 5074 because aggressive negative caching was first proposed
there.

# IANA Considerations

IANA should update the annotation of the DLV RR type (code 32769) to
"Obsolete" in the DNS Parameters registry.

# Security considerations

When the DLV mechanism goes away, zones that rely on DLV for their
validation will be treated as insecure.  The chance that this scenario actually 
occurs is very low, since no well-known DLV registry exists.

# Acknowledgements

Ondrej Sury for initial review.

{backmatter}

