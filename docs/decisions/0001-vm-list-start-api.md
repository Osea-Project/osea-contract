# 0001: Define a shared VM list and start API

## Context

The first project milestone is a proof of concept in which both hypervisor services can list and start a VM. Their API must remain provider-neutral, while central authorization and hypervisor-side ACLs enforce tenant boundaries.

## Decision

Define `GET /v1/vms` and `POST /v1/vms/{vmId}/start`. Inventory entries contain an opaque string ID, display name and normalized status. A successful start returns HTTP `202` with an opaque task ID because hypervisors perform start operations asynchronously.

Each service resolves provider-specific routing details from its own inventory. The contract does not expose node names or hypervisor guest types, and it does not make tenant authorization decisions.

## Alternatives considered

- Adding provider-specific node and guest-type fields to the shared VM schema would force other hypervisors to invent those values.
- Accepting node and guest type in the start request would trust routing details supplied by the caller.
- Reporting that a start is complete would misrepresent asynchronous hypervisor behavior.

## Trade-offs and limits

A service may need an extra inventory lookup before starting a VM, but the caller only needs the shared VM ID. The initial contract intentionally omits stop, create, edit, console access and task polling operations.
