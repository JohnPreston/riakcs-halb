riakcs-halb
===========

Creates a simple (for now) VRRP ring for HA of the service and setup NGINX on every machines to load-balance the traffic. Each RiackCS node is its own load-balancer and VRRP router.

Requirements
------------

hosts:

- riakcs_nodes

Role Variables
--------------

| Parameter | Required | Default | Description
|--- |--- |--- |---
| nginx_repo_url | Yes | [RiakCS](//nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm) | RiakCS Repository
| riakcs_cluster_http_port | Yes | 8081 | Defines on which port NGINX will listen for load-balancing. This must not be the same as RiakCS (8080)
| riakcs_cluster_https_port | No | 8082 | Defines on which port NGINX will listen for load-balancing. This must not be the same as RiakCS port (8080)
| riakcs_default_port | Yes | 8080 | RiakCS default port
| nginx_riakcs_server_name | No | None | RiakCS FQDN if you have an upstream DNS configured to point the VIP
| vrrp_riakcs_vip | YES | None | VirtualIP to which the traffic will be directed
| vnet_defined | YES | None | Defines the interface on which RiackCS is listening to gather IP address information of each riakcs_nodes


Configuration and packages variables
------------------------------------

| Parameter | Required | Default | Description
|--- |--- |--- |---
| nginx_core_conf_path | YES | /etc/nginx/nginx.conf | NGINX core configuration file
| nginx_configs_path | YES | /etc/nginx/conf.d/ | NGINX configurations directory
| nginx_riakcs_config_name | YES | riakcs.conf | Name of the NGINX configuration file for RiakCS
| keepalived_configs_dir | YES | /etc/keepalived/ | Keepalived default directory
| keepalived_conf_name | YES | keepalived.conf | Keepalived configuration file name

Dependencies
------------

None. This role can work out of the box but it is preferable to build your RiakCS cluster with the role [JohnPreston.riakcs-cluster](http://github.com/JohnPreston/riakcs-cluster)


Example Playbook
----------------

```

- hosts: riakcs_nodes
  roles:
  - JohnPreston.riakcs-halb
  vars:
  - vrrp_riakcs_vip: 1.2.3.4
  - vnet_defined: em2

```

```

- hosts: riakcs_nodes
  roles:
  - JohnPreston.riakcs-cluster
  - JohnPreston.riaklcs-credentials
  - JohnPreston.riakcs-halb
  vars:
  - vrrp_riakcs_vip: 1.2.3.4
  - vnet_defined: em2

```

License
-------

BSD

Author Information
------------------

John Preston [John Mille]

