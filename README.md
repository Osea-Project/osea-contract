# osea-contract

OpenAPI contract shared by every Osea hypervisor microservice.

Part of the [Osea Project](https://github.com/Osea-Project).

## Status

The first proof-of-concept API is defined in [`openapi.yaml`](openapi.yaml). It supports listing virtual machines and starting a VM through a provider-neutral contract.

## Compatibility

The contract is the single source of truth for the central server and both hypervisor services. Additive changes are preferred. Changes to paths, schemas or error formats affect every implementation and should be reviewed with those repositories.
