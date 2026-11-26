# Sanitized infrastructure and service inventory

[Documentation index](index.md) · [Architecture](architecture.md) · [Operations](operations.md)

## 3. Infrastructure inventory

| Layer | Public edge | Home workload host |
| --- | --- | --- |
| Hosting | DigitalOcean Droplet | Residential machine running Windows 11 Pro |
| Virtualization and OS | Ubuntu guest on KVM | Ubuntu 26.04 guest on Hyper-V |
| Compute role | Small gateway for public ingress and edge utilities | Larger allocation for application compute |
| Storage model | Not detailed in this case study | Virtual disk with LVM and a separate root logical volume; exact capacities and filesystem usage omitted |
| Network role | Public IPv4, Apache, WireGuard peer | Behind ISP CGNAT; Ubuntu WireGuard peer and application host |

Hosting, compute roles and storage model are operator-confirmed. An earlier restricted probe corroborated selected OS, virtualization and network indicators; the inventory is not a live check.

## 10. Service inventory and dependencies

Container names below are application families or generalized labels, not runtime container names or registry references. Container counts, placements, and states are operator-confirmed. The restricted probe verified an active Docker service and a Compose executable candidate on each host; it did not read the Docker socket.

### Homelab — 16 containers, 11 Compose projects

| Service | Technology or category | State |
| --- | --- | --- |
| AdGuard Home | DNS filtering | Running |
| Vaultwarden | Credential-vault application | Running |
| n8n | Workflow automation | Running |
| Automation helper A | Custom automation helper | Running |
| Automation helper B | Custom automation helper | Running |
| Custom API backend | FastAPI | Running |
| Docmost | Collaborative documentation | Running |
| PostgreSQL | PostgreSQL 16 | Running |
| Redis | Redis 7.2 | Running |
| Portainer | Portainer CE | Running |
| MariaDB | Relational database | Running |
| Profile website | Nginx | Running |
| phpMyAdmin | Database administration UI | Running |
| Custom messaging application | Messaging integration | Running |
| Custom application | Purpose not documented here | Running |
| Browser automation runtime | Selenium standalone Chromium | Stopped |

### VPS — 2 running containers, 1 Compose project detected

| Service | Technology or category | Runtime | State |
| --- | --- | --- | --- |
| Uptime monitoring | Uptime Kuma | Docker | Running |
| Container management | Portainer CE | Docker | Running |
| Ingress | Apache reverse proxy | Host service | Active |
| Host-to-host link | WireGuard | Host networking | Direct peer relationship confirmed |

Only the first two VPS rows are containers. A stopped container is recorded as stopped, not assumed deprecated. Counts and states are an inventory snapshot, not a live check.

### Additional host services

| Host | Service | What the inventory supports |
| --- | --- | --- |
| VPS | Certbot / Let's Encrypt | Certificate management; renewal health and per-route TLS termination are not verified |
| VPS | GitLab Runner | Enabled; job execution, pipeline use, and deployment integration are not confirmed |
| Homelab | Apache | Host service; active at the earlier probe, routing role unconfirmed |
| Homelab | Tailscale | Enabled; active at the earlier probe, no confirmed role in public ingress |
| Homelab | Cloudflared, UFW, Glances | Enabled; current running states and operational roles are not established by enablement |

### Docker and Compose organization

The homelab uses Docker Engine 29.4.1 and Docker Compose v5.1.3, with manual application deployments. Stacks use Docker bridge networks and named Docker volumes. Project counts are known, but actual project names, container-to-project membership, active network and volume usage, image digests, runtime configuration, and resource limits were not collected. No relationship is inferred between PostgreSQL, MariaDB, Redis and any application. Product names describe technologies, not service reachability or access policies.

For future documentation, maintain a [sanitized Compose catalog](../examples/compose-catalog.example.yaml) with aliases, workload category, intended lifecycle, and evidence for membership. Keep live files, addresses and volume inventories out of the portfolio.

### Dependency evidence

| Relationship | Support and boundary |
| --- | --- |
| Public Apache → home upstream boundary | Confirmed at host level only; route mapping and upstream protocol unknown. |
| VPS WireGuard peer ↔ VM WireGuard peer | Direct peering confirmed; per-route transport not established. |
| Home Compose → Engine → workload containers | Confirmed deployment model; not a manifest of project membership, startup order or release automation. |
| Home applications ↔ database/cache services | PostgreSQL, MariaDB and Redis are inventoried; which applications use them is unknown. No connection or required startup edge is drawn. |
| Automation tooling ↔ helpers/browser runtime | Co-location only. No n8n workflow, helper call, Selenium invocation or public webhook is confirmed. |
| Monitoring and management → targets | Uptime Kuma and Portainer placements are documented, not their configured targets, access paths or coverage. |

The [Docker diagram](architecture.md#d06-docker-service-dependencies) models only the confirmed platform pattern. The [synthetic routing record](../examples/proxy-route.example.yaml) is a blank evidence template, not a live configuration.
