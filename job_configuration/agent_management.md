# Agent Management

Veeam Agents provide the ability to backup and restore physical workloads and starting from version 9.5 Update 3, Veeam Backup & Replication lets you centrally deploy and manage *Veeam Agent for Microsoft Windows* and *Veeam Agent for Linux* on computers in your infrastructure from within the Veeam Backup & Replication console.

The central management of Veeam Agents is built on additonal elements in the console, the main elements are:

- **Protection Group** - Container in the Veeam Backup & Replication inventory aimed to combine protected computers (e.g. by type or workload etc.)
- **Agent Backup Job** - Runs on the backup server in the similar way as a regular job for VM data backup. One job can be used to process one or more Protection Groups and/or individual computers.
- **Backup Policy** - Describes configuration of individual Veeam Agent backup jobs running on protected computers (in contrast to Agent Backup Jobs running on the backup server). Settings from a Backup Policy can be applied to one or more individual computers or computers added to the inventory as part of a Protection Group.

For detailed descriptions of the purpose, setup, usage and behavior of these elements please refer to the [Veeam Agent Management Guide].

## Agent for Windows Types
Veeam Agent for Windows is available in two different versions, depending on the way how the agent's backup is managed: A lightweight version and a full version.

The leightweight version does not include a local database or any GUI elements so it cannot be managed on the local agent computer. The full version utilizes a local *Microsoft SQL Server 2012 Express LocalDB Edition* (automatically installed) and installs a set of applications with local user interfaces for creating and managing backup and restore jobs. However, these user interfaces become restricted (i.e. most settings cannot be changed locally) as soon as the agent is under control of a backup policy of a Backup & Replication server (i.e. the agent computer belongs to a Protection Group which is assigned to a backup job in _"managed by agent"_ mode). Details about system requirements and components installed as part of the Veeam Agent package can be found in the [Veeam Agent Management Guide].

 The following table shows the relationships between agent types and management scenarios.

| Management Scenario | Full Backup Agent | Lightweight Agent |
| --- | :---: | :---: |
| **Standalone mode** (manual install, no VBR server) | X |  |
| **Distributed and managed by Backup Server** |  |  |
| Backup job mode: Managed by backup server |  | X |
| Backup job mode: Managed by agent (= Backup Policy) | X |  |

## Protection Groups



<!-- referenced links -->
[Veeam Agent Management Guide]: https://helpcenter.veeam.com/docs/backup/agents/
