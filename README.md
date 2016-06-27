# kubernetes_binaries
put compiled binaries here as a mirror, see files in the release tab.

v1.2.0: at commit: 672d5a777d5df35cc1e74c8075e3c17a20c4c20b


# cloud-config 
cloud-config for install kubernetes on coreos.
1. use ```etcd2_cc.yaml``` to install etcd2 on a cluster. must configure "name" and discovery field.
1. in kubernetes_master_cc.yaml, change <SSH_PUBLIC_KEY> to your own ssh public key.
1. in kubernetes_master_cc.yaml, change <MY_ETCD_ENDPOINTS> to the etcd cluster endpoints, like ```http:///127.0.0.1:2379```
