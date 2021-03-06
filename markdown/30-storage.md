<!-- .slide: data-state="subchapter" id="pool-volumes" data-timing="20s" data-menu-title="Pools and Volumes" -->
# Storage Pools and Volumes


<!-- .slide: data-state="normal" id="libvirt-concepts" data-timing="20s" data-menu-title="Libvirt concepts" -->
# Libvirt Concepts

<div class="breadcrumbs">Introduction / Minor Changes / Pools & Volumes</div>

```xml
<disk type='file' device='disk'>
  <driver name='qemu' type='qcow2'/>
  <source file='/public/vms/myvm'/>
  <target dev='vda' bus='virtio'/>
</disk>
```
<!-- .element class="column fragment" --> 

```xml
<disk type='volume' device='disk'>
  <driver name='qemu' type='qcow2'/>
  <source pool='vms' volume='myvm'/>
  <target dev='vda' bus='virtio'/>
</disk>
```
<!-- .element class="column fragment" --> 

* **File**
  * Contained in a local folder
  * Network FS, Local directory, File System
  * Usual format: qcow2
  * May be contained in a pool

<!-- .element class="column fragment" -->

* **Volume**
  * Contained in a pool
  * partition, LV, Rados or iSCSI volume

<!-- .element class="column fragment" -->

**Pool**: unit containing files or volumes to be used as VM disks
<!-- .element class="fragment" -->

**Driver**: libvirt code handling actual storage actions. One for each pool type.
<!-- .element class="fragment" -->


<!-- .slide: data-state="normal" id="storage-ui" data-timing="20s" data-menu-title="Storage UI" -->
# Storage UI

<div class="breadcrumbs">Introduction / Minor Changes / Pools & Volumes</div>

![Screenshot of the storage page](images/storage-list.png)

Note:

* Explain about refresh
* Only volume delete action. Explain why


<!-- .slide: data-state="normal" id="create-pool-screenshot" data-timing="20s" data-menu-title="Pool Creation Screenshot" -->

![Screenshot of RBD pool creation](images/pool-create-rbd.png)


<!-- .slide: data-state="normal" id="create-pool" data-timing="20s" data-menu-title="Creating a pool" -->
# Creating a Pool

<div class="breadcrumbs">Introduction / Minor Changes / Pools & Volumes</div>

* Handled drivers:
  * `fs`, `dir`, `iscsi`, `scsi`, `logical`, `netfs`, `disk`, `mpath`, `rbd`, `sheepdog`, `gluster`, `iscsi-direct`
  * Handled ≠ L3-supported
  * libvirt < 5.2.0: list generated
```sh
virsh pool-capabilities
```

* Useful libvirt documentation:
    * Properties detailled explanations: https://libvirt.org/formatstorage.html
    * Driver specifics: https://libvirt.org/storage.html


<!-- .slide: data-state="normal" id="salt-states" data-timing="20s" data-menu-title="Salt States" -->
# Salt States

```yaml
nfs_pool:
  virt.pool_running:
    - name: nfs_pool
    - ptype: netfs
    - target: /srv/pools/nfs
    - source:
        format: nfs
        hosts:
          - nas
        dir: /export/documents
```


<!-- .slide: data-state="normal" id="vm-disk" data-timing="20s" data-menu-title="VM Disk Changes" -->
# Virtual Machine Disk Changes

<div class="breadcrumbs">Introduction / Minor Changes / Pools & Volumes / VM Disks</div>

![Volume selection screenshot](images/vm-volume-select.png)

<!-- .element class="col1-large" -->

* Pool started?
* Pool refreshed?
* Some pools can't upload images

<!-- .element class="col2-small" -->

Note:

* Can select volume in pool
* Can select disk format
* No longer file-based


<!-- .slide: data-state="normal" id="vm-disk" data-timing="20s" data-menu-title="CDROM Images" -->
# CDROM Images

<div class="breadcrumbs">Introduction / Minor Changes / Pools & Volumes / VM Disks</div>

* Attaching / Detaching images
![CDROM device details screenshot](images/cdrom-disk.png)

* HTTP, FTP, file
* Live
