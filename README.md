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


vagrant@k8s-node1:~$ sudo calicoctl node status
Calico process is running.
IPv4 BGP status
+--------------+---------------+-------+----------+-------------+
| PEER ADDRESS |   PEER TYPE   | STATE |  SINCE   |    INFO     |
+--------------+---------------+-------+----------+-------------+
| 10.0.0.8     | node specific | up    | 03:04:59 | Established |
+--------------+---------------+-------+----------+-------------+
IPv6 BGP status
No IPv6 peers found.

vagrant@k8s-node1:~$ ip route
default via 172.16.10.1 dev ens3 proto dhcp src 172.16.10.41 metric 100 
10.0.0.0/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.2/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.4/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.6/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.8/31 dev ens4 proto kernel scope link src 10.0.0.9 
10.0.0.10/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.12/31 via 10.0.0.8 dev ens4 proto bird 
10.0.0.14/31 via 10.0.0.8 dev ens4 proto bird 
172.16.10.0/24 dev ens3 proto kernel scope link src 172.16.10.41 
172.16.10.1 dev ens3 proto dhcp scope link src 172.16.10.41 metric 100 
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown 
192.168.0.0 via 10.0.0.8 dev ens4 proto bird 
192.168.0.1 via 10.0.0.8 dev ens4 proto bird 
192.168.0.2 via 10.0.0.8 dev ens4 proto bird 
192.168.0.3 via 10.0.0.8 dev ens4 proto bird 
blackhole 192.168.36.64/26 proto bird 
192.168.107.192/26 via 10.0.0.8 dev ens4 proto bird 
192.168.122.64/26 via 10.0.0.8 dev ens4 proto bird 
192.168.169.128/26 via 10.0.0.8 dev ens4 proto bird 
