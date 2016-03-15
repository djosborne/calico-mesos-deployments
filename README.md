# Calico-Mesos Deployments
This repo provides RPMs for installing Calico Networking for your Mesos Cluster, documentation on how to get up and running, and how to launch calico networked tasks.

<!--- master only -->
> ![warning](docs/images/warning.png) This document applies to the HEAD of the calico-mesos-deployments source tree.
>
> View the calico-mesos-deployments documentation for the latest release [here](https://github.com/projectcalico/calico-mesos-deployments/blob/0.27.0%2B2/README.md).
<!--- else
> You are viewing the calico-mesos-deployments documentation for release **release**.
<!--- end of master only -->

We recommend browsing the docs for the [latest release](https://github.com/projectcalico/calico-mesos-deployments/releases/latest).

- For information on Calico, see [projectcalico.org](http://projectcalico.org)
- For more information on Calico's Mesos integraiton, see [github.com/projectcalico/calico-mesos][calico-mesos]
- Still have questions? Contact us on the #Mesos channel of [Calico's Slack][calico-slack].

## Background
Calico's integrates with the Mesos Networking story in several ways. Each have slightly different behaviors, components, and installations. The following information is specific to Calico's networking integration with each.
#### Docker Containerizer
Calico can network standard Docker Containers launched by Mesos' Docker Containerizer via the calico's docker-libnetwork plugin.

- Containers are launched using the standard Docker Engine
- Compatible with official Mesos RPM releases, and can be added to existing Mesos Clusters
- Compatible with standard Marathon Releases
- Compatible with Mesos DNS

#### Mesos Tasks - Mesos Containerizer
Mesos tasks, launched via the Mesos Containerizer, can also be networked by calico's net-modules plugin. Net-modules is a pluggable networking layer for Mesos Containerizer.
- Task networks can be created on the fly at container creation time

Note: net-modules is a Mesos Module which [requires that Mesos is built with unbundled dependencies](https://github.com/mesos/modules#building-and-installing-mesos). This requirement will be removed once dependency header files are included in Mesos RPM releases, which is expected in an upcoming Mesos Release). We provide prebuilt RPMs of Mesos which include net-modules and are compatible with Calico.

#### Docker Tasks - Unified (Mesos) Containerizer
Since Mesos 0.28, the Mesos containerizer has received the added ability to download and run docker images using its own engine in Mesos 0.28, earning it the new name - "Unified Containerizer". Calico's Mesos Containerizer works with docker images launched in this manner out of the box.

Calico's Networking story remains the same for the Unified. The Unified Containerizer itself has the following beneifts:
- "Unifies" the two container stories, allowing Networking of Docker Images and arbitrary mesos tasks with the same networking engine.
- Compatible with standard Docker Images

Users of Calico's Unified Containerizer Integration should be aware that:
- The Unified Containerizer doesn't make use of the Docker Engine, instead opting to unpack, isolate, and run images itself.
- As mentioned above, the Calico networking story for the Unified Containerizer uses net-modules - a Mesos Module which [requires that Mesos is built with unbundled dependencies](https://github.com/mesos/modules#building-and-installing-mesos). This requirement will be removed from net-modules once dependency header files are included in Mesos RPM releases, which is expected in an upcoming Mesos Release). We provide prebuilt RPMs of Mesos which include net-modules and are compatible with Calico.

## Demo
The Unified Containerizer and Mesos Containerizer stories for Calico require a slightly different 
- [Automatic Vagrant Install For a Running Demo Cluster, Fast](docs/DockerizedVagrant.md): Following this guide to see what a running Mesos cluster with Calico looks with a simple `vagrant up` 

## Install Guides
#### Calico with the Docker Containerizer
Calico is capable of networking Docker tasks launched via the Docker Containerizer by networking them with the calico libnetwork plugin. This networking solution is compatible with official Mesos RPMs and can be added to your existing mesos cluster.
- The [Install Calico for Docker Containerizer](docs/CalicoWithTheDockerContainerizer.md) guide details how to configure docker and calico in your cluster.

#### Install Calico with the Mesos/Unified Containerizer
Calico works as a net-modules compatible networking plugin for mesos, able to natively network standard Mesos Tasks launched via the Mesos Containerizer. Currently, this requires a version of Mesos built with unbundled 3rd party dependencies. The following two guides provide information on building and installing a compatible Mesos with netmodules, and detail how to add Calico to it:
- [RPM Installation](docs/RpmInstallCalicoMesos.md): These RPMs include a custom build of Mesos bundled with Netmodules. 
- [Manual Compilation and Installation](docs/ManualInstallCalicoMesos.md): For an in-depth walkthrough of the full compilation and installation of Mesos, netmodules, and calico, see the Calico-Mesos Manual Install Guide.


[calico-mesos]: https://github.com/projectcalico/calico-mesos/
[calico-slack]: https://calicousers-slackin.herokuapp.com/
[![Analytics](https://calico-ga-beacon.appspot.com/UA-52125893-3/calico-containers/docs/mesos/README.md?pixel)](https://github.com/igrigorik/ga-beacon)
