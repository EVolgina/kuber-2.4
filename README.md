# Задание 1. Создайте конфигурацию для подключения пользователя
- Создайте и подпишите SSL-сертификат для подключения к кластеру.
- Настройте конфигурационный файл kubectl для подключения.
- Создайте роли и все необходимые настройки для пользователя.
- Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (kubectl logs pod <pod_id>, kubectl describe pod <pod_id>).
- Предоставьте манифесты и скриншоты и/или вывод необходимых команд.
```
vagrant@vagrant:~$ openssl genrsa -out sof.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
............................................................................................................................................................................+++++
....+++++
e is 65537 (0x010001)
vagrant@vagrant:~$ openssl req -new -key sof.key -out sof.csr -subj "/CN=sof/O=group"
vagrant@vagrant:~$ openssl x509 -req -in sof.csr -CA /var/snap/microk8s/6641/certs/ca.crt -CAkey /var/snap/microk8s/6641/certs/ca.key -CAcreateserial -out sof.crt -days
 365
Signature ok
subject=CN = sof, O = group
Getting CA Private Key
vagrant@vagrant:~$ kubectl config set-context sof-context --cluster=my-cluster --user=sof
Context "sof-context" created.
vagrant@vagrant:~$ export KUBECONFIG=/home/vagrant/kubeconfig.yaml
vagrant@vagrant:~$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/vagrant/cert.pem
    server: https://10.0.2.15:16443
  name: my-cluster
contexts:
- context:
    cluster: my-cluster
    user: my-user
  name: my-context
current-context: my-context
kind: Config
preferences: {}
users:
- name: my-user
  user:
    client-certificate: /home/vagrant/cert.pem
    client-key: /home/vagrant/key.pem


```
