# k8s + TrueNAS Scale
[democratic-csi](https://github.com/democratic-csi/democratic-csi) based simple guide to use Kubernetes cluster with TrueNAS Scale over API. 

You can use democratic-csi documentation and achieve the same results but the reason I created this guide is the fact that democratic-csi docs are covering multiple awkward combinations of various technologies and if you just want to have NFS/iSCSI over API then the whole setup guide can be much simpler.

# Prerequisities
* k8s cluster - in my case deployed using [kubespray](https://kubespray.io) but it shouldn't really matter what you use to create it.
* TrueNAS Scale - in my case it's [ugly-nas](https://github.com/fenio/ugly-nas) 

# Preparations

## Nodes
All your nodes should be capable of using NFS/iSCSI. In my case it's being handled by [dumb-provisioner](https://github.com/fenio/dumb-provisioner) but in general you just have to run this on every node:
```
apt install nfs-common open-iscsi multipath-tools scsitools lsscsi
cat <<EOF > /etc/multipath.conf
defaults {
    user_friendly_names yes
    find_multipaths yes
}
EOF
```
