# Etcd v2.3.7 Persistent

### Backup Configuration

Backups are enabled/disabled via the `Enable Backups` radio buttons.

The backup period durations must be a sequence of decimal numbers, each with optional fraction and a unit suffix, such as `300ms`, `1.5h` or `2h45m`. Valid time units are `ns`, `us` or `µs`, `ms`, `s`, `m` and `h`.

The `Backup Creation Period` duration indicates at what rate backups should be created. It is not recommended to create backups more often than `30s`.

The `Backup Retention Period` duration indicates at what rate historical backups should be deleted. Backups outside of the retention period are expired after the next successful backup.

The maximum number of backups stored on disk at any given moment follows the equation `ceiling(retention period / creation period)`. For example, `5m` creation period with `4h` retention period would store at most `ceiling(4h / 5m)` backups or `48` backups. A conservative estimate for backup size is `50MB`, so the attached network storage should have at least `2.4GB` free space. Backup sizes will vary depending on usage.

If backups are disabled, the values for `Backup Creation Period` and `Backup Retention Period` are ignored.

### Disaster Recovery

See [this wiki](https://github.com/rancher/rancher/wiki/Kubernetes-Management#disaster-recovery) for instructions.

### Restoring Backups

See [this wiki](https://github.com/rancher/rancher/wiki/Kubernetes-Management#restoring-backups) for instructions.


# Galera Cluster

### Info:

This template creates a MariaDB Galera cluster on top of Rancher. Using the Galera plugin, the MariaDB cluster runs in a multi-master replicated mode.

When deployed from the catalog, a three node cluster is created with a database, root password, database user and password. The cluster is set up for replication between all of the nodes. The cluster sits behind a light weight proxy layer that forwards all reads/writes to a single server in order for transaction locks. The proxy layer is fronted by a Rancher load balancer.

Clients should access the cluster through the load balancer so they do not need to be updated in the event of a failure.

When the cluster is completely stopped and started, user intervention is required to bring it back online. Instructions can be found in the [Galera documentation](http://galeracluster.com/documentation-webpages/quorumreset.html).

The replication mechanism used in the cluster is based on the MySQL dump method. This has the side effect of taking a long time to bring in a new node when there are large data sets.

### Usage:

Once deployed, use a mysql client to connect:

`mysql -u<db_user> -p -h<galera-lb>`
