# Задание 1. Создайте конфигурацию для подключения пользователя
- Создайте и подпишите SSL-сертификат для подключения к кластеру.
- Настройте конфигурационный файл kubectl для подключения.
- Создайте роли и все необходимые настройки для пользователя.
- Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (kubectl logs pod <pod_id>, kubectl describe pod <pod_id>).
- Предоставьте манифесты и скриншоты и/или вывод необходимых команд.
```
vagrant@vagrant:~/rb$ sudo snap install kubectl --classic
kubectl 1.29.4 from Canonical✓ installed
vagrant@vagrant:~/rb$ kubectl apply -f role.yaml
role.rbac.authorization.k8s.io/pod-reader created
vagrant@vagrant:~/rb$ kubectl apply -f role-bin.yaml
rolebinding.rbac.authorization.k8s.io/read-pods created
vagrant@vagrant:~$ sudo mkdir cert && cd cert
vagrant@vagrant:~/cert$ sudo openssl genrsa -out user1.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
.............................+++++
...+++++
e is 65537 (0x010001)
vagrant@vagrant:~/cert$ sudo openssl req -new -key user1.key -out user1.csr -subj "/CN=user1/O=group1"
vagrant@vagrant:~/cert$ sudo  openssl x509 -req -in user1.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out user1.crt -days 500
Signature ok
subject=CN = user1, O = group1
Getting CA Private Key

vagrant@vagrant:~/cert$ kubectl config set-credentials user1 --client-certificate=user1.crt --client-key=user1.key
User "user1" set.
vagrant@vagrant:~/cert$ kubectl config set-context user1-context --cluster=minikube --user=user1
Context "user1-context" created.
```
```
vagrant@vagrant:~/cert$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/vagrant/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Sat, 20 Apr 2024 06:54:14 UTC
        provider: minikube.sigs.k8s.io
        version: v1.33.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Sat, 20 Apr 2024 06:54:14 UTC
        provider: minikube.sigs.k8s.io
        version: v1.33.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
- context:
    cluster: minikube
    user: user1
  name: user1-context
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/vagrant/.minikube/profiles/minikube/client.crt
    client-key: /home/vagrant/.minikube/profiles/minikube/client.key
- name: user1
  user:
    client-certificate: /home/vagrant/cert/user1.crt
    client-key: /home/vagrant/cert/user1.key
```
```
vagrant@vagrant:~/cert$ kubectl config use-context user1-context
Switched to context "user1-context".
vagrant@vagrant:~/cert$ kubectl config current-context
user1-context
vagrant@vagrant:~/cert$ kubectl config use-context minikube
Switched to context "minikube".
vagrant@vagrant:~/rb$ kubectl apply -f role.yaml
role.rbac.authorization.k8s.io/pod-reader unchanged
vagrant@vagrant:~/rb$ kubectl apply -f role-bin.yaml
rolebinding.rbac.authorization.k8s.io/read-pods unchanged
vagrant@vagrant:~/rb$  kubectl get roles
NAME         CREATED AT
pod-reader   2024-04-20T07:00:33Z
vagrant@vagrant:~/rb$ kubectl get rolebindings
NAME        ROLE              AGE
read-pods   Role/pod-reader   16m
```
```
```
