# Домашнее задание к занятию "10.5 Балансировка нагрузки. HAProxy/Nginx." - Неудахин Денис

---

### Задание 1.

Что такое балансировка нагрузки и зачем она нужна? 

*Приведите ответ в свободной форме.*

Это процесс распределения нагрузки на пул серверов для оптимизации ресурсов, сокращения времени обслуживания запросов, горизонтальное масштабирование кластера, отказойстойчивости.

---

### Задание 2.

Чем отличаются между собой алгоритмы балансировки round robin и weighted round robin? В каких случаях каждый из них лучше применять? 

*Приведите ответ в свободной форме.*

Round Robin – занимается переборов по круговому циклу, первый запрос передаётся одному серверу, следующий передаётся другому и так до достижения последнего сервера.
Weighted Round Robin – тот же Round Robin но усовершенствованный. Каждому серверу присваивается коэффициент в соответствии с его производительностью и мощность. Это помогает распределять нагрузку более гибко: серверы с большим весом обрабатывают больше запросов.
В первом случае целесообразнее применение при наличии большого количества одинаковых серверов. Второй алгоритм более уместен при наличии серверов разной мощности.

---

### Задание 3.

Установите и запустите haproxy.

*Приведите скриншот systemctl status haproxy, где будет видно, что haproxy запущен.*

<img src = "img16.png" width = 100%>

---

### Задание 4.

Установите и запустите nginx.

*Приведите скриншот systemctl status nginx, где будет видно, что nginx запущен.*

<img src = "10.5-3.PNG" width = 100%>

---

### Задание 5.

Настройте nginx на виртуальной машине таким образом, чтобы при запросе:

`curl http://localhost:8088/ping`

он возвращал в ответе строчку: 

"nginx is configured correctly"

*Приведите скриншот получившейся конфигурации.*

```
server {
listen 8080;

location /ping{
return 200 'nginx is configured correctly';

}

}
```

---
