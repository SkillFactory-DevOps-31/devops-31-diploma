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
- Делаем проброс портов на Kubernetes API сервер. Для этого клонируем на NAT-инстанс https://github.com/kodxxl/devops-31-shellscripts.git редактируем и запускаем firewall-forward-to-kapi.sh

Инструкция по деплою приложения в кластер Kubernetes
---

- На узле созданного кластера запускаем скрипт create_role.sh из репозитория https://github.com/kodxxl/devops-31-shellscripts.git
- Создаем GitHub Actions Secrets для репозитория 
```
CLIENT_CERT_DATA - Открытый сертификат клиента Kubernetes
CLIENT_KEY_DATA - Закрытый ключ клиента Kubernetes
CLUSTER_CA - Сертификат кластера
```
- Для рапуска HELM-чарта Postgress необходимо отредактировать values.yaml в папке helm-charts/postgresql/ и создать коммит в ветку pg-deploy
- Для развертывания самого приложения отредактировать манифесты в helm-charts/app/ и сделать коммит в ветку app-deploy
