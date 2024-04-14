# Задание 1. Создайте конфигурацию для подключения пользователя
- Создайте и подпишите SSL-сертификат для подключения к кластеру.
- Настройте конфигурационный файл kubectl для подключения.
- Создайте роли и все необходимые настройки для пользователя.
- Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (kubectl logs pod <pod_id>, kubectl describe pod <pod_id>).
- Предоставьте манифесты и скриншоты и/или вывод необходимых команд.
```
vagrant@vagrant:~$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
Generating a RSA private key
...................................................................................++++
...................................................................................................................++++
writing new private key to 'key.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ru
State or Province Name (full name) [Some-State]:hmao
Locality Name (eg, city) []:nyagan
Organization Name (eg, company) [Internet Widgits Pty Ltd]:nob
Organizational Unit Name (eg, section) []:it
Common Name (e.g. server FQDN or YOUR name) []:server
Email Address []:elena-volgina@mail.ru
vagrant@vagrant:~$ ls
cert.pem  key.pem  snap


```
