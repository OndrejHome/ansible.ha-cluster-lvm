ha-cluster-lvm
=========

Role for enabling Highly Available LVM (HA-LVM) configuration in rgmanager/pacemaker cluster.
Note: previous versions of this role were creating shared VGs, newer version don't do that anymore.

Requirements
------------

RHEL: It is expected that machines will already be registered and subscribed for access to 'High Availability' or 'Resilient storage' channels.

Role Variables
--------------

  - type of HA-LVM, possible options are 'tagging' or 'clvm'. Default is 'tagging'
  
    ```
    HALVMtype: "tagging"
    ```

  - (optional) - this is applied only when 'tagging' mode is selected. List of VGs that should be included in 'volume_list' additionally to VG on which root filesystem resides. By default this list is empty, below example shows how to specify the list of VGs.
  
    ```
    local_vg_list: [ 'vg1', 'vg2' ]
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

Example playbook for tagging HA-LVM.

    - hosts: servers
      roles:
         - { role: "OndrejHome.ha-cluster-lvm" }

Example playbook for tagging HA-LVM with extra local VG names 'vg_local'.

    - hosts: servers
      roles:
         - { role: "OndrejHome.ha-cluster-lvm", local_vg_list: [ 'vg_local' ] }

Example playbook for clvm variant of HA-LVM.

    - hosts: servers
      roles:
         - { role: "OndrejHome.ha-cluster-lvm", HALVMtype: "clvm" }

License
-------

GPLv3

Author Information
------------------

WARNING: this is alpha-version quality proof-of-concept role that still needs some polishing. 
         This is suitable for testing purposes only.

To get in touch with author you can use email ondrej-xa2iel8u@famera.cz or create a issue on github when requesting some feature.
