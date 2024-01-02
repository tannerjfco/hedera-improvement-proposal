---
hip: <HIP number (this is determined by the HIP editor)>
title: Adopt Calendar Versioning Schema (CalVer) for hedera-services repository
author: Tanner Ferguson <tanner@swirldslabs.com>
working-group: Tanner Ferguson <@tannerjfco>, Nathan Klick, Cody Littley, Michael Tinker, Richard Bair, Leemon Baird
type: Standards Track
category: Core | Service
needs-council-approval: Yes
status: Draft
created: 2023-12-29
discussions-to: <a URL pointing to the official discussion thread>
updated: <comma separated list of dates>
requires: <HIP number(s)>
replaces: <HIP number(s)>
superseded-by: <HIP number(s)>
---

## Abstract

This HIP proposes to transition the [hashgraph/hedera-services](https://github.com/hashgraph/hedera-services) repository versioning schema from [Semantic Versioning (SemVer)](https://semver.org/) schema to [Calendar Versioning (CalVer)](https://calver.org/) with an approach modeled off of the [Ubuntu Release Cycle](https://ubuntu.com/about/release-cycle) that is compatible with most of the rules and priniciples that SemVer provides.

## Motivation

In the early days of the Hedera Core Platform and Services projects (collectively referred to as the Consensus Node Software or simply "the Node Software"), the Semantic Versioning schema (SemVer) was adopted to define the versioning strategy for the Hedera Node Software. SemVer is a popular choice for many major open-source software projects. The choice of SemVer was reasonable at the time, and the work over the years to standardize around it for the Consensus Node Software as well as other important Hedera software projects have provided significant benefits to core and community engineering teams, development operations and release engineering teams and more.

A rational and well-defined versioning schema is a critical component to the success of any software project of significance. This is especially true for any open-source software project that needs to foster a vibrant community around to support itself. 

Unfortunately, this fact presents a significant challenge for the maintainers and contributors of the Hedera - SemVer, one of the most popular and well-known versioning schemes ever created, is the wrong choice for the Node Software project.

Fortunately, the right choice already exists which can prevent the Node Software project from needing to define its own custom versioning scheme. Another popular, well-defined versioning scheme has already been defined, and the characteristics of this scheme provide a good fit for the unique characteristics of the Node Software project.

That right choice is to implement a flavor of the Calendar Versioning Schema (CalVer). The implementation of this versioning schema change would be made in such a way to remain compatible with many of the beneficial aspects of the SemVer spec that the Node Software project has standardized around in practice, code, and operations.

## Rationale

The [SemVer 2.0.0 Summary](https://semver.org/#summary) describes the current Node Software scheme succinctly:

> Given a version number MAJOR.MINOR.PATCH, increment the:
>> MAJOR version when you make incompatible API changes
>>
>> MINOR version when you add functionality in a backward compatible manner
>>
>> PATCH version when you make backward compatible bug fixes
>
> Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

This version format is also often referred to with letters e.g `x.y.z` where

- `x` == **MAJOR**
- `y` == **MINOR**
- `z` == **PATCH**

Today, the Node Software uses SemVer with Major Version zero (0.y.z) per [rule 4 of the SemVer 2.0.0 Spec](https://semver.org/#spec-item-4):

> Major version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.

At the time of writing for this proposal, the current version of the Node Software is `v0.45.2`. As per rule 4, MAJOR `x` is never incremented, while MINOR `y` can (and does) indicate both major and minor changes that can represent breaking, backwards-incompatible changes. PATCH `z` is used to indicate and deliver important bug fixes for a given MINOR `y` release, but typically does not indicate any significant feature or compatibility change.

This usage of the SemVer spec has been appropriate for the Node Software project as it iterated and matured through its early stages and initial major milestones and goalposts. Today however, this usage of the SemVer spec presents 3 significant issues:

1. It is not true that the Node Software is in the initial development stages, as the project has completed most of the initial milestones, providing a robust and mature feature set for the greater Hedera community to build upon.


    a. It is not true that "The public API SHOULD NOT be considered stable" as many of the core features exposed through the [Hedera Protobufs](https://github.com/hashgraph/hedera-protobufs) that define the gRPC API have reached some levels of maturity. While new features and potentially some further breaking changes are still possible in some circumstances, for the most part many of the core services the community depends on can be considered relatively stable and safe to build upon

2. It is true that incrementing the MINOR `y` number typically represents both major and minor changes that are not compatible with previous versions of the Node Software

3. It is not possible for the Node Software project to properly utilize and benefit from all three `x.y.z` numbers. The project can only do one of two things under the SemVer spec:

    a. Choose to increment the MAJOR version and never increment the MINOR version. Since most releases represent a new version that not be run in a consensus node network alongside nodes running the previous version, it is unlikely at present that a MINOR release could ever be produced that provided changes that were compatible with the previous release.

    b. Choose to never increment the MAJOR version and continue to use the MINOR version to increment both major and minor changes. In doing this we simply choose to continue to use the versioning scheme as currently implemented. If we choose this path, we are choosing to no longer follow SemVer (at least to the full extent it's defined) and instead choosing to follow [Zero-Based Versioning (0Ver)](https://0ver.org/). 0ver is in fact "Software's most popular versioning scheme!" as the 0ver site well demonstrates with numerous examples. However, it is also well-stated in [the About Page](https://0ver.org/about.html) that "ZeroVer is satire, please do not use it."

    That said, the [0Ver About Page "What to do" section](https://0ver.org/about.html) also provides the answer to a proper versioning scheme that is well suited to the unique aspects and circumstances of the Node Software project and how it is developed:

    > [0ver.org](https://0ver.org/) may list some auspicious names, but it's not a very good look. If you're having trouble picking a version, or are stuck asymptotically approaching an "actual" release, do yourself a favor and slap a [CalVer](https://calver.org/) on it. [You'll be in much better company.](https://calver.org/users.html)

## User stories

Provide a list of "user stories" to express how this feature, functionality, improvement, or tool will be used by the end user. Template for user story: “As (user persona), I want (to perform this action) so that (I can accomplish this goal).”

@TODO define user stories - there are many of them
  
## Specification

The Node Software project, collectively developed in the [hashgraph/hedera-services](https://github.com/hashgraph/hedera-services) GitHub repository, will adjust its versioning, release tagging, and release planning to utilize the CalVer scheme modeled under the [Ubuntu Release Cycle](https://ubuntu.com/about/release-cycle). While the Node Software project releases under a different cadence from that of the Ubuntu project, the versioning scheme is compatible with any calendar-based software release cycle. Since the scheme works for any calendar-based cycle, it can also continue to work for the Node Software project should the project ever decide to adjust its release cadence in the future.



## Backwards Compatibility

All HIPs that introduce backward incompatibilities must include a section describing these incompatibilities and their severity. The HIP must explain how the author proposes to deal with these incompatibilities. HIP submissions without a sufficient backward compatibility treatise may be rejected outright.

## Security Implications

If there are security concerns in relation to the HIP, those concerns should be explicitly addressed to make sure reviewers of the HIP are aware of them.

## How to Teach This

For a HIP that adds new functionality or changes interface behaviors, it is helpful to include a section on how to teach users, new and experienced, how to apply the HIP to their work.

## Reference Implementation

The reference implementation must be complete before any HIP is given the status of “Final”. The final implementation must include test code and documentation.

## Rejected Ideas

Throughout the discussion of a HIP, various ideas will be proposed which are not accepted. Those rejected ideas should be recorded along with the reasoning as to why they were rejected. This both helps record the thought process behind the final version of the HIP as well as preventing people from bringing up the same rejected idea again in subsequent discussions.

In a way, this section can be thought of as a breakout section of the Rationale section that focuses specifically on why certain ideas were not ultimately pursued.

## Open Issues

While a HIP is in draft, ideas can come up which warrant further discussion. Those ideas should be recorded so people know that they are being thought about but do not have a concrete resolution. This helps make sure all issues required for the HIP to be ready for consideration are complete and reduces people duplicating prior discussions.

## References

A collections of URLs used as references through the HIP.

## Copyright/license

This document is licensed under the Apache License, Version 2.0 -- see [LICENSE](../LICENSE) or (https://www.apache.org/licenses/LICENSE-2.0)
