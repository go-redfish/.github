<p align="center">
  <img src="https://raw.githubusercontent.com/go-redfish/brand/main/social/go-redfish.png" alt="go-redfish: Go libraries for the DMTF Redfish API of a server's BMC" width="640">
</p>

# go-redfish

[![License](https://img.shields.io/badge/license-BSD--3--Clause-blue.svg)](https://github.com/go-redfish/.github/blob/main/LICENSE)
[![Pure Go](https://img.shields.io/badge/pure%20Go-CGO%3D0-00ADD8?logo=go&logoColor=white)](https://github.com/go-redfish/redfish)

**Go libraries for the [DMTF Redfish](https://www.dmtf.org/standards/redfish) API of a server's BMC.**

Read what a node consumes, manage it out of band, and test against a mock BMC, without
shelling out to a Python client.

## What is here

| Repo | Latest | |
|---|---|---|
| [`redfish`](https://github.com/go-redfish/redfish) | v0.2.0 | The client: service discovery, Basic or session authentication, a CA file for the BMC's certificate, bounded answers, typed errors; a `Meter` that turns reads into energy; the mock BMC and the `redfish` command are being added. Standard library only. |
| [`brand`](https://github.com/go-redfish/brand) | | The logos of the organisation, in every format. |
| [`docs`](https://github.com/go-redfish/docs) | | The documentation site (Hugo + Hextra): getting started, security model, mock BMC, CLI reference, parity. |
| [`go-redfish.github.io`](https://github.com/go-redfish/go-redfish.github.io) | | This organisation's landing page. |

It was written for [go-llmflow/gateway](https://github.com/go-llmflow/gateway), which bills the
electricity a shared GPU machine uses, and is meant to serve any tool that needs to read or drive a
BMC.

## Parity with the Python tooling

The target is the behaviour of `community.general.redfish_info`, `redfish_command` and
`redfish_config` (the Ansible modules, themselves built on DMTF's `redfishtool`). Which command
is done, which is not, and how each is tested against the reference is tracked in
[`PARITY.md`](https://github.com/go-redfish/redfish/blob/main/PARITY.md) in the `redfish` repository.

## Security posture

A BMC is a privileged, often old, network service; its answers are untrusted input.

- **Bounded reads.** Every response is capped in size; a malformed or oversized answer is a
  typed error.
- **Credentials.** A URL that carries credentials, or is not https, is refused; the password only
  travels over https. Sessions are logged out. Certificates are verified (against a CA file when
  given), TLS 1.2 or later; there is no switch to turn verification off.
- **Fuzzed decoders.** Each JSON decoder and the energy meter is fuzzed, with the corpus committed.
- **CI.** `go vet`, `go test -race`, a fuzz smoke test and `govulncheck` on every change.

## How the code is made

Each rule a library follows is traced to the DMTF document it comes from (the Redfish Specification
and the published JSON schemas), opened and cited in the package documentation. Releases are tags of
the form `vMAJOR.MINOR.PATCH` on a merged, green commit.

## Links

- Landing: <https://go-redfish.github.io/>
- Documentation: <https://go-redfish.github.io/docs/latest/>
