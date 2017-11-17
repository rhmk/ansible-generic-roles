
This is how the variable declaration for disk-setup look like

~~~

force_disk_delete: false

disks:
  /dev/vdc: vg00
  /dev/vdb: vg01

logvols:
  hana_shared:
    size: 24G
    vol: vg00
    mountpoint: /hana/shared
    pvs: /dev/vdc1 (not necessary)
    fs: xfs
  hana_data:
    size: 24G
    vol: vg00
    mountpoint: /hana/data
    pvs: /dev/vdc1
    fs: xfs
  hana_logs:
    size: 12G
    vol: vg00
    mountpoint: /hana/logs
    pvs: /dev/vdc1
    fs: xfs
  install:
    size: 128G
    vol: vg00
    mountpoint: /install
    pvs: /dev/vdc1
    fs: ext4
  usr_sap:
    size: 49G
    vol: vg01
    mountpoint: /usr/sap
    pvs: /dev/vdb1
    fs: ext4


~~~
