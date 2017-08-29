# Mixed Linux and Windows Docker swarm-mode

This is a local setup using Vagrant with VMware Fusion to demonstrate a mixed Linux and Windows Docker swarm-mode.


## Vagrant boxes

There are four VM's with the following internal network and IP addresses:

| VM        | IP address   | Memory | Type    |
|-----------|--------------|--------|---------|
| sw-lin-01 | 192.168.36.2 | 2GB    | Manager |
| sw-lin-02 | 192.168.36.3 | 2GB    | Worker  |
| sw-win-01 | 192.168.36.4 | 2GB    | Worker  |
| sw-win-02 | 192.168.36.5 | 2GB    | Worker  |

To create the four node cluster run

```
vagrant up
```

Depending on your host's memory you may want to spin up only two nodes to have a minimal Linux and Windows Server cluster.

```
vagrant up sw-lin-01 sw-win-01
```


## Swarm Manager

The node `sw-lin-01` is the Swarm manager.

## Swarm worker

The nodes `sw-lin-02`, `sw-win-01` and `sw-win-01` are Swarm workers.

## Open Visualizer

When spinning up the manager node it also starts the Docker swarm visualizer as a service. You can then open the visualizer with

```
open http://192.168.36.2:8080
```

![mixed Linux and Windows Docker swarm](images/mixed-swarm.png)

## Example usage: Overlay network

The folder `demo` contains some helper scripts to use the overlay network. Beginning with Windows Server 2016 update 1066 or Windows 10 Creators Update you can use overlay network.

The overlay network also works in a mixed Linux and Windows Docker swarm.

### Run Portainer service

You can run Portainer as a service in the swarm on port 9000  
