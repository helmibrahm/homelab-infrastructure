# Proposed backup and disaster recovery strategy

**Current state: no backup strategy is implemented.** This is a proposal, not a runbook proven on either server. There is no verified retention schedule, off-host copy, recovery time, or restore test.

## 1. Decide what must be recoverable

Inventory state and configuration privately. Map each application to its actual persistent stores before choosing a backup method. Give credential-vault data, automation state, and application data explicit recovery priorities. Review databases, Docker volumes, n8n settings and automation data, monitoring configuration, and the minimum platform and network configuration required to reconstruct both hosts. Do not infer which apps depend on the observed database containers.

Agree on a recovery point objective (acceptable data loss) and a recovery time objective (acceptable downtime) for each critical workload. Define who can restore it and how recovery credentials are escrowed. Keep exact operational paths, encryption material and endpoint details out of this repository.

## 2. Design recoverable copies

- Prefer app-consistent, documented backups over copying live database files. Select logical backups or documented storage snapshots for databases after their ownership and version are confirmed.
- Keep versioned, encrypted backups with an independently recoverable off-host copy. Consider a separate, protected retention tier for accidental deletion or compromise. A VM checkpoint alone is not a backup.
- Escrow the actual encryption material and compatible configuration needed to restore encrypted application state, separately and securely. Do not commit recovery material or live backup jobs.
- Select a schedule, retention, and integrity check based on recovery objectives rather than assuming daily backups suffice.

## 3. Prove restore before relying on backup

Use an isolated restore target. Rebuild only the platform and dependencies that the application actually requires. Restore its data and configuration from a selected backup, check application startup and a read-only functional path, and record elapsed time and actual recovery point. Keep outbound integrations inactive until safe reactivation is approved. Include a test that the restore still works when one host is unavailable.

## 4. Rollout checkpoints

1. Confirm inventory, ownership, application dependencies, and recovery objectives.
2. Choose approved storage and retention, then test one critical workload in isolation.
3. Expand to remaining services and add monitoring of backup recency and failures.
4. Schedule repeatable restore exercises and revise the recovery order based on evidence.

No backup job, database dump, snapshot or restore was run for this portfolio.
