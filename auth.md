## Регистрация/Аутентификация
### 1. запрос регистрации
```
POST /api/register/ HTTP/1.1
Host: localhost:8000
Content-Type: application/x-www-form-urlencoded

first_name=name&phone=+7-999-13-393-20&email=mail@mail.com
```
Ответ при APP_DEBUG=true
```
{"code":"711468"}
```
### 2. подтверждение регистрации
```
POST /api/register/confirm HTTP/1.1
Host: localhost:8000
Content-Type: application/x-www-form-urlencoded

code=711468&phone=+79991339320
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
Content-Type: application/x-www-form-urlencoded

phone=%2B79991339323
```
Ответ при APP_DEBUG=true
```
{"code":"216239"}
```
### 4. подтверждение аутентификации
```
POST /api/login/confirm HTTP/1.1
Host: localhost:8000
Content-Type: application/x-www-form-urlencoded

code=216239&phone=%2B79991339322
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
