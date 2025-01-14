---
title: Cassandra troubleshooting
linkTitle: Cassandra troubleshooting
weight: 8
date: 2021-08-19
description: |
  Troubleshoot problems you might encounter when running Cassandra with API Gateway.
---

### Error when starting Cassandra

If the following exception occurs during the Cassandra startup process:

```
Exception (java.lang.RuntimeException) encountered during startup: A node with address <host>/<IP> already exists, cancelling join. Use 
cassandra.replace_address if you want to replace this node.
```

Run the `nodetool removenode` command to remove the old Cassandra node from the cluster before starting the new Cassandra node. You must run the command from a different node than the one currently being upgraded.

1. Run `nodetool status` to get the list of nodes in the cluster and their respective Cassandra host IDs:
    ```
    $ ./nodetool status
    Datacenter: datacenter1
    =======================
    Status=Up/Down
    |/ State=Normal/Leaving/Joining/Moving
    --  Address        Load       Tokens       Owns (effective)  Host ID                               Rack
    DN  10.142.58.95   7.26 MB    256          100.0%            3c201a6f-441a-4510-93fd-53c2025073c3  rack1
    UN  10.142.58.223  1.78 MB    256          100.0%            c2361215-be9f-4e90-8b4a-73085e1b92e1  rack1
    UN  10.142.58.195  7.25 MB    256          100.0%            3e3d345f-48ce-4224-8485-aa8e3fdac632  rack1
    ```
2. Select the `Host ID` of the down node (`DN`) and remove it from the cluster using the `nodetool removenode` command:
    ```
    $cd /home/cassandra-2219/cassandra/bin
    $ ./nodetool removenode 3c201a6f-441a-4510-93fd-53c2025073c3
    ```

### Restoring to Cassandra 2.2.8/2.2.12

In the event of a failed migration, you can restore your Cassandra environment to its original 2.2.8/2.2.12 state using the backup data. For more information, see [Restore API Management and KPS keyspaces](/docs/cass_admin/cassandra_bur/#restore-api-management-and-kps-keyspaces-manually).
