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

Today, the Node Software uses SemVer with MAJOR Version zero (0.y.z) per [rule 4 of the SemVer 2.0.0 Spec](https://semver.org/#spec-item-4):

> MAJOR version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.

At the time of writing for this proposal, the current version of the Node Software is `v0.45.2`. As per rule 4, MAJOR `x` is never incremented, while MINOR `y` can (and does) indicate both major and minor changes that can represent breaking, backwards-incompatible changes. PATCH `z` is used to indicate and deliver important bug fixes for a given MINOR `y` release, but typically does not indicate any significant feature or compatibility change.

### The problem

This usage of the SemVer spec has been appropriate for the Node Software project as it iterated and matured through its early stages and initial major milestones and goalposts. Today however, this usage of the SemVer spec presents 3 significant issues:

1. It is not true that the Node Software is in the initial development stages, as the project has completed most of the initial milestones, providing a robust and mature feature set for the greater Hedera community to build upon.

    a. It is not true that "The public API SHOULD NOT be considered stable" as many of the core features exposed through the [Hedera Protobufs](https://github.com/hashgraph/hedera-protobufs) that define the gRPC API have reached some levels of maturity. While new features and potentially some further breaking changes are still possible in some circumstances, for the most part many of the core services the community depends on can be considered relatively stable and safe to build upon

2. It is true that incrementing the MINOR `y` number typically represents both major and minor changes that are not compatible with previous versions of the Node Software

3. It is not possible for the Node Software project to properly utilize and benefit from all three `x.y.z` numbers. The project can only do one of two things under the SemVer spec:

    a. Choose to increment the MAJOR version and never increment the MINOR version. Since most releases represent a new version that not be run in a consensus node network alongside nodes running the previous version, it is unlikely at present that a MINOR release could ever be produced that provided changes that were compatible with the previous release.

    b. Choose to never increment the MAJOR version and continue to use the MINOR version to increment both major and minor changes. In doing this we simply choose to continue to use the versioning scheme as currently implemented. If we choose this path, we are choosing to no longer follow SemVer (at least to the full extent it's defined) and instead choosing to follow [Zero-Based Versioning (0Ver)](https://0ver.org/). 0ver is in fact "Software's most popular versioning scheme!" as the 0ver site well demonstrates with numerous examples. However, it is also well-stated in [the About Page](https://0ver.org/about.html) that "ZeroVer is satire, please do not use it."

    That said, the [0Ver About Page "What to do" section](https://0ver.org/about.html) also provides the answer to a proper versioning scheme that is well suited to the unique aspects and circumstances of the Node Software project and how it is developed:

    > [0ver.org](https://0ver.org/) may list some auspicious names, but it's not a very good look. If you're having trouble picking a version, or are stuck asymptotically approaching an "actual" release, do yourself a favor and slap a [CalVer](https://calver.org/) on it. [You'll be in much better company.](https://calver.org/users.html)


### Real Talk

_Paraphrased from [0Ver About Real Talk](https://0ver.org/about.html#real-talk)_

Today, Hedera Node Software meets the ZeroVer criteria

> **ZeroVer**: If a project has a logo or Wikipedia article, but not a major version, it's officially 0ver.

Though it currently intends to follow SemVer

> [**SemVer**](https://semver.org/#faq): "If your software is being used in production, it should probably already be 1.0.0."

However, there should never _be_ a `1.0.0`. So we can't really use SemVer, and we shouldn't use ZeroVer because it tells you itself it should not be used.

That only leaves us with one option

### The solution

> [**CalVer**](https://sedimental.org/designing_a_version.html#fn:2): "If both you and someone you don't know use your project seriously, then use a serious version."

## User stories

<!--Provide a list of "user stories" to express how this feature, functionality, improvement, or tool will be used by the end user. Template for user story: “As (user persona), I want (to perform this action) so that (I can accomplish this goal).”-->

- As a user of, or contributor to the Hedera Node Software project (whether technical or non-technical), I want to understand what any given versioned release of the software describes in relation to any versioned release that comes before or after it
- As a developer/engineer of any kind involved in the Hedera Node Software project, I want to understand the implications of a given versioned release I may be contributing to in some way.
  - As such a user, I also want to understand what key deliverables may relate to such a release and any deadlines associated with it.
  - As such a user, I want to understand when such a release will be deployed to various pre-production and production environments, and when the release can be expected to no longer be deployed in any actively maintained and operated hashgraph consensus network environment.
- As an engineering manager, project manager, key stakeholder, or governing council member, I want to understand what versioned release is being discussed or communicated at any given time, and when such release will be deployed to various pre-production and production environments, and when the version will be replaced by a future version.
- As a developer, tester, researcher, or user conducting historical analysis of previous Node Software releases using production network state data produced by such releases, I want to understand what version of software produced a given state and what version(s) can be utilized to load such state into a test environment
- As a developer or tester, I want to be able to invoke certain upgrade / migration pathways in a testing environment without the invocation actually performing any operations that may significantly upgrade or otherwise alter an existing state produced by another hashgraph network.
  
## Specification

### Specification Overview

The Node Software project, collectively developed in the [hashgraph/hedera-services](https://github.com/hashgraph/hedera-services) GitHub repository, will adjust its versioning, release tagging, release planning, and communications regarding new releases to utilize the CalVer scheme modeled under the [Ubuntu Release Cycle](https://ubuntu.com/about/release-cycle).

While the Node Software project releases under a different cadence from that of the Ubuntu project, the versioning scheme is compatible with any calendar-based software release cycle. Since the scheme works for any calendar-based cycle, it can also continue to work for the Node Software project should the project ever decide to adjust its release cadence in the future.

In the process of adopting CalVer / Ubuntu version scheme, the project will preserve and maintain the current existing principles, processes, and tooling that have codified its use of SemVer up to this point. This includes but is not necessarily limited to:

  - Documented guidelines and processes for creating and managing release branches and tags
  - CI/CD and build pipelines that evaluate / compare version strings and numbers to determine a given course of action
  - Code within the Node Software project itself that evaluates / compares version strings and numbers to determine a given course of action (though it is intended to enhance some of this functionality in the process of implementing the changes proposed in this HIP)

### Scheme

**Short Version:** Node Software project versioning will follow the convention `YY.MM.z<-modifier>`, while also providing 2 optional Build Number settings to allow invocation of extremely minor version increments to support certain cases that only affect development and testing of the Node Software.

1. `YY` is the first number in the version and indicates the MAJOR version. This number will represent the year a given version was created in two-digit short form.

    e.g., a version created in the calendar year 2024 would be indicated as `24.MM.z<-modifier>`

2. `MM` is the second number in the version and indicates the MINOR version. This number will represent the calendar month a given version was created in two-digit form. If the number representing a particular calendar month is only a single digit, the number should be padded with a 0 to maintain the specified format.

    e.g., a version created in January 2024 would be indicated as `24.01.z<-modifier>`, and a version created in October 2024 would be indicated as `24.10.z<-modifier>`


3. `z` is the third number in the version and indicates the PATCH version (also sometimes referred to as the MICRO version). This version indicates a bug fix of some kind for a software release series that has graduated from pre-release cycles. A bug fix is defined as an internal change that fixes incorrect behavior. This number would start as a single digit beginning at 0 when a new stable release version is created, and would increment by 1 on an as-needed basis. The number of digits in this number can also be incremented to any extent necessary should the need arise.

    e.g., the release series for January 2024 (including pre-releases and the first stable release) would be indicated as `24.01.0<-modifier>`

    If a bug is later identified that warrants a bug fix for this release (as opposed to or in addition to fixing in a future release cycle) the release's patch number would be incremented by one e.g. `24.01.1<-modifier>`

    If circumstances have provided such that a 10th patch release is required, an additional digit is added to the patch number on such occasion e.g. `24.01.10<-modifier>`

4. `<-modifier>` is the fourth (optional) component in the version and allows for defining an optional text tag, such as "dev", "alpha", "beta", "rc1", and so on. This text tag can be a combination of numbers and strings and would continue to follow SemVer specification and rules regarding pre-releases [(See SemVer 2.0.0 Spec Item 9)](https://semver.org/#spec-item-9), build metadata [(See SemVer 2.0.0 Spec Item 10)](https://semver.org/#spec-item-10), and precedence (referenced and described below in Item 7 of this HIP's scheme)

5. A new optional setting referred to as the **Services Build Number _(actual setting name TBD)_** is added to the Hedera Node Software Services-level configuration that defines a Build Number starting at 0. The Services Build Number should always remain at 0 when versions are created, tagged, and built. A build of the Node Software should NEVER contain a Services Build Number higher than 0. The number is only ever incremented via configuration file on disk (e.g. `bootstrap.properties` or `node.properties`). The number should NEVER be incremented via dynamic configuration file entities (e.g. `0.0.121`) The number should only ever be used in support of development and/or testing purposes and should NEVER be used in any production setting. This optional setting is provided to allow invocation of upgrade / migration code paths without invoking any real migration code paths.

6. A new optional setting referred to as the **Platform Build Number _(actual setting name TBD)_** is added to the Hedera Node Software Platform-level configuration that defines a Build Number starting at 0. The Platform Build Number should always remain at 0 when versions are created, tagged, and built. A build of the Node Software should NEVER contain a Platform Build Number higher than 0. The number is only ever incremented via configuration file on disk (e.g. `settings.txt`). The number should NEVER be incremented via dynamic configuration file entities (e.g. `0.0.121`) should such capability ever be provided for platform-level settings.  The number should only ever be used in support of development and/or testing purposes and should NEVER be used in any production setting. This optional setting is provided to allow invocation of upgrade / migration code paths without invoking any real migration code paths.

7. This version scheme must follow all the rules for Precedence as defined in [SemVer 2.0.0 Item 11](https://semver.org/#spec-item-11)

## Backwards Compatibility

The transition from SemVer to CalVer as specified in this HIP does not present any backwards compatibility issues. In fact, since a given git commit can have multiple tags associated with it, it is possible to go back to any previously issued releases that currently follow SemVer rules and create a new tag alongside the existing SemVer that follows the new CalVer format. It may even be possible to create new tags alongside past releases that predate the adoption of SemVer.

## Security Implications

There are no foreseeable security implications for this HIP

## How to Teach This

Software Versioning is a hard problem. It is easy for a project's versioning scheme to get out of hand, lose its meaning, and confuse technical and non-technical persons alike.

> [I am Bill Gates and today I will be teaching you how to count to ten:](https://old.reddit.com/r/Jokes/comments/4bblmr/i_am_bill_gates_and_today_i_will_be_teaching_you/)
>
> 1, 2, 3, 95, 98, NT, 2000, XP, Vista, 7, 8, 10.

This is one of the major reasons the Node Software project adopted SemVer early on - as the name itself indicates, the version numbers and optional tag carry implicit meaning behind them. The creators of this specification were very diligent in their work and ensured most every aspect of the scheme is well-defined and articulated, while also ensuring succinct high-level summaries are provided so that anyone can understand what a particular version number means for a given software project.

Unfortunately, there are some cases where despite all best intentions, SemVer simply does not work as defined and can result in extremely confusing circumstances. Fortunately, _this_ problem is also well defined and articulated in the satirical 0Ver spec. The creators of this spec were also diligent in clearly demonstrating the problems in summary and in detail. Best of all, the creators take one more step in providing CalVer as a viable alternative for projects where SemVer isn't quite the right fit.

That brings us to CalVer, yet another case where the creators of the specification are diligent in ensuring the problem and solution are well articulated both in summary and in detail. In CalVer's case, the original post [Designing A Version](https://sedimental.org/designing_a_version.html) that CalVer was born from is also provided so as to understand the thought process and collaboration that went into creating the spec.

With the significant bodies of work already produced to teach this, as an open-source project we must simply do what open-source builders do best: stand on the backs of giants, leverage the 85% portion of the work that is already done, fill in the remaining 15% and ship the final result.

### How to teach summary

All of the above to say, to teach this we must

- Reference the source materials SemVer, 0Ver, CalVer, and Designing a Version
- Create a hedera.com blog post and any other useful mediums for community engagement to announce and describe the change
- Add or change a section in the hashgraph/hedera-services repository README that describes the project's use of the Calendar Versioning scheme. This section should provide at minimum
  - A summary version of the Scheme provided in this HIP,
  - A reference to this HIP,
  - A reference to the relevant hedera.com blog post, and
  - A reference to the CalVer site


<!--For a HIP that adds new functionality or changes interface behaviors, it is helpful to include a section on how to teach users, new and experienced, how to apply the HIP to their work.-->

## Reference Implementation

<!--The reference implementation must be complete before any HIP is given the status of “Final”. The final implementation must include test code and documentation.-->

## Rejected Ideas

<!--Throughout the discussion of a HIP, various ideas will be proposed which are not accepted. Those rejected ideas should be recorded along with the reasoning as to why they were rejected. This both helps record the thought process behind the final version of the HIP as well as preventing people from bringing up the same rejected idea again in subsequent discussions.

In a way, this section can be thought of as a breakout section of the Rationale section that focuses specifically on why certain ideas were not ultimately pursued.-->

- Continue to use SemVer by beginning to utilize the MAJOR version number. Such a choice would produce a version 1.0.0, immediately followed by a 2.0.0 the following month, a 3.0.0 after that, and so on. Due to some of the unique aspects of the Node Software, choosing to transition to increment the MAJOR version number implies that the MINOR version would never be used. The end result of this choice is that a 0 gets moved from one position to another.
- Continue to use SemVer the way it is currently being used, by keeping the MAJOR version number at 0. Many successful software projects do this, and their communities begin to treat the projects as stable and dependable even though the 0 MAJOR version number would typically indicate the project to be potentially unstable in early development stages, with breaking changes possible to be introduced at any time
- Create our own versioning strategy. As demonstrated in this HIP and its key resources and references, this is a very hard thing to do that would require significant planning, discussion, documentation, and implementation changes. Doing so would also likely introduce barriers for potential future contributors to the project to overcome.

## Open Issues

- <>

<!--While a HIP is in draft, ideas can come up which warrant further discussion. Those ideas should be recorded so people know that they are being thought about but do not have a concrete resolution. This helps make sure all issues required for the HIP to be ready for consideration are complete and reduces people duplicating prior discussions.-->

## References

- [Semantic Versioning Scheme 2.0.0 (SemVer)](https://semver.org/)
- [Zero-based Versioning (0Ver)](https://0ver.org/)
  - [0Ver About Page](https://0ver.org/about.html)
- [Calendar-based Versioning (CalVer)](https://calver.org/)
  - [Designing a Version](https://sedimental.org/designing_a_version.html)

## Copyright/license

This document is licensed under the Apache License, Version 2.0 -- see [LICENSE](../LICENSE) or (https://www.apache.org/licenses/LICENSE-2.0)
