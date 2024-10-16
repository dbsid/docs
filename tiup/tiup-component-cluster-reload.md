---
title: tiup cluster reload
---

# tiup cluster reload

After [modifying the cluster configuration](/tiup/tiup-component-cluster-edit-config.md), the cluster needs to be reloaded using the `tiup cluster reload` command for the configuration to take effect. This command publishes the configuration of the control machine to the remote machine where the service is running, and follows the upgrade process to restart the services in order according to the upgrade process. The cluster is still available during the restart process.

## Syntax

```shell
tiup cluster reload <cluster-name> [flags]
```

`<cluster-name>`: the cluster name to operate on.

## Options

### --force

- Ignores errors in the reloading process and forces reload.
- Data type: `BOOLEAN`
- Default: false

### --transfer-timeout

- When restarting PD or TiKV, the leader of the restarted node is migrated to other nodes first, and the migration process takes some time. You can set the maximum wait time (in seconds) by setting `-transfer-timeout`. After the timeout, the service can be restarted directly without waiting.
- Data type: `uint`
- Default: 300

> **Note:**
>
> In the case of skipping the waiting and restarting directly, the service performance might jitter.

### --ignore-config-check

- After the binary files of components are deployed, the configurations of TiDB, TiKV, and PD components are checked using `<binary> --config-check <config-file>`. `<binary>` is the path of the deployed binary file. `<config-file>` is the configuration file generated based on the user configuration. If you want to skip this check, you can use this option.
- Data type: `BOOLEAN`
- Default: false

### -N, --node

- Specifies the nodes to be restarted. If not specified, all nodes are restarted. The value of this option is a comma-separated list of node IDs. The node ID is the first column of the [cluster status](/tiup/tiup-component-cluster-display.md) table.
- Data type: `strings`
- Default: `[]`, which means all nodes are selected.

> **Note:**
>
> + If the `-R, --role` option is specified at the same time, then the service status in their intersection is restarted.
> + If the `--skip-restart` option is specified, the `-N, --node` option is invalid.

### -R, --role

- Specifies the roles to be restarted. If not specified, all roles are restarted. The value of this option is a comma-separated list of node roles. The role is the second column of the [cluster status](/tiup/tiup-component-cluster-display.md) table.
- Data type: `strings`
- Default: `[]`, which means all roles are selected.

> **Note:**
>
> 1. If the `-N, --node` option is specified at the same time, the services in their intersection is restarted.
> 2. If the `--skip-restart` option is specified, the `-R, --role` option is invalid.

### --skip-restart

The `tiup cluster reload` command performs two operations:

- Refreshes all node configurations
- Restarts the specified node

After you specify the `--skip-restart` option, it only refreshes the configuration without restarting any nodes, so that the refreshed configuration is not applied and does not take effect until the next restart of the corresponding service.

- Data type: `BOOLEAN`
- Default: false

### -h, --help

- Prints the help information.
- Data type: `BOOLEAN`
- Default: false

## Output

The execution log of the tiup-cluster.
