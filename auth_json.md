## Регистрация/Аутентификация (JSON)
### 1. запрос регистрации
<br> Доступные поля

|field | type | comment |
|---|---|---|
|first_name | string | Имя <b>Обязательное</b>|
|last_name | string, nullable | Фамилия |
|company | string | nullable Компания |
|position | string | nullable Должность
|phone | phone, user:unique | Номер телефона <b>Обязательное</b> |
|code | 6 digits, nullable | Смс код |
|email | unique, nullable | почта <b>Обязательное</b> |
```
POST /api/v1/register HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
	"first_name": "user",
	"last_name": "last",
	"email": "freel.k@gmail.c",
	"phone": "89991339321",
}
```
Ответ при APP_DEBUG=true
```
{"code":"711468"}
```
### 2. подтверждение регистрации
```
POST /api/v1/register/confirm HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
	"phone": "89991339321",
	"code": "695184"
}
```
Ответ:
```
{
    "token": "{token}",
    "token_type": "bearer",
    "expires_in": 3600
}
```
### 3. аутентификация
```
POST /api/login HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
	"phone": "89991339321"
}
```
Ответ при APP_DEBUG=true
```
{"code":"216239"}
```
### 4. подтверждение аутентификации
```
POST /api/login/confirm HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
	"phone": "89991339321",
	"code": "695184"
}
```
Ответ:
```
{
    "token": "{token}",
    "token_type": "bearer",
    "expires_in": 3600
}
```
### 4. обновление токена
TTL токена 60 минут
<br>TTL обновления 2 недели
```
POST /api/login/refresh HTTP/1.1
Host: localhost:8000
Authorization: Bearer {#token}
```
Ответ:
```
{
    "token": "{token}",
    "token_type": "bearer",
    "expires_in": 3600
}
```
### 6. любой запрос после получения токена
```
POST /api/v1/presentations HTTP/1.1
Host: localhost:8000
Authorization: Bearer {#token}
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
    "data": {
        "type": "presentations",
        "attributes": {
            "name": "hello-world"
        }
    }
}
```
Ответ:
```
{
    "data": {
        "type": "presentations",
        "id": "4",
        "attributes": {
            "created-at": "2019-01-03T06:37:18+00:00",
            "updated-at": "2019-01-03T06:37:18+00:00"
        },
        "links": {
            "self": "http://localhost:8000/v1/presentations/4"
        }
    }
}
```
