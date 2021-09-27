Role Name
=========
nar_solidfire_cluster_config


Description
-----------
This role creates a cluster from a set of NetApp SolidFire Enterprise SDS nodes. We recommend this role only be run on hosts 
that have eSDS nodes installed and reachable via the management and storage networks. This role assumes that the entire 
cluster is represented by the inventory host-group identified and will create a single cluster from that inventory/host 
group. The cluster is created using the network addresses, cluster name, cluster credentials, and other details 
specified in the inventory file.

Once the cluster is successfully created, you can access the cluster management UI from a browser 
at https://<MVIP_ip_address>
 
Requirements
------------
* This role requires a minimum of four nodes with eSDS installed and running on hosts running RHEL 7.6 or later.

* This role requires Python version 3.6 or later and Ansible version 2.10 or later.

Role Variables
--------------

| Variable                   | Required | Default | Description                                                                   |
|----------------------------|----------|---------|-------------------------------------------------------------------------------|
| MIP                        | yes      | N/A     | The Management interface IP address for the node (host) in the inventory      |
| SIP                        | yes      | N/A     | The Storage interface IP address for the node (host) in the inventory         |
| MVIP                       | yes      | N/A     | The Management Virtual IP address for the cluster                             |
| SVIP                       | yes      | N/A     | The Storage Virtual IP address for the cluster                                |
| sf_cluster_admin_passwd    | yes      | N/A     | The password for the Cluster Administrator user                               |
| sf_cluster_admin_username  | yes      | N/A     | The username for the Cluster Administrator user                               |
| sf_serialnumber            | yes      | N/A     | Product entitlement Serial Number to supply when creating the cluster         |
| sf_ordernumber             | yes      | N/A     | Product entitlement Order Number to supply when creating the cluster          |
| sf_cluster_name            | yes      | N/A     | The name of the cluster being created                                         |
| sf_cluster_connect_timeout | No       | 20      | The API connection timeout value in seconds                                   |
| sf_api_version             | No       | 12.2    | The version of the SolidFire eSDS API to use and should not be modified!      |
| sf_use_proxy               | No       | True    | Whether to use proxy settings on the target host (or not). See note [1] below |
| sf_validate_certs          | No       | True    | Check SSL/TLS certificates                                                    |

Note:

[1] Setting sf_use_proxy to False might have no effect due to a bug in Ansible version 2.10.x or earlier. Instead, use the
    environment variable "no_proxy" in a playbook as a workaround when needed until the bug is fixed by Ansible. See the example:
```ansible
    environment:
      no_proxy: <target_ip_address>                                                             
```


Example Playbook
----------------
```ansible
- name: Create cluster for SolidFire Enterprise SDS
  hosts: cluster1
  gather_facts: True

  roles:
    - role: nar_solidfire_cluster_config
```

License
-------
GPL-3.0-only

Author Information
------------------

NetApp
https://www.netapp.com
