# k8s + TrueNAS Scale using democratic-csi
[democratic-csi](https://github.com/democratic-csi/democratic-csi) based simple guide to use Kubernetes cluster with TrueNAS Scale over API. 

You can use democratic-csi documentation and achieve the same results but the reason I created this guide is the fact that democratic-csi docs are covering multiple awkward combinations of various technologies and if you just want to have NFS/iSCSI over API then the whole setup guide can be much simpler.

# Prerequisities
* k8s cluster - in my case deployed using [kubespray](https://kubespray.io) but it shouldn't really matter what you use to create it.
* NAS based on TrueNAS Scale - in my case it's [ugly-nas](https://github.com/fenio/ugly-nas) 

# Preparations

* Nodes section needs to be run on nodes of your cluster, preferably Debian based
* NAS section needs to be done on your NAS and assumption is that this is TrueNAS Scale based solution
* K8S section needs to be run on whatever machine you're using to manage your cluster

## Nodes
All your nodes should be capable of using NFS/iSCSI. In my case it's being handled by [dumb-provisioner](https://github.com/fenio/dumb-provisioner) but in general you just have to run this on every node:
```
# apt install nfs-common open-iscsi multipath-tools scsitools lsscsi
# cat <<EOF > /etc/multipath.conf
defaults {
    user_friendly_names yes
    find_multipaths yes
}
EOF
```
Slash at the beginning of lines means you have to run them as root. So in case you're on Ubuntu just add sudo in front of these commands.
In case of Fedora/RedHat based systems something like that should also work:
```
# dnf install -y lsscsi iscsi-initiator-utils sg3_utils device-mapper-multipath
```

## NAS 

Need to make screenshots.

## K8S
Let's start with adding repo.
```
helm repo add democratic-csi https://democratic-csi.github.io/charts/
helm repo update
```

```
helm upgrade --install --create-namespace --values <EXPLAINED BELOW> --namespace storage iscsi democratic-csi/democratic-csi
```

Creating files with values.

# NFS
```
wget https://raw.githubusercontent.com/democratic-csi/charts/master/stable/democratic-csi/examples/freenas-nfs.yaml -O - | sed '/INLINE/,$d' > nfs.yaml
wget https://raw.githubusercontent.com/democratic-csi/democratic-csi/master/examples/freenas-api-nfs.yaml | sed -e 's/^/    /g' >> nfs.yaml
```

# iSCSI
```
wget https://raw.githubusercontent.com/democratic-csi/charts/master/stable/democratic-csi/examples/freenas-iscsi.yaml -O - | sed '/INLINE/,$d' > nfs.yaml
wget https://raw.githubusercontent.com/democratic-csi/democratic-csi/master/examples/freenas-api-iscsi.yaml | sed -e 's/^/    /g' >> nfs.yaml
```
