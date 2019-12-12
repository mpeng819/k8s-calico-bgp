# k8s-calico-bgp

Apply the YAML files:

0. calicoctl apply -f bgp.yaml
1. calicoctl apply -f ippool-default.yaml
2. calicoctl apply -f nodes-bgp.yaml
3. calicoctl apply -f nodes-bgppeer.yaml

Check the eBGP status:

1. calicoctl get nodes
2. calicoctl get bgpconfig
3. calicoctl get ippool
4. calicoctl get bgppeer
5. sudo calicoctl node status
