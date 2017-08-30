# Build a mixed Linux and Windows Docker Swarm

This is a local setup using Vagrant demonstrate a mixed Linux and Windows Docker swarm-mode.

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

### Run Portainer service

You can run Portainer as a service in the swarm on port 9000  

## Example usage: 
Use docker-compose to build bootstrap website


