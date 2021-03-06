HA Zookeeper cluster on Kontena
===============================

[Zookeeper](https://zookeeper.apache.org/) is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

(based on Kafka/Zookeeper stack used in [HHypermap BOP -- Harvard Hypermap, Billion Object Platform](https://github.com/cga-harvard/hhypermap-bop))

## Install

Prerequisites: You need to have working Kontena Container Platform installed. If you are new to Kontena, check quick start guide.


Zookeeper is a stateful service, therefore you must first create a Kontena volume.  For a local volume run the following command:

```
$ kontena volume create --scope instance --driver local zookeeper-cluster-data
```

For local development purposes you can skip volume creation by using the `SKIP_VOLUMES` variable.

Next install the stack itself.  There are a few options available:

| Option | Description |
| -------| ------------|
| `NUM_INSTANCES` | Number of instances of Zookeeper.  Default is 3. |
| `SKIP_VOLUMES` | Boolean, if true no volumes are mapped.  Useful for local development.  Defaults to `false` |

Generally, the default values are good for a basic cluster setup.

To initially install:

```
$ kontena stack install
```

To upgrade:

```
$ kontena stack upgrade zookeeper-cluster
```

Other services inside your Kontena Grid can now connect to Zookeeper using the address `zookeeper.zookeeper-cluster.${GRID}.kontena.local`.

## Caveats

Due to the way Zookeeper works, Kontena service scaling via the `kontena service scale` command will not work properly.  This is due to the `ZOO_SERVERS` variable requiring all available Zookeeper host names to be present.  In order to scale Zookeeper up and down, please run `kontena stack upgrade` and specify the new cluster size via the `NUM_INSTANCES` environment variable or CLI prompt.  Doing this will allow the stack file's Liquid templates to rerun and set the correct `ZOO_SERVERS` value.
