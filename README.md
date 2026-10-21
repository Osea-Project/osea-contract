# osea-contract

OpenAPI contract shared by all Osea microservices.

Part of the [Osea Project](https://github.com/Osea-Project).

## Status

Early stage. See the organization profile for the architecture and the roadmap.

## Contract

[`openapi.yaml`](openapi.yaml) describes the API every microservice implements: health, service info, and virtual machine inventory with start, shutdown and power-off. How a VM is identified and stored is in [`docs/virtual-machine-identity.md`](docs/virtual-machine-identity.md). Technical decisions are in [`docs/decisions/`](docs/decisions/).

Validate it with `npx @redocly/cli lint openapi.yaml`.
