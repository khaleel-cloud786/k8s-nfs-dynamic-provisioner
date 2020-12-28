# k8s-nfs-dynamic-provisioner
## Setup NFS Server

yum install nfs-utils

mkdir /var/nfsshare
chmod -R 755 /var/nfsshare
chown nfsnobody:nfsnobody /var/nfsshare

systemctl enable rpcbind; systemctl enable nfs-server; systemctl enable nfs-lock; systemctl enable nfs-idmap
systemctl start rpcbind; systemctl start nfs-server; systemctl start nfs-lock; systemctl start nfs-idmap

vim /etc/exports
/var/nfsshare    192.168.150.101(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)
### or
/var/nfsshare *(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)
### or
/var/nfsshare 192.168.150.0/24(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)

exportfs -ra
systemctl restart nfs-server

################################################

## Deploy Client Provisioner

kubectl apply -f nfs-client-provisioner-rbac.yaml

kubectl get clusterrole,role | grep -i nfs

kubectl create -f nfs-client-provisioner-storageclass.yaml

kubectl get storageclass

kubectl apply -f nfs-client-provisioner-deployment.yaml

kubectl describe po nfs-pod-provisioner-66ffbbbbf-sg4kh

kubectl get pv,pvc

kubectl apply -f  pvc-nfs.yaml

ls /var/nfsshare

default-nfs-pvc-test-pvc-620ff5b1-b2df-11e9-a66a-080027db98ca


kubectl apply -f nfs-nginx.yaml

kubectl get po 

kubectl exec -it po nfs-nginx-76c48f6466-fnkh9 bash

cd /usr/share/nginx/html
### touch testfile.txt

ls /var/nfsshare/default-nfs-pvc-test-pvc-620ff5b1-b2df-11e9-a66a-080027db98ca/

testfile.txt
