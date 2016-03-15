<!--- master only -->
> ![warning](images/warning.png) This document applies to the HEAD of the calico-mesos-deployments source tree.
>
> View the calico-mesos-deployments documentation for the latest release [here](https://github.com/projectcalico/calico-mesos-deployments/blob/0.26.0%2B1/README.md).
<!--- else
> You are viewing the calico-mesos-deployments documentation for release **release**.
<!--- end of master only -->

# Vagrant Deployed Mesos Cluster with Calico
This guide will start a running Mesos cluster with Calico Networking using a simple `vagrant up`. 
It automates the the installation procedure that is documented in the [RPM Installation Guide](RpmInstallCalicoMesos.md) while additionally running calico-libnetwork to form a mesos cluster which can launch Calico-Networked tasks launched via the Unified Containerizer or the Docker Containerizer.

This guide will start two VMs on your hypervisor with the following layout:
### Master
All services on Master are installed from official upstream RPMs.
 * **OS**: `Centos`
 * **Hostname**: `calico-01`
 * **IP**: `172.18.8.101`
 * **Running Services**:
   * `mesos-master`
   * `etcd`
   * `zookeeper`
   * `marathon`

### Agent
The Mesos Agent is configured using unofficial Mesos-Netmodules RPMs produced by the net-modules repository, as well as the calico-mesos RPM. Once installed, the agent will match the following specification:
 * **OS**: `Centos`
 * **Hostname**: `calico-02`
 * **IP**: `172.18.8.102`
 * **Running Services**:
   * `mesos-agent`
 * **Docker Containers**:
   * `calico-node`
   * `calico-libnetwork`


## Prerequisites
This guide requires a hypervisor with the following specs:

 * [VirtualBox][virtualbox] to host the Mesos master and slave virtual machines
 * [Vagrant][vagrant] to run the script that provisions the Virtual Machines
 * 4+ GB memory
 * 2+ CPU
 * ~16GB available storage space


## Getting Started
1. You must run the vagrant script from its location in the repo as it adds the unit files in its path to each host. First, download it:
  ```
  curl -O https://github.com/projectcalico/calico-mesos-deployments/archive/master.tar.gz
  tar -xvf calico-mesos-deployments-master.tar.gz
  cd calico-mesos-deployments-master
  ```

2. Then launch the Vagrant demo:
  ```
  vagrant up
  ```

You can log into each machine by running:
```
vagrant ssh <HOSTNAME>
```

### Adding More Agents
You can modify the script to use multiple agents. To do this, modify the `num_instances` variable
in the `Vagrantfile` to be greater than `2`.  The first instance created is the master instance, every 
additional instance will be an agent instance.

Every agent instance will take similar form to the agent instance above:

 * **OS**: `Centos`
 * **Hostname**: `calico-0X`
 * **IP**: `172.18.8.10X`
 * **Docker Containers**:
   * `mesos-agent` - `calico/mesos-calico`
   * `calico-node` - `calico/node`

where `X` is the instance number.
 
Each agent instance will require additional storage and memory resources.

## Next steps
See [Our Guide on Using Calico-Mesos](UsingCalicoMesos.md) for info on how to test your cluster and start launching tasks networked with Calico.

[virtualbox]: https://www.virtualbox.org/
[vagrant]: https://www.vagrantup.com/
[![Analytics](https://calico-ga-beacon.appspot.com/UA-52125893-3/calico-containers/docs/mesos/DockerizedVagrant.md?pixel)](https://github.com/igrigorik/ga-beacon)
