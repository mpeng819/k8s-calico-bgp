# k8s-calico-bgp

Apply the YAML files

0. calicoctl apply -f bgp.yaml
1. calicoctl apply -f ippool-default.yaml
2. calicoctl apply -f nodes-bgp.yaml
3. calicoctl apply -f nodes-bgppeer.yaml

Check the eBGP status

calicoctl get nodes -o wide
calicoctl get bgpconfig -o wide
calicoctl get ippool -o wide
calicoctl get bgppeer

sudo calicoctl node status



