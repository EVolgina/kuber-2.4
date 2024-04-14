# Задание 1. Создайте конфигурацию для подключения пользователя
- Создайте и подпишите SSL-сертификат для подключения к кластеру.
- Настройте конфигурационный файл kubectl для подключения.
- Создайте роли и все необходимые настройки для пользователя.
- Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (kubectl logs pod <pod_id>, kubectl describe pod <pod_id>).
- Предоставьте манифесты и скриншоты и/или вывод необходимых команд.
```
Generating RSA private key, 2048 bit long modulus (2 primes)
.......................................+++++
............................................................................................................................+++++
e is 65537 (0x010001)
vagrant@vagrant:~$ openssl req -new -key sn.key -out sn.csr -subj "/CN=sn/O=group"
vagrant@vagrant:~$  openssl x509 -req -in sn.csr -CA /var/snap/microk8s/6641/certs/ca.crt -CAkey /var/snap/microk8s/6641/certs/ca.key -CAcreateserial -out sn.crt -days 365
Signature ok
subject=CN = sn, O = group
Getting CA Private Key


```
