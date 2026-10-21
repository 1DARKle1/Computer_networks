# HTTP

## Определения

**HTTP** - HyperText Transfer Protocol, протокол передачи гипертекста. По нему браузер запрашивает у сервера страницы, картинки и данные и получает ответ.

**HTTPS** - тот же HTTP, но поверх шифрования TLS. Содержимое запроса и ответа по пути не прочитать, клиент проверяет сертификат сервера. Обычный HTTP идет в открытом виде, чаще всего на порту 80, HTTPS на порту 443.

**Клиент-серверная модель** - роли разделены. Клиент сам начинает обмен и посылает запрос. Сервер ждет запросы и возвращает ответ. Браузер - клиент, сайт - сервер.

**Идемпотентность** - повтор одного и того же запроса не меняет результат сверх первого раза. GET можно вызывать сколько угодно: данные только читаются. PUT ставит одно и то же состояние, DELETE повторно удаляет уже удаленное. POST не идемпотентен: каждый вызов может создать еще один объект.

## Разбор запроса

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Length: 58

{"name":"Иван","email":"ivan@mail.com"}
```

- Метод: POST. Клиент просит создать ресурс.
- Путь: /api/users.
- Версия: HTTP/1.1.
- Content-Type: application/json. Тело записано в формате JSON.
- Authorization: Bearer и дальше токен. Клиент показывает серверу, от чьего имени идет запрос.
- Тело запроса стоит после пустой строки. Это JSON с именем и почтой: Иван, ivan@mail.com.

## Разбор ответа

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/users/42

{"id":42,"name":"Иван","email":"ivan@mail.com"}
```

- Код 201 Created значит, что ресурс создан.
- Location: /api/users/42 показывает адрес нового пользователя.
- В теле ответа сам созданный объект: id 42, имя Иван, почта ivan@mail.com.

## Свои запросы

Список всех товаров:

```http
GET /api/products HTTP/1.1
Host: example.com
```

Новый товар «Ноутбук» за 50000:

```http
POST /api/products HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 39

{"name":"Ноутбук","price":50000}
```

Удаление товара с id=7:

```http
DELETE /api/products/7 HTTP/1.1
Host: example.com
```

Новая цена 45000 у товара с id=3:

```http
PUT /api/products/3 HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 15

{"price":45000}
```
