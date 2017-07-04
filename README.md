ha-cluster-lvm
=========

Role for configuring Highly Available LVM (HA-LVM) in rgmanager/pacemaker cluster

Requirements
------------

RHEL: It is expected that machines will already be registered and subscribed for access to 'High Availability' or 'Resilient storage' channels.
CentOS 5.xx: requires installation of python-simplejson.x86_64 package prior to ansible run.

Role Variables
--------------

  - path to device shared between nodes, the path must be same on both nodes

    ```
    shared_drive: '/dev/disk/by-path/ip-192.168.34.153:3260-iscsi-iqn.1994-05.com.redhat:target-lun-0'
    ```

  - name of VG that will be clustered between nodes
  
    ```
    cluster_vg_name: "cluster_vg"
    ```

  - type of HA-LVM, possible options are 'tagging' or 'clvm'. Default is 'tagging'
  
    ```
    HALVMtype: "tagging"
    ```

  - list of VGs enclosed in '"' and separated by ','. This will become part of 'volume_list' variable in /etc/lvm/lvm.conf and must end with ','!. NOTE: this setting is ignored whe the HALVMtype is 'clvm' as the 'volume_list' gets commented out.
  
    ```
    local_volume_list: '"c6vg",'
    ```

  - (EL6 with tagging) type of running cluster is required to determine correct volume_list. Only 2 possible options are 'pacemaker' and 'rgmanager'. Default is unset.
    ```
    cluster_type: 'rgmanager'
    ```

  - (RHEL only) enable the repositories containint packages needed
    ```
    enable_repos: true
    ```

  - (RHEL only) enable the extended update (EUS) repositories containint packages needed
    ```
    enable_eus_repos: false
    ```


Example Playbook
----------------

Example playbook for tagging HA-LVM with VG name 'cluster_vg' and with single local VG named 'c6vg'.

    - hosts: servers
      roles:
         - { role: "OndrejHome.ha-cluster-lvm", HALVMtype: "tagging", local_volume_list: '"c6vg"', cluster_vg_name: "cluster_vg", shared_drive: '/dev/disk/by-path/ip-192.168.34.153:3260-iscsi-iqn.1994-05.com.redhat:target-lun-0' } 

Example playbook for clvm variant of HA-LVM.

    - hosts: servers
      roles:
         - { role: "OndrejHome.ha-cluster-lvm", HALVMtype: "clvm", cluster_vg_name: "cluster_vg", shared_drive: '/dev/disk/by-path/ip-192.168.34.153:3260-iscsi-iqn.1994-05.com.redhat:target-lun-0' }

License
-------

GPLv3

Author Information
------------------

WARNING: this is alpha-version quality proof-of-concept role that still needs some polishing. 
         This is suitable for testing purposes only.

To get in touch with author you can use email ondrej-xa2iel8u@famera.cz or create a issue on github when requesting some feature.
