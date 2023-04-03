# devops-31-diploma
\
Инструкция по развертыванию инфраструктуры.
---

- Войдите в каталог ./IaC-terraform/
```
  $ terraform init
  $ terraform plan -out=tfplan -input=false
  $ terraform apply -input=false tfplan
```
- Затем, используя ключ, переданный через ./IaC-terraform/users.yaml
подключиться по ssh к NAT-устройству. Все действующие IP-адреса 
содержатся в ./current_adresses.out
```
  $ ssh X.X.X.X
  $ sudo apt update
  $ sudo apt install git pip -y
  $ git clone https://github.com/kubernetes-sigs/kubespray.git
  $ cd kubespray
  $ sudo pip install -r requirements.txt
``` 
- Разворачиваем кластер Kubernetes на рабочих машинах используя
информацию из файла IaC/current_adresses.out
```
  $ cp -rfp inventory/sample inventory/diplomacluster
  $ declare -a IPS=(Y.Y.Y.Y Z.Z.Z.Z)
  $ CONFIG_FILE=inventory/diplomacluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
  $ ansible-playbook -i inventory/diplomacluster/hosts.yaml  --become --become-user=root cluster.yml
```
