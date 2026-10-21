# 0001 - Synchronous power actions and bearer token

## Context

The proof of concept must list and start a VM on Hyper-V and Proxmox. Both
hypervisors run power actions as long operations (Proxmox returns a task id,
WMI returns a job). The contract must stay easy to implement in two languages.

## Decision

- Power actions (`start`, `shutdown`, `power-off`) are synchronous and return
  the VM. The microservice waits for the hypervisor to accept the action, not
  for the final state. Callers follow the state with `getVirtualMachine`.
- The central server authenticates with a bearer token. The microservice is
  also network-restricted to the server.
- Errors use RFC 9457 `application/problem+json`.

## Rejected alternatives

- Asynchronous operations (`202` plus an operation resource): more accurate
  for slow actions, but adds a new resource, polling rules and storage in each
  microservice. Can be added later without breaking callers.
- Mutual TLS: stronger, but heavier to set up for the proof of concept.
  Can be added on top of the token.

## Trade-offs

A slow hypervisor can make a request wait until its timeout. Callers must set
a timeout and treat `502` as retryable after checking the VM state.
