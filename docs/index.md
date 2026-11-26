# Hybrid homelab engineering documentation

[Portfolio home](../README.md) · [Requirements traceability](requirements.md#requirements-traceability) · [Evidence and verification](case-study.md#evidence-and-verification)

## Where to start

- **Recruiters:** Start with the [project background](requirements.md#1-project-background-and-problem-statement), then see [what I built and why](case-study.md#14-engineering-decisions-and-trade-offs).
- **Infrastructure engineers:** Read the [infrastructure inventory](service-inventory.md#3-infrastructure-inventory), [network topology](architecture.md#5-network-topology), [HTTPS lifecycle](architecture.md#7-public-https-request-lifecycle), and [trust boundaries](architecture.md#12-security-design-and-trust-boundaries).
- **Technical interviewers:** Start with the [requirements matrix](requirements.md#requirements-traceability), then use the [cost rationale](case-study.md#13-cost-optimization-rationale), [limitations](case-study.md#16-known-limitations), and [future design](case-study.md#17-future-improvements) to discuss alternatives.

## Contents

| Section | Topic |
| --- | --- |
| 1 | [Project background and problem statement](requirements.md#1-project-background-and-problem-statement) |
| 2 | [Objectives and infrastructure requirements](requirements.md#2-objectives-and-infrastructure-requirements) |
| 3 | [Infrastructure inventory](service-inventory.md#3-infrastructure-inventory) |
| 4 | [Hybrid infrastructure architecture](architecture.md#4-hybrid-infrastructure-architecture) |
| 5 | [Network topology](architecture.md#5-network-topology) |
| 6 | [CGNAT problem and WireGuard solution](architecture.md#6-cgnat-problem-and-wireguard-solution) |
| 7 | [Public HTTPS request lifecycle](architecture.md#7-public-https-request-lifecycle) |
| 8 | [Hyper-V virtualization architecture](architecture.md#8-hyper-v-virtualization-architecture) |
| 9 | [Docker container architecture](architecture.md#9-docker-container-architecture) |
| 10 | [Service inventory and dependencies](service-inventory.md#10-service-inventory-and-dependencies) |
| 11 | [Reverse proxy and TLS architecture](architecture.md#11-reverse-proxy-and-tls-architecture) |
| 12 | [Security design and trust boundaries](architecture.md#12-security-design-and-trust-boundaries) |
| 13 | [Cost optimization rationale](case-study.md#13-cost-optimization-rationale) |
| 14 | [Engineering decisions and trade-offs](case-study.md#14-engineering-decisions-and-trade-offs) |
| 15 | [Infrastructure management and maintenance](operations.md#15-infrastructure-management-and-maintenance) |
| 16 | [Known limitations](case-study.md#16-known-limitations) |
| 17 | [Future improvements](case-study.md#17-future-improvements) |

## How to read the evidence

| Label | Meaning |
| --- | --- |
| Operator-confirmed | Firsthand account or a supplied inventory snapshot; not a fresh availability check. |
| Previously observed | Narrow indicators from a prior restricted probe; not an inspection of live routing, firewall rules, container internals, or application health. |
| Conceptual | A role-level model or illustrative request sequence. It does not establish a particular endpoint, protocol leg, or configuration. |
| Unverified | Not established by the evidence available to this case study. It does not mean absent. |
| Proposed | Future work, not a deployed capability. |

Figures D01–D07 describe the existing design or explicitly labeled conceptual views; D08 is proposal-only. The [recovery proposal](backup-and-recovery.md) and [synthetic catalog](../examples/compose-catalog.example.yaml) and [routing record](../examples/proxy-route.example.yaml) are not deployment artifacts.
