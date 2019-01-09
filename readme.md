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
| Method   | URI                           | Name                        | Action                                                | Middleware                                      |
|---|---|---|---|---|
| POST     | api/login                     | login                       | App\Http\Controllers\Auth\LoginController@login       | api                                             |
| POST     | api/login/confirm             | login:confirm               | App\Http\Controllers\Auth\LoginController@confirm     | api                                             |
| POST     | api/login/refresh             | login:refresh               | App\Http\Controllers\Auth\LoginController@refresh     | api                                             |
| POST     | api/register                  | register                    | App\Http\Controllers\Auth\RegisterController@register | api                                             |
| POST     | api/register/confirm          | register:confirm            | App\Http\Controllers\Auth\RegisterController@confirm  | api                                             |
| GET,HEAD | api/v1/presentations          | api:v1:presentations.index  | App\Http\Controllers\PresentationController@index     | api,json-api:default,json-api.bindings,jwt.auth |
| POST     | api/v1/presentations          | api:v1:presentations.create | App\Http\Controllers\PresentationController@create    | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/presentations/{record} | api:v1:presentations.read   | App\Http\Controllers\PresentationController@read      | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/presentations/{record} | api:v1:presentations.update | App\Http\Controllers\PresentationController@update    | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/presentations/{record} | api:v1:presentations.delete | App\Http\Controllers\PresentationController@delete    | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/presentations/{record}/author | api:v1:presentations.relationships.author | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@readRelatedResource | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/presentations/{record}/relationships/author | api:v1:presentations.relationships.author.replace | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@replaceRelationship    | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/presentations/{record}/relationships/author | api:v1:presentations.relationships.author.read    | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@readRelationship       | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/presentations/{record}/relationships/slides | api:v1:presentations.relationships.slides.remove  | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@removeFromRelationship | api,json-api:default,json-api.bindings          |
| POST     | api/v1/presentations/{record}/relationships/slides | api:v1:presentations.relationships.slides.add     | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@addToRelationship      | api,json-api:default,json-api.bindings          |
| PATCH    | api/v1/presentations/{record}/relationships/slides | api:v1:presentations.relationships.slides.replace | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@replaceRelationship    | api,json-api:default,json-api.bindings          |
| GET,HEAD | api/v1/presentations/{record}/relationships/slides | api:v1:presentations.relationships.slides.read    | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@readRelationship       | api,json-api:default,json-api.bindings          |
| GET,HEAD | api/v1/slides                 | api:v1:slides.index         | App\Http\Controllers\SlideController@index            | api,json-api:default,json-api.bindings,jwt.auth |
| POST     | api/v1/slides                 | api:v1:slides.create        | App\Http\Controllers\SlideController@create           | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/slides/{record}        | api:v1:slides.read          | App\Http\Controllers\SlideController@read             | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/slides/{record}        | api:v1:slides.update        | App\Http\Controllers\SlideController@update           | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/slides/{record}        | api:v1:slides.delete        | App\Http\Controllers\SlideController@delete           | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/users/{record}/presentations | api:v1:users.relationships.presentations          | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@readRelatedResource    | api,json-api:default,json-api.bindings,jwt.auth |
| DELETE   | api/v1/users/{record}/relationships/presentations  | api:v1:users.relationships.presentations.remove   | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@removeFromRelationship | api,json-api:default,json-api.bindings,jwt.auth |
| POST     | api/v1/users/{record}/relationships/presentations  | api:v1:users.relationships.presentations.add      | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@addToRelationship      | api,json-api:default,json-api.bindings,jwt.auth |
| PATCH    | api/v1/users/{record}/relationships/presentations  | api:v1:users.relationships.presentations.replace  | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@replaceRelationship    | api,json-api:default,json-api.bindings,jwt.auth |
| GET,HEAD | api/v1/users/{record}/relationships/presentations  | api:v1:users.relationships.presentations.read     | CloudCreativity\LaravelJsonApi\Http\Controllers\JsonApiController@readRelationship       | api,json-api:default,json-api.bindings,jwt.auth |

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
