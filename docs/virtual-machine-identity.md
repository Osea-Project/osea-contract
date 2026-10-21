# Virtual machine identity

## Why a VM id is not enough

`VirtualMachine.id` is chosen by the hypervisor. A Hyper-V id is a GUID, but a
Proxmox `vmid` is a small number that every cluster reuses. Two environments
can therefore return the same id for different machines.

A VM is identified by the pair:

| Field | Source | Meaning |
| --- | --- | --- |
| `environmentNativeId` | `GET /info`, `hostingEnvironment.nativeId` | Cluster or standalone hypervisor that owns the VM |
| `virtualMachineId` | `VirtualMachine.id` | VM inside that environment |

In the contract this pair is `VirtualMachineIdentity`.

## Hosting environment

`HostingEnvironment` is the cluster or standalone hypervisor behind one
microservice. Its `nativeId` comes from the hypervisor (Failover Cluster id,
Proxmox cluster name, machine id), so it survives a reinstall of the
microservice or a change of its URL. The central server stores it when an
administrator registers a microservice, and refuses a registration whose
`nativeId` is already registered.

A VM in a cluster can move between nodes. `hostName` is therefore information,
never part of the identity.

## Storing a VM in the database

`VirtualMachineRecord` shows what the server keeps per VM:

- Unique index on (`environmentId`, `nativeId`).
- `environmentId` points to the registered environment, which gives the
  microservice URL used to send actions to the right place.
- Values from the hypervisor (`name`, `hostName`, `lastKnownPowerState`, CPU,
  memory) are a cache, refreshed on every sync. The microservice stays the
  source of truth.
- `isMissing` marks machines that disappeared, instead of deleting the record,
  so audit log entries keep pointing to something.
- Tenant, project and quota fields belong to the server and are not in this
  contract. The microservice never receives them.
