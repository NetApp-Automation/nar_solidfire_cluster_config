Role Name
=========
nar_solidfire_cluster_config


Description
===========
This role is designed to create a cluster from a set of hosts that are running NetApp Element software on either NetApp SolidFire Enterprise SDS (eSDS) platform or SolidFire all-flash storage (AFA) platform [1].

All hosts must be reachable via the management and storage networks. This role assumes that the entire cluster is represented by the host group identified in the inventory file and will create a single cluster from that host group. The cluster is created using the network addresses, cluster name, cluster credentials, and other details specified in the inventory file.

Once the cluster is successfully created, you can access the cluster management UI from a browser at https://<MVIP>
 
Requirements
============
* This role requires a minimum of four host nodes. 

* To run NetApp SolidFire Element software on eSDS, RHEL OS version 7.6 or later is required.

* This role requires Python version 3.6 or later and Ansible version 2.10 or later installed on the controller.

* This role requires root or superuser privilege to run.

Role Variables
==============

| Variable                   | Required | Default | Description                                                               |
|----------------------------|----------|---------|---------------------------------------------------------------------------|
| MIP                        | yes      | N/A     | The Management interface IP address for the node (host) in the inventory  |
| SIP                        | yes      | N/A     | The Storage interface IP address for the node (host) in the inventory     |
| MVIP                       | yes      | N/A     | The Management Virtual IP address for the cluster                         |
| SVIP                       | yes      | N/A     | The Storage Virtual IP address for the cluster                            |
| sf_cluster_admin_passwd    | yes      | N/A     | The password for the Cluster Administrator user                           |
| sf_cluster_admin_username  | yes      | N/A     | The username for the Cluster Administrator user                           |
| sf_cluster_name            | yes      | N/A     | The name of the cluster being created                                     |
| sf_serialnumber            | yes      | N/A     | Product entitlement Serial Number to supply when creating the cluster [2] |
| sf_ordernumber             | yes      | N/A     | Product entitlement Order Number to supply when creating the cluster [3]  |
| sf_enable_sw_encryption    | yes      | N/A     | Whether to enable software encryption at rest when creating cluster       |
| sf_cluster_connect_timeout | No       | 20      | The API connection timeout value in seconds                               |
| sf_api_version             | No       | 12.2    | The version of the SolidFire eSDS API to use                              |
| sf_use_proxy               | No       | True    | Whether to use proxy settings on the target host, see note [4] below      |
| sf_validate_certs          | No       | False   | Whether to Validate SSL/TLS certificates                                  |

Note
----

[1] Mixing eSDS and AFA nodes in the same cluster is not supported.

[2] sf_serialnumber is not required for a cluster with AFA nodes.

[3] sf_ordernumber is not required for a cluster with AFA nodes.

[4] Setting sf_use_proxy to False might have no effect due to a bug in Ansible version 2.10.x or earlier. Instead, use the
    environment variable "no_proxy" in a playbook as a workaround when needed until the bug is fixed by Ansible. See the example:
```ansible
    environment:
      no_proxy: <target_ip_address>                                                             
```


Example Playbook
----------------
```ansible
- name: Create SolidFire Enterprise SDS cluster
  remote_user: <root or superuer ID>
  gather_facts: True
  hosts: all
  roles:
    - role: nar_solidfire_cluster_config
```

Example Inventory
-----------------
```
all:
  hosts:
    host1:
      ansible_host: <host1 IP>
      MIP: <node management interface IP>
      SIP: <node storage interface IP>
    host2:
      ansible_host: <host2 IP>
      MIP: <node management interface IP>
      SIP: <node storage interface IP>
    host3:
      ansible_host: <host3 IP>
      MIP: <node management interface IP>
      SIP: <node storage interface IP>
    host4:
      ansible_host: <host4 IP>
      MIP: <node management interface IP>
      SIP: <node storage interface IP>
  vars:
    ansible_python_interpreter: <full path on target hosts to a python interpreter, such as /usr/libexec/platform-python>
    become: True
    ansible_password: <login user password> # prefer vault key
    ansible_become_pass: <privilege escalation password> # or --ask-become-pass on command line to prompt for input
    MVIP: <cluster management virtual IP>
    SVIP: <cluster storage virtual IP>
    sf_cluster_name: <cluster name>
    sf_cluster_admin_username: <admin user name>
    sf_cluster_admin_passwd: <admin password>
    sf_serialnumber: <serial number>
    sf_ordernumber: <order number>
    sf_enable_sw_encryption: True
```

License
-------
GPL-3.0-only

Author Information
------------------

NetApp
https://www.netapp.com
