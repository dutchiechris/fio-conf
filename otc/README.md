== Files for testing ONTAP Cloud ==
1. Become root using `sudo -i`
1. Install fio
    ```
    apt-get install -y fio
    ```
1. On linux host add fstab entry like:
    ```
    10.31.1.25:/    /mnt   nfs     soft,rsize=65536,wsize=65536,nfsvers=3 0 0
    ```
1. Make mount point `mkdir /mnt`
1. Use `mount -a` to mount
1. On NetApp create 4 volumes like
    ```
    volume create -vserver svm_okATOS02 -volume okATOSvol01 -aggregate aggr1 -size 1t -junction-path /okATOSvol01 -snapshot-policy none -percent-snapshot-space 0 -policy export-svm_okATOS02 -space-guarantee none 
    volume create -vserver svm_okATOS02 -volume okATOSvol02 -aggregate aggr1 -size 1t -junction-path /okATOSvol02 -snapshot-policy none -percent-snapshot-space 0 -policy export-svm_okATOS02 -space-guarantee none 
    volume create -vserver svm_okATOS02 -volume okATOSvol03 -aggregate aggr1 -size 1t -junction-path /okATOSvol03 -snapshot-policy none -percent-snapshot-space 0 -policy export-svm_okATOS02 -space-guarantee none 
    volume create -vserver svm_okATOS02 -volume okATOSvol04 -aggregate aggr1 -size 1t -junction-path /okATOSvol04 -snapshot-policy none -percent-snapshot-space 0 -policy export-svm_okATOS02 -space-guarantee none 
    ```
1. On NetApp create dummy files like
    ```
    > set  -priv diag
    > node run -node okATOS01-01 mkfile 100g /vol/okATOSvol01/100g
    > node run -node okATOS01-01 mkfile 100g /vol/okATOSvol02/100g
    > node run -node okATOS01-01 mkfile 100g /vol/okATOSvol03/100g
    > node run -node okATOS01-01 mkfile 100g /vol/okATOSvol04/100g
    ```
1. Run workload profiles like:
    ```
    fio nfs-rnd-read.conf
    ```

