# kubernetes_binaries
put compiled binaries here as a mirror, see files in the release tab.

v1.2.0: at commit: 672d5a777d5df35cc1e74c8075e3c17a20c4c20b


# cloud-config 
cloud-config for install kubernetes on coreos.
## setup kubernetes master
1. use ```etcd2_cc.yaml``` to install etcd2 on a cluster. must configure "name" and discovery field.
1. in kubernetes_master_cc.yaml, change <SSH_PUBLIC_KEY> to your own ssh public key.
1. in kubernetes_master_cc.yaml, change <MY_ETCD_ENDPOINTS> to the etcd cluster endpoints, like ```http:///127.0.0.1:2379```
1. run ```coreos-cloudinit --from-file kubernetes_master_cc.yaml``` to install and start kubernetes master
1. skydns.yaml will be under

## Configure TLS
The master requires the CA certificate, ```ca.pem```; its own certificate, ```apiserver.pem``` and its private key, ```apiserver-key.pem```. This [CoreOS guide](https://coreos.com/kubernetes/docs/latest/openssl.html) explains how to generate these.
1. Generate the necessary certificates for the master. 
1. Send the three files to your master host (using scp for example).
1. Move them to the /etc/kubernetes/ssl folder and ensure that only the root user can read the key:
```
# Move keys
sudo mkdir -p /etc/kubernetes/ssl/
sudo mv -t /etc/kubernetes/ssl/ ca.pem apiserver.pem apiserver-key.pem
    
# Set Permissions
sudo chmod 600 /etc/kubernetes/ssl/apiserver-key.pem
sudo chown root:root /etc/kubernetes/ssl/apiserver-key.pem
```
1. Restart the kubelet to pick up the changes:
```
sudo systemctl restart kubelet
```

## setup worker nodes
Use kubernetes_node_cc.yaml as the cloud config to boot the worker node. Before boot the node, make sure to do the following.
1. ```<HOSTNAME>```: Hostname for this node (e.g. kube-node1, kube-node2)
1. ```<SSH_PUBLIC_KEY>```: The public key you will use for SSH access to this server.
1. ```<KUBERNETES_MASTER>```: The IPv4 address of the Kubernetes master.
1. ```<MY_ETCD_ENDPOINTS>```: Complete etcd endpoints.
1. ```<CA_CERT>```: Complete contents of ca.pem
1. ```<CA_KEY_CERT>```: Complete contents of ca-key.pem

