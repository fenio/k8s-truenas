# k8s + TrueNAS Scale
[democratic-csi](https://github.com/democratic-csi/democratic-csi) based simple guide to use Kubernetes cluster with TrueNAS Scale over API. 

You can use democratic-csi documentation and achieve the same results but the reason I created this guide is the fact that democratic-csi docs are covering multiple awkward combinations of various technologies and if you just want to have NFS/iSCSI over API then the whole setup guide can be much simpler.

# Prerequisities
* k8s cluster - in my case deployed using [kubespray](https://kubespray.io) but it shouldn't really matter
* TrueNAS Scale - in my case it's [ugly-nas](https://github.com/fenio/ugly-nas)

# Preparations

## Nodes
All your nodes should be capable of using NFS/iSCSI 
