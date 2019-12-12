# k8s-calico-bgp

Apply the YAML files

0. calicoctl apply -f bgp.yaml
1. calicoctl apply -f ippool-default.yaml
2. calicoctl apply -f nodes-bgp.yaml
3. calicoctl apply -f nodes-bgppeer.yaml

Check the eBGP status
 
 vagrant@k8s-master:~$ calicoctl get nodes -o wide
 NAME         ASN       IPV4              IPV6   
k8s-master   (63400)   172.16.10.40/24          
k8s-node1    64516     10.0.0.9/31              
k8s-node2    64517     10.0.0.11/31             
k8s-node3    64518     10.0.0.13/31             
k8s-node4    64519     10.0.0.15/31             

 
 vagrant@k8s-master:~$ calicoctl get bgpconfig -o wide
 NAME      LOGSEVERITY   MESHENABLED   ASNUMBER   
default   Info          false         63400    

vagrant@k8s-master:~$ calicoctl get ippool -o wide
NAME                  CIDR             NAT    IPIPMODE   VXLANMODE   DISABLED   SELECTOR   
default-ipv4-ippool   192.168.0.0/16   true   Never      Never       false      all()      

vagrant@k8s-master:~$ calicoctl get bgppeer
NAME         PEERIP      NODE        ASN     
node1.ebgp   10.0.0.8    k8s-node1   64514   
node2.ebgp   10.0.0.10   k8s-node2   64514   
node3.ebgp   10.0.0.12   k8s-node3   64515   
node4.ebgp   10.0.0.14   k8s-node4   64515   


