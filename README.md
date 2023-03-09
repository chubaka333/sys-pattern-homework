# Домашнее задание к занятию 13.3. «Защита сети»-Неудахин-Денис

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

При сканирование с параметрами -sA, suricat не отреагировал.
При сканирование с параметрами -sT сурикат указал какие порты были просканированы.
```
03/09/2023-19:10:27.269342  [**] [1:2010937:3] ET SCAN Suspicious inbound to mySQL port 3306 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:50492 -> 192.168.0.103:3306
03/09/2023-19:10:27.408710  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:60406 -> 192.168.0.103:5904
03/09/2023-19:10:27.478664  [**] [1:2010939:3] ET SCAN Suspicious inbound to PostgreSQL port 5432 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:39112 -> 192.168.0.103:5432
03/09/2023-19:10:27.521147  [**] [1:2010936:3] ET SCAN Suspicious inbound to Oracle SQL port 1521 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:60766 -> 192.168.0.103:1521
03/09/2023-19:10:27.785129  [**] [1:2002910:6] ET SCAN Potential VNC Scan 5800-5820 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:50386 -> 192.168.0.103:5815
03/09/2023-19:10:27.792312  [**] [1:2010935:3] ET SCAN Suspicious inbound to MSSQL port 1433 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:36708 -> 192.168.0.103:1433
```
Сурикат так же отреагировал при стелс сканировании показав порты популярных СУБД.

```
03/09/2023-19:16:32.892211  [**] [1:2010937:3] ET SCAN Suspicious inbound to mySQL port 3306 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:3306
03/09/2023-19:16:32.932194  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:5904
03/09/2023-19:16:32.944087  [**] [1:2010935:3] ET SCAN Suspicious inbound to MSSQL port 1433 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:1433
03/09/2023-19:16:32.964052  [**] [1:2010936:3] ET SCAN Suspicious inbound to Oracle SQL port 1521 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:1521
03/09/2023-19:16:33.005205  [**] [1:2010939:3] ET SCAN Suspicious inbound to PostgreSQL port 5432 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:5432
03/09/2023-19:16:33.016030  [**] [1:2002910:6] ET SCAN Potential VNC Scan 5800-5820 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:64879 -> 192.168.0.103:5815
```
При сканировании с опциями -sV логи показывают дополнительно RPC удалённый вызов процедуры.
```
03/09/2023-19:19:28.243109  [**] [1:2010937:3] ET SCAN Suspicious inbound to mySQL port 3306 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:3306
03/09/2023-19:19:28.275526  [**] [1:2010936:3] ET SCAN Suspicious inbound to Oracle SQL port 1521 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:1521
03/09/2023-19:19:28.286386  [**] [1:2010935:3] ET SCAN Suspicious inbound to MSSQL port 1433 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:1433
03/09/2023-19:19:28.302953  [**] [1:2010939:3] ET SCAN Suspicious inbound to PostgreSQL port 5432 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:5432
03/09/2023-19:19:28.310081  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:5901
03/09/2023-19:19:28.358497  [**] [1:2002910:6] ET SCAN Potential VNC Scan 5800-5820 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:50972 -> 192.168.0.103:5800
03/09/2023-19:19:34.535148  [**] [1:2100598:13] GPL RPC portmap listing TCP 111 [**] [Classification: Decode of an RPC Query] [Priority: 2] {TCP} 192.168.0.104:955 -> 192.168.0.103:111
03/09/2023-19:19:34.536842  [**] [1:2100598:13] GPL RPC portmap listing TCP 111 [**] [Classification: Decode of an RPC Query] [Priority: 2] {TCP} 192.168.0.104:955 -> 192.168.0.103:111
```

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*
`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

Сурикат показывает следующие сообщения в логах: происходит сканировании SSH, а так же брутфорсинг перебором юзеров и паролей
```
03/09/2023-20:19:27.675629  [**] [1:2001219:20] ET SCAN Potential SSH Scan [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:49618 -> 192.168.0.103:22
---
03/09/2023-20:19:27.689014  [**] [1:2260002:1] SURICATA Applayer Detect protocol only one direction [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.0.103:22 -> 192.168.0.104:49706
---
03/09/2023-20:19:30.018869  [**] [1:2006546:9] ET SCAN LibSSH Based Frequent SSH Connections Likely BruteForce Attack [**] [Classification: Attempted Administrator Privilege Gain] [Priority: 1] {TCP} 192.168.0.104:49840 -> 192.168.0.103:22
03/09/2023-20:19:30.044242  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.0.104:49868 -> 192.168.0.103:22
```
Fail2ban же показывает множество сообщений о попытке подключений через sshd.
```
2023-03-09 20:19:28,162 fail2ban.filter         [110819]: INFO    [sshd] Found 192.168.0.104 - 2023-03-09 20:19:27
```
В конце показывает, что произошел бан и разбан после некоторого времени.
```
2023-03-09 20:19:32,443 fail2ban.actions        [110819]: NOTICE  [sshd] 192.168.0.104 already banned
2023-03-09 20:29:32,463 fail2ban.actions        [110819]: NOTICE  [sshd] Unban 192.168.0.104
```


