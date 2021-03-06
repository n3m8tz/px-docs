---
layout: page
title: "px-enterprise usage"
keywords: usage
sidebar: home_sidebar
---

## PX-Enterprise Usage  

Portworx storage container expects a config file in "/etc/pwx/config.json" upon startup.
For customers wanting to deploy PX-Enterprise in an automated manner, a config file can be generated by launching 'px-enterprise' 
with the following options:


+ -t <token> token that was provided in email (or arbitrary clusterID)
+ -s <device> of the form /dev/sda, repeat for multiple devices
+ -d <data_network_interface> of the form eth0 - (optional)
+ -m <management_network_interface> of the form eth0 - (optional)
+ -k <key_value_store> of the form [etcd|consul]://<IP>:<port|4001> - (optional)
+ -a will attempt to use all available devices
+ -f when combined with -a will use all available devices even those with a filesystem

Example:
Following the "docker run" command:

```
#sudo docker run --restart=always --name px-enterprise -d --net=host --privileged=true \
-v /run/docker/plugins:/run/docker/plugins \
-v /var/lib/osd:/var/lib/osd:shared \
-v /dev:/dev \
-v /etc/pwx:/etc/pwx \
-v /opt/pwx/bin:/export_bin:shared \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /mnt:/mnt:shared \
-v /var/cores:/var/cores \
-v /usr/src:/usr/src \
--ipc=host \
portworx/px-enterprise ...
```
Provide one of the following examples as command line arguments positioned after "px-enterprise"

```
   -t 06670ede-70af-11e6-beb9-0242fc110003 -s /dev/sdd -s /dev/sde -d eth0 -m eth1
   -t 06670ede-70af-11e6-beb9-0242fc110003 -s /dev/sdd -s /dev/sde
   -t 06670ede-70af-11e6-beb9-0242fc110003 -a -k etcd://10.0.0.123:4001 
   -t 06670ede-70af-11e6-beb9-0242fc110003 -a -f
```

If using Lighthouse as the management interface, then the "-t" argument will be the PWX_TOKEN, visible from **"Manage Clusters -> Get Startup Script"**.  In this case, the **"-k"** argument would not apply, since Lighthouse uses a Portworx hosted **etcd**.

If running in an "air-gapped" mode, then the "-t" argument will correspond to any site-generated ClusterID that should be the same for all nodes participating in the same cluster.  In this case, the **"-k"** argument should point to an existing on-prem version of **etcd** or **consul**.
