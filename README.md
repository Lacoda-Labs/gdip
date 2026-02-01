# GeoDistricts Improvement Proposals (GDIPs)

This repository contains the **GeoDistricts Protocol** specification: numbered improvement proposals (GDIPs), process, and governance. The protocol defines how to objectively generate U.S. congressional districts by geographic division of population—no gerrymandering, comparable and auditable results.

Implementers can adopt the protocol independently. This repo is specification and process only; no application code.

## Quick links

- **[GDIP index](GDIPs/README.md)** — List of all GDIPs (Meta, Required, Optional) with status and descriptions.
- **How to submit a GDIP** — [GDIP-001: Purpose and Guidelines](GDIPs/gdip-001-purpose-and-guidelines.md) and [process/GDIP-PROCESS.md](process/GDIP-PROCESS.md).
- **[Reference implementation](https://github.com/Lacoda-Labs/geodistricts)** — Working code that implements the protocol; use it as an example or run it locally.

## Repository layout

| Path | Contents |
|------|----------|
| [GDIPs/](GDIPs/) | Numbered spec files (gdip-001 through gdip-006) and index |
| [process/](process/) | GDIP process, template, governance |
| [dao/](dao/) | DAO governance, treasury, compensation, and ecosystem documents |

## DAO Governance

The GeoDistricts Protocol includes a comprehensive DAO framework for decentralized governance of the protocol and ecosystem:

- **[DAO Governance](process/GOVERNANCE.md)** — Complete governance structure and token model
- **[Delegate Elections](dao/governance/DELEGATE_ELECTIONS.md)** — Elected representatives and oversight
- **[Working Groups](dao/governance/WORKING_GROUPS.md)** — Specialized committees and operations
- **[Treasury Management](dao/treasury/TREASURY_MANAGEMENT.md)** — Financial strategy and yield optimization
- **[Contributor Programs](dao/compensation/CONTRIBUTOR_PROGRAMS.md)** — Compensation and incentives
- **[Security Framework](dao/security/SECURITY_AUDITS.md)** — Audit program and insurance coverage

## Protocol version and releases

Releases are tagged in this repo (e.g. `protocol-v1.0`). The reference implementation documents which protocol version or GDIP set it follows. See [process/GOVERNANCE.md](process/GOVERNANCE.md) for versioning and maintainers.
