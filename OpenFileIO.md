# "OpenFileIO" Proposal

## Abstract

This proposal calls for the creation of a minimal, open source (C/C++)
library based on the prior art of the OpenImageIO `IOProxy`[^1] and
Houdini File System[^2] APIs. Providing a common cross-platform,
cross-application extensible abstraction of a random access byte stream.
Its purpose is to aid the migration away from `fopen` et. al. as the
de-facto low-level data access mechanism within digital content creation
tooling and infrastructure. This is required to allow secure data access
via other means without the need for complex user-space filesystem
emulations or elaborate data orchestration techniques.

> N.B. "OpenFileIO" is used colloquially as a place-holder name.

## Problem Statement

It is currently difficult to provide data access to processing
operations in media production pipelines though anything other than the
standard filesystem.

This hampers industry adoption of modern storage, security and compute
architectures where data may be held in other forms - such as
access-controlled object stores exposed through HTTP endpoints.

## Background

Within the Media and Entertainment sector Digital Content Creation (DCC)
tools require data to be read, processed and written as part of their
operation. Traditionally data is accessed via the standard filesystem
system calls, often targeting a network mount of a shared storage
resource. With the advent of modern distributed systems architectures,
there is a strong desire to use other mechanism for the secure
transport and storage of necessary data.

The source code of the DCC tools involved is often proprietary and so
can not directly be modified to use alternate transport mechanisms. In
addition, filesystem semantics are often baked into application logic.
Consequently, there have been numerous attempts to wrap these alternate
technologies so they can be accessed through the filesystem itself (e.g.
FUSE[^3] layers, or indirectly via out-of-band data orchestration). All
of these require significant engineering effort and long-term
maintenance and often result in a fragile and incomplete solution.

Conversation with the community surfaced that there are two notable
cases of prior-art[^1][^2] in this space. They form established and
validated solutions to this problem by providing light-weight shims that
present file-like interfaces bridging to custom data access
implementations. This makes their adoption into large, legacy code bases
a feasible proposition.

All of these solutions are however, part of large (and in some cases
proprietary) projects. Consultation with their respective owners has
shown a strong willingness to collaborate on a distilled,
community-owned evolution of the core premise of these existing ideas.

## Proposal

Create a new, minimal, self-contained Apache 2.0 licensed project within
the ASWF community[^4] based on established solutions[^1][^2], to be
used as a base abstraction for byte level I/O within DCC tools and
supporting libraries where appropriate.

Based on the nature of the existing solutions, this would most likely
look like a minimal API surface plus a C plugin system that uses URI
schemes to dispatch calls at run time to a suitable user-provided
handler. Default implementation for `file` would probably be provided.

The library will be designed to facilitate low-friction integration into
existing filesystem-oriented codebases with minimal boilerplate/business
logic changes.

We hope this consideration will go some way to making the migration of
existing codebases a viable proposition.

### Goals

- "Drop-in" replacement for existing filesystem based **byte stream**
  access, that allows data access via arbitrary tertiary mechanisms.
- Easy migration path for existing solutions[^1][^2].
- Run-time extensible to allow new data sources to be used with existing
  binaries.
- Minimal API surface area
- Maximum language compatibility.
- Don't re-invent wheels, this is more about ease of extension than a
  new low-level interface.

### Out of Scope

In order to ensure initial integration is feasible in real-world
projects, the initial scope is deliberately limited to direct byte
stream access.

It seems an important premise to separate out the fundamentals of data
access once some URL has been derived, and how that URL or its peers are
determined in the first place.

The following topics could be investigated at a later stage and
added (potentially as additional "layers"):

- Full filesystem modelling (e.g. directories, resource iteration)
- Additional authorization/authentication flows.

## Next steps

- [ ] Ratify proposal by community members (by Feb 2023).
- [ ] Establish TSC.
- [ ] Formally define initial scope.

## References

[^1]: https://github.com/OpenImageIO/oiio/blob/3b8807df854253911934caaa9587013c47209f35/src/include/OpenImageIO/filesystem.h#L403
[^2]: https://www.sidefx.com/docs/hdk/_h_d_k__f_s.html
[^3]: https://www.kernel.org/doc/html/latest/filesystems/fuse.html
[^4]: https://www.aswf.io
