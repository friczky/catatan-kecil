### Kubernetes

#### Server

1. 34.101.75.33 - master/bsm : ok
2. 34.101.82.55 - node1/bsm : ok
3. 34.101.233.203 - node2/bsm : ok

#### Install Alat Tempur di master

1. sudo apt update
2. ssh-keygen
3. ssh-copy-id root@nodename
4. cd ~
5. apt install python3-pip
6. git clone https://github.com/kubernetes-sigs/kubespray.github
7. cd kubespray
8. pip3 install -r requirements.txt
9. cp -rfp inventory/sample inventory/mycluster
10. declare -a IPS=(34.101.75.33 34.101.82.55 34.101.233.203)
10. CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
11. nano inventory/mycluster/hosts.yaml
12. ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml
13. kubectl get nodes -o wide
