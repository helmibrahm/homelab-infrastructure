# Operations, maintenance, and security

[Documentation index](index.md) · [Trust boundaries](architecture.md#12-security-design-and-trust-boundaries) · [Recovery proposal](backup-and-recovery.md)

## 15. Infrastructure management and maintenance

**Current management model:** Home application deployments are manual via Docker Compose. Portainer CE is recorded on both hosts. An enabled GitLab Runner is not evidence of job execution or deployment automation. The private operational inventory should remain separate from this case study.

### Monitoring: current and proposed

**VPS:** Uptime Kuma runs on the public gateway and can check availability. Configured targets, alert delivery, and coverage of the gateway, VPN, or home applications are unconfirmed. Portainer CE runs on both hosts for container management; its presence does not establish monitoring coverage.

**Homelab:** Glances is enabled and can provide host resource visibility. Its running state, collection mode, and any alerting or export configuration are unconfirmed. Treat Uptime Kuma and Glances as different monitoring capabilities, not as a confirmed integrated system.

**Proposed:** Use a role-based monitoring inventory. Separate externally relevant ingress checks from authorized private checks and host resource signals. Record an owner, expected failure condition, and alert route by category. Verify an alert and its recovery before marking a monitor operational. Track backup completion and restore-test results only after a backup system exists.

### Maintenance: current and proposed

The restricted probe observed a reboot-required marker on both hosts. It did not refresh package metadata; update counts from local cache are not a current maintenance assessment. Confirm maintenance requirements and schedule separately. No checks here authorized package changes, upgrades or reboots.

**Proposed:** Document a maintenance window, compatibility checks, and rollback approach for Ubuntu, Docker, Compose and application upgrades. Capture evidence for a restore test before making riskier changes.

### Security considerations

- Confirm which Apache listeners and routes are actually reachable and where TLS terminates. Let's Encrypt and Certbot are used on the VPS, but renewal health and upstream TLS are not established. Review access controls before exposing any new route; do not assume a service is public or private based on its presence in Docker.
- Limit administrative interfaces to the operator-approved management path. Treat Docker socket access as privileged and never expose it as part of a documentation or monitoring probe.
- Maintain WireGuard peer authorization separately from application permissions. UFW is enabled on the homelab, but no effective rules or exposure policy were verified. Keep private keys, peer endpoints, and network ranges outside the portfolio.
- Record where sensitive values are provisioned by category, not as live files or values. Keep server dumps, operational logs, workflow exports, actual environment files and application data out of Git.
- Before any future publication, review outgoing files and history for sensitive material and publication rights, including diagrams and examples. This repository is not a security audit.

### Open questions

- Individual proxy routes, upstream targets, TLS termination, and actual application traffic over WireGuard.
- Container-to-project mapping, internal service dependencies, application versions other than those expressly documented, and exact persistence mappings.
- Uptime Kuma targets, alert delivery, and whether Glances is running and collecting data. The DNS hosting provider and current certificate renewal health are also unconfirmed.
- Backup implementation and recovery testing; see the [proposed strategy](backup-and-recovery.md).
