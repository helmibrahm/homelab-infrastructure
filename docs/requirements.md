# Project definition and infrastructure requirements

[Documentation index](index.md) · [Architecture](architecture.md) · [Case study](case-study.md)

## 1. Project background and problem statement

I built this homelab to learn self-hosting, FastAPI deployment, networking and DevOps while keeping personal automation and other workloads off a larger cloud instance. The first experiment was making a containerized FastAPI application reachable from the internet.

My residential connection sits behind carrier-grade NAT (CGNAT). I control the home router, but not the ISP's upstream translation, so a port forward on the router alone cannot give me a public inbound IPv4 endpoint. I put that endpoint on a small DigitalOcean VPS and kept application compute at home. The original FastAPI experiment explains why I built the gateway; it does not establish a current public FastAPI route.

## 2. Objectives and infrastructure requirements

The requirements below are retrospective, not a historical acceptance checklist. Scope covers the gateway, the host-to-host link, the Windows/Hyper-V and Ubuntu layers, and manual container operations. It excludes a guarantee of uptime, a full exposure audit, automated deployment and disaster recovery.

### Requirements traceability

The suggested checks were **not run** for this documentation update. Any operational check would require separately authorized scope.

| ID | Requirement | Existing design and evidence | Suggested acceptance evidence |
| --- | --- | --- | --- |
| INF-01 | Provide public IPv4 ingress independent of the home router. | DigitalOcean VPS and Apache ingress, operator-confirmed ([D01](architecture.md#d01-system-context), [D02](../README.md#d02-hybrid-infrastructure-at-a-glance)). | From an approved external client, reach one explicitly authorized test route via the public edge. |
| INF-02 | Establish an encrypted link between the VPS and local Ubuntu VM. | Direct WireGuard peering, operator-confirmed; prior probe observed a WireGuard interface on each host ([D03](architecture.md#d03-network-topology)). | Verify a current handshake and an authorized test flow separately. |
| INF-03 | Forward selected public requests toward the homelab and manage certificates at the edge. | Apache host-level proxy relationship and VPS certificate tooling, operator-confirmed; per-route TLS and transport unverified ([D04](architecture.md#d04-public-https-request-sequence), [D07](architecture.md#d07-security-trust-boundaries)). | Validate certificate presentation and success for a selected route; separately confirm TLS termination, upstream protocol, and route transport. |
| INF-04 | Run Linux workloads without moving application compute to a larger VPS. | Ubuntu guest on Windows 11 Pro with Hyper-V, operator-confirmed; prior guest indicators corroborate virtualization ([D05](architecture.md#d05-virtualization-deployment)). | Confirm the guest boots and the expected workload is available after a scheduled maintenance exercise. |
| INF-05 | Deploy application stacks manually and separate retained data from container lifecycle. | Docker Compose, bridge networking and named-volume deployment pattern, operator-confirmed; prior Docker/Compose indicators ([D06](architecture.md#d06-docker-service-dependencies)). | Use a disposable stack to verify connectivity and persistence across container replacement. |

### Quality considerations

| Concern | Design choice and boundary |
| --- | --- |
| Cost | Keep the public edge small and use existing home compute. No measured savings or cost targets. |
| Security | Separate internet-facing ingress from home compute and use WireGuard between hosts. Per-route exposure, firewall policy and TLS details are unverified. |
| Availability | The design depends on the gateway, the home connection, and the VM. No availability target, failover test or recovery time was verified. |
| Operability | Manual Compose keeps the lab approachable; release repeatability and rollback have not been demonstrated. |
| Recoverability | Named volumes support persistence, not recovery from host loss. A backup strategy and restore testing are future work. |
