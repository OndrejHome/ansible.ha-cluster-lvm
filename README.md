ha-cluster-lvm
=========

Role for enabling Highly Available LVM (HA-LVM) configuration in rgmanager/pacemaker cluster.
This role can also create LVs and VGs for both tagging and clvm variant, check the examples to see how.

Requirements
------------

RHEL: It is expected that machines will already be registered and subscribed for access to 'High Availability' or 'Resilient storage' channels.

Role Variables
--------------

  - type of HA-LVM, possible options are 'tagging' or 'clvm' on EL6 and EL7. On EL8/EL9 the only option is 'systemid'. Default is 'tagging'
  
    ```
    HALVMtype: 'tagging'
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
         - { role: 'ondrejhome.ha-cluster-lvm' }

Example playbook for tagging HA-LVM with extra local VG names 'vg_local'.

    - hosts: servers
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm', local_vg_list: [ 'vg_local' ] }

Example playbook for clvm variant of HA-LVM.

    - hosts: servers
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm', HALVMtype: 'clvm' }

(EL8) Example playbook for systemid variant of HA-LVM.

    - hosts: servers
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm', HALVMtype: 'systemid' }

Example of playbook for tagged variant with one VG and one LV on whole VG.

    - hosts: servers
      vars:
        tagging_vgs:
          - name: 'vg_shared'
            pvs: '/dev/sdb'
            lvs:
              - name: 'lv_name5'
                size: '200M'
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm' }


Example playbook for clvm variant combined with tagging variant and creation of VGs/LVs for both clustered and tagging VGs.

    - hosts: servers
      vars:
        local_vg_list:
          - 'vg_clvm'
        clvm_vgs:
          - name: 'vg_clvm'
            pvs: '/dev/sdb2'
            lvs:
              - name: 'lv_name1'
                size: '200M'
              - name: 'lv_name2'
                size: '100M'
        tagging_vgs:
          - name: 'vg_shared'
            pvs: '/dev/sdb1'
            lvs:
              - name: 'lv_name3'
                size: '200M'
              - name: 'lv_name4'
                size: '100M'
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm', HALVMtype: 'clvm' }

Example of playbook for systemid variant on EL8 with one VG and one LV on whole VG.

    - hosts: servers
      vars:
        systemid_vgs:
          - name: 'vg_systemid'
            pvs: '/dev/sdb'
            lvs:
              - name: 'lv_name5'
                size: '200M'
      roles:
         - { role: 'ondrejhome.ha-cluster-lvm', HALVMtype: 'systemid }

License
-------

GPLv3

Author Information
------------------

WARNING: While this was tested on newest CentOS/RHEL 6.9 and 7.4 and is used by
  the Author regularly, you should be careful when using this. In worst case the
  wrong detection of root VG may fail and cause issues.

Additionally tested on: RHEL 8.2/8.6/9.0, AlmaLinux 8.6/9.0 with systemid approach

To get in touch with author you can use email ondrej-xa2iel8u@famera.cz or create a issue on github when requesting some feature.
