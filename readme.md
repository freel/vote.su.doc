# Установка
```
git clone {repo} ./{path}
cd ./{path}
composer install
cp .env.example .env
nano .env
php artisan key:generate
```
При включении в .env APP_DEBUG=true
<br> вместо отправки реальных смс происходит отправка ответа в виде JSON
``` {"code":"711468"} ```
# API
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

# API
| Method   | URI                           | Name                        | Action                                                | Middleware                                      |
|---|---|---|---|---|
| POST     | api/login                     | login                       | App\Http\Controllers\Auth\LoginController@login       | api                                             |
| POST     | api/login/confirm             | login:confirm               | App\Http\Controllers\Auth\LoginController@confirm     | api                                             |
| POST     | api/login/refresh             | login:refresh               | App\Http\Controllers\Auth\LoginController@refresh     | api                                             |
| POST     | api/register                  | register                    | App\Http\Controllers\Auth\RegisterController@register | api                                             |
| POST     | api/register/confirm          | register:confirm            | App\Http\Controllers\Auth\RegisterController@confirm  | api                                             |
| GET|HEAD | api/v1/presentations          | api:v1:presentations.index  | App\Http\Controllers\PresentationController@index     | api,json-api:default,json-api.bindings,jwt.auth |
| POST     | api/v1/presentations          | api:v1:presentations.create | App\Http\Controllers\PresentationController@create    | api,json-api:default,json-api.bindings,jwt.auth |
| GET|HEAD | api/v1/presentations/{record} | api:v1:presentations.read   | App\Http\Controllers\PresentationController@read      | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/presentations/{record} | api:v1:presentations.update | App\Http\Controllers\PresentationController@update    | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/presentations/{record} | api:v1:presentations.delete | App\Http\Controllers\PresentationController@delete    | api,json-api:default,json-api.bindings,jwt.auth |
| GET|HEAD | api/v1/slides                 | api:v1:slides.index         | App\Http\Controllers\SlideController@index            | api,json-api:default,json-api.bindings,jwt.auth |
| POST     | api/v1/slides                 | api:v1:slides.create        | App\Http\Controllers\SlideController@create           | api,json-api:default,json-api.bindings,jwt.auth |
| GET|HEAD | api/v1/slides/{record}        | api:v1:slides.read          | App\Http\Controllers\SlideController@read             | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/slides/{record}        | api:v1:slides.update        | App\Http\Controllers\SlideController@update           | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/slides/{record}        | api:v1:slides.delete        | App\Http\Controllers\SlideController@delete           | api,json-api:default,json-api.bindings,jwt.auth |

# База данных
### users
|field | type | comment |
|---|---|---|
|id | autoincrement, integer | ID |
|first_name | string | Имя |
|last_name | string, nullable | Фамилия |
|company | string | nullable Компания |
|position | string | nullable Должность
|phone | phone, user:unique | Номер телефона |
|code | 6 digits, nullable | Смс код |
|code_created_at | timestamp, nullable | Время генерации смс кода |
|phone_verified_at | timestamp, nullable | Время подтверждения смс кода |
|password | nullable | Пароль генерируемый из кода и номера телефона |
|email | unique, nullable | почта |
### presentations
|field | type | comment |
|---|---|---|
|id | autoincrement, integer | ID |
|name | required, string | Название презентации |
### slides
|field | type | comment |
|---|---|---|
|id | autoincrement, integer | ID |
|name | Имя слайда |
|position | integer | позиция слайда |
|slide | json | Структура слайда |
