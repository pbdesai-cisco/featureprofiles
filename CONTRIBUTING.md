# Contributing the Openconfig feature profiles

Thank you for your interest in contributing to OpenConfig feature profiles.  

## Rationale

See the [README](README.md) for an explanation of what OpenConfig feature
profiles are and why we have them.

## Contributing to OpenConfig feature profiles

OpenConfig prefers contributions in the form of code, documentation and
even bug reporting. If you wish to discuss the suitability or approach
for a change this can be done with an issue in the
[OpenConfig feature profiles GitHub](https://github.com/openconfig/featureprofiles/issues).

## Contributor License Agreement

All contributions to OpenConfig feature profiles MUST be Apache 2.0 licensed.
The [Google contributor license agreement (CLA)](https://cla.developers.google.com/),
MUST be signed for any contribution to be merged into the repository.

The CLA is used to ensure that the rights to use the contribution are well
understood by the OpenConfig community. Since copyright over each contribution
is assigned to its authors, code comments should reflect the contribution
made, and the copyright holder. No code will be reviewed if the license is
not explicitly stated, or the CLA has not been signed.

Note that we use the Google CLA because the OpenConfig project is [administered
and maintained by Google](https://opensource.google.com/docs/cla/#why), not to
ascribe any specific rights to a single OpenConfig contributor.

## Make a contribution

To make a contribution to OpenConfig featureprofiles:

1. Open a pull request in the
 [openconfig/featureprofiles](https://github.com/openconfig/featureprofiles)
 repo. A brief description of the proposed addition along with references to
 any discussion issues should be included.
    * Pull requests should be kept small. An ideal change is less than 500 lines.
     Small changes allow detailed discussions of the additions that are
     being made to the model, whilst also ensuring that course-corrections can be
     made early in the process. If a test is growing to more than 500 lines, it
     may need to be broken into multiple smaller tests.

1. The pull request should include both the suggested feature profile textproto
 additions, as well as any relevant additions to tests. Tests should be written
 in golang using the [ONDATRA](https://github.com/openconfig/ondatra) framework.

1. The automated CI running against each pull request will check the pull
 request for compliance.  The author should resolve any issues found by CI.

1. One or more peers in the community may review the pull request.

1. A feature profile repository maintainer will be reponsible for a final review
and approval.  Only a feature repository maintainer can merge a pull request to
the main branch.
  
The aim of this process is maintain the model quality and approach that OpenConfig
working group has strived for since its inception in 2014. Thank you for your contributions!

## Test development style guide

### Directory Organization

The directory tree is organized as follows:

* `cloudbuild/` contains google cloud build scripts for running virtual
    routers in containers on [KNE](https://github.com/google/kne)
* `feature/` contains definition and tests of feature profiles.
* `internal/` contains packages used by feature profile tests.
* `proto/`  contains protobuf files for feature profiles.
* `tools/` contains code used for CI checks.
* `topologies/` contains the testbed topology definitions.
* `yang/` contains autogenerated go structures.

### Test Suite Organization

Test suites should be placed in subdirectories formatted like `feature/<featurename>/[<sub-feature>/]<tests|ate_tests|otg_tests>/<testname>/main_test.go`, for example:

* `feature/interface/` contains feature profiles for interfaces.
* `feature/interface/README.md` - documents the interface feature profile.
* `feature/interface/ate_tests/` contains the interfaces test suite using ATE traffic generation.
* `feature/interface/otg_tests/` contains the interfaces test suite using OTG traffic generation.
* `feature/interface/tests/` contains the interfaces test suite without traffic generation.
* `feature/bgp/` container for BGP feature profiles and test suites.
* `feature/bgp/addpath` contains feature profiles related to BGP.
* `feature/bgp/addpath/README.md` documents the BGP feature profile.
* `feature/bgp/addpath/ate_tests` contains test code related to BGP Add Path features.
* `internal/deviations` contains code which overrides standard tests where
    there are known issues in a DUT.

Within each test directory `<test>_test.md` should document the test suite.  The
`*.go` files should be named after an appropriate [github featureprofiles project](https://github.com/orgs/openconfig/projects/2/views/1?filterQuery=)
item. For example:

* `feature/interface/tests/singleton_test/singleton_test.md` - documentation for the test suite
* `feature/interface/tests/singleton_test/singleton_test.go` implements issue [RT-5.1 Singleton Interface](https://github.com/openconfig/featureprofiles/issues/111).
* `feature/interface/tests/aggregate_test/aggregate_test.go` implements issue [RT-5.2 Aggregate Interface](https://github.com/openconfig/featureprofiles/issues/112).

## Pull Request Title

A PR title should be as descriptive as possible,
preferably starting with the scope (i.e. section in the test plan).

```{.good}
zz_1_1 testbed topology, no traffic generation yet
```

The PR description may add more details as desired to benefit the
reviewers.  The preferred format is:

```{.good}
    * (M) yang/keychain/openconfig-keychain.yang
      - Add a typedef for referencing a keychain from other modules.
    * (M) yang/isis/*
      - Fix support for hello-authentication to allow for references to a
        specific keychain as defined in the keychain model.
      - Fix support for authentication of *SNP packets, referencing a
        keychain that can be used to auth these packets.
      - move IS-IS model to openconfig-inet-types rather than ietf-inet-types.
```

## Code Organization

Each code file should follow this general structure:

```{.todo}
TODO: Add preferred code structure
```

## IP Addresses Assignment

Netblocks used in the test topology should follow IPv4 Address Blocks Reserved
for Documentation ([RFC 5737]) and IPv6 Address Prefix Reserved for
Documentation ([RFC 3849]). In particular:

[RFC 5737]: https://datatracker.ietf.org/doc/html/rfc5737
[RFC 3849]: https://datatracker.ietf.org/doc/html/rfc3849

### IPv4

* `TEST-NET-1`: (192.0.2.0/24): control plane addresses split into /30 subnets for each ATE/DUT port pair.
* `TEST-NET-2`: (198.51.100.0/24): data plane source network addresses used for traffic testing; split as needed.
* `TEST-NET-3`: (203.0.113.0/24): data plane destination network addresses used for traffic testing; split as needed.

### IPv6

2001:DB8::/32 is a very large space, so we divide them as follows.

* 2001:DB8:0::/64: control plane addresses split into /126 subnets for each ATE/DUT port pair.
* 2001:DB8:1::/64: data plane addresses used for traffic testing as the source address; split as needed.
* 2001:DB8:2::/64: data plane addresses used for traffic testing as the destination address; split as needed.

## ASN assignment

Autonomous System numbers used in test should follow Autonomous System (AS)
Number Reservation for Documentation Use ([RFC 5398]). In particular:

[RFC 5398]: https://datatracker.ietf.org/doc/html/rfc5398

* 16-bit ASN: 64496 - 64511 (`0xfbf0` - `0xfbff`)
* 32-bit ASN: 65536 - 65551 (`0x10000` - `0x1000f`)

Both ranges have 16 total numbers each. The hexadecimal notation makes it more
obvious where the range starts and stops.

## Code Style

All code should follow [Go language style guide](https://github.com/golang/go/wiki/CodeReviewComments)
and [Effective Go](https://go.dev/doc/effective_go) for writing readable Go code.
