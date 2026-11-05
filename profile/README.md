# go-redfish

[![License](https://img.shields.io/badge/license-BSD--3--Clause-blue.svg)](https://github.com/go-redfish/.github/blob/main/LICENSE)
[![Pure Go](https://img.shields.io/badge/pure%20Go-CGO%3D0-00ADD8?logo=go&logoColor=white)](https://github.com/go-redfish/redfish)

**Go libraries for the [DMTF Redfish](https://www.dmtf.org/standards/redfish) API of a server's BMC.**

Today there is one:

- **[redfish](https://github.com/go-redfish/redfish)** reads what a node consumes: the energy
  counter of its chassis (`EnvironmentMetrics.EnergykWh`), else its power, by service discovery,
  with HTTP Basic or session authentication, a CA file for the BMC's certificate, bounded answers
  and typed errors. Its `Meter` turns successive reads into energy: a counter's difference, or the
  trapezoid of a polled power, with no interpolation across a gap and no negative energy when a
  counter resets. Standard library only; the JSON decoders and the meter are fuzzed.

It was written for [go-llmflow/gateway](https://github.com/go-llmflow/gateway), which bills the
electricity a shared GPU machine uses, and is meant to serve other tools that need to read a BMC
without shelling out to a Python client.

## How the code is made

Each rule a library follows is traced to the DMTF document it comes from (the Redfish Specification
and the published JSON schemas), opened and cited in the package documentation. Parsers are fuzzed
with a committed corpus; CI runs `go vet`, `go test -race`, a fuzz smoke test and `govulncheck`.
Releases are tags of the form `vMAJOR.MINOR.PATCH` on a merged, green commit.
