# Поддомены и DNS-сервер

## Поддомены 4-го уровня и глубже

Уровни читаются справа налево. Точка в самом конце - корень, домен нулевого уровня. В имени её обычно не пишут.

Имя `pc1.lab.dept.university.local.`

| Часть | Уровень |
|---|---|
| . (корень) | 0 |
| local | 1, зона верхнего уровня |
| university | 2, основной домен |
| dept | 3, поддомен |
| lab | 4, поддомен |
| pc1 | 5, поддомен |

Примеры:

- 4 уровня: `lab.dept.university.local`
- 5 уровней: `pc1.lab.dept.university.local`
- 6 уровней: `eth0.pc1.lab.dept.university.local`

Четвертый уровень и дальше в обычных сайтах почти не используют: имя становится длинным. В локальной сети так удобно разложить здания, отделы и компьютеры.

## Настройки сервера

Зона `university.local`, сервер первичный. Адреса из сети `192.168.10.0/24`.

Какая зона обслуживается:

```text
zone "university.local" {
    type master;
    file "db.university.local";
};
```

Записи зоны:

```text
$TTL 300
@   IN SOA ns1.university.local. admin.university.local. (
        2026101501 ; serial
        3600       ; refresh
        600        ; retry
        604800     ; expire
        300        ; minimum ttl
)

@       IN NS   ns1.university.local.

ns1     IN A    192.168.10.10
@       IN A    192.168.10.20
www     IN CNAME university.local.
mail    IN A    192.168.10.25
@       IN MX 10 mail.university.local.

; 3-й уровень
dept    IN A    192.168.10.30

; 4-й уровень: lab.dept.university.local
lab.dept        IN A 192.168.10.40

; 5-й уровень: pc1.lab.dept.university.local
pc1.lab.dept    IN A 192.168.10.41

; 6-й уровень: eth0.pc1.lab.dept.university.local
eth0.pc1.lab.dept IN A 192.168.10.42
```

В SOA почта администратора записана через точку: `admin.university.local`. Символ `@` в почте здесь использовать нельзя, он уже занят именем самой зоны.

Что означают записи:

- SOA - параметры зоны: серийный номер, refresh, retry, expire и TTL
- NS - имя сервера этой зоны, `ns1.university.local`
- A - адрес узла
- CNAME - `www` указывает на тот же адрес, что и сам домен
- MX - почта домена идет на `mail.university.local`
