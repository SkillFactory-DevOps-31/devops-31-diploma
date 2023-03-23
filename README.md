# devops-31-diploma
\
Инструкция по развертыванию нифраструктуры.

Войдите в каталог IaC/terraform/

  $ terraform init
  $ terraform plan -out=tfplan -input=false
  $ terraform apply -input=false tfplan
  
Затем, используя ключ, переданный через IaC/terraform/users.yaml
подключиться по ssh к NAT-устройству. Все действующие IP-адреса 
содержатся в IaC/current_adresses.out

  $ ssh X.X.X.X
  $ sudo apt install ansible git -y
  $ git clone https://github.com/kubernetes-sigs/kubespray.git
  
Разворачиваем кластер Kubernetes на рабочих машинах используя
информацию из файла IaC/current_adresses.out

  $ cp -rfp inventory/sample inventory/diplomacluster
  $ declare -a IPS=(Y.Y.Y.Y Z.Z.Z.Z)
  $ 
