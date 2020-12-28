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
or
/var/nfsshare *(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)
or
/var/nfsshare 192.168.150.0/24(rw,sync,no_root_squash,no_subtree_check,no_all_squash,insecure)

exportfs -ra
systemctl restart nfs-server
