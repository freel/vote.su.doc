# Запросы
## Без аутентификации
### 1. Запрос презентации (без аутентификации)
```
GET /api/v1/presentations/1 HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```
Ответ
```
{
    "data": {
        "type": "presentations",
        "id": "1",
        "attributes": {
            "created-at": "2019-01-06T08:31:35+00:00",
            "updated-at": "2019-01-06T08:31:35+00:00",
            "name": "hello-world 3"
        },
        "relationships": {
            "author": {
                "links": {
                    "related": "http://localhost:8000/v1/presentations/1/author"
                }
            },
            "slides": []
        },
        "links": {
            "self": "http://localhost:8000/v1/presentations/1"
        }
    }
}
```
### 2. Запрос слайдов
```
GET /api/v1/presentations/1/slides HTTP/1.1
Host: localhost:8000
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```
Ответ:
```
{
    "data": [
        {
            "type": "slides",
            "id": "1",
            "attributes": {
                "created-at": "2019-01-09T13:28:49+00:00",
                "updated-at": "2019-01-09T13:28:49+00:00",
                "slide": "{}"
            },
            "relationships": {
                "presentation": {
                    "links": {
                        "self": "http://localhost:8000/v1/slides/1/relationships/presentation",
                        "related": "http://localhost:8000/v1/slides/1/presentation"
                    }
                }
            },
            "links": {
                "self": "http://localhost:8000/v1/slides/1"
            }
        },
        {
            "type": "slides",
            "id": "2",
            "attributes": {
                "created-at": "2019-01-09T13:33:43+00:00",
                "updated-at": "2019-01-09T13:33:43+00:00",
                "slide": "{}"
            },
            "relationships": {
                "presentation": {
                    "links": {
                        "self": "http://localhost:8000/v1/slides/2/relationships/presentation",
                        "related": "http://localhost:8000/v1/slides/2/presentation"
                    }
                }
            },
            "links": {
                "self": "http://localhost:8000/v1/slides/2"
            }
        },
```

## С аутентификацией
### 1. Создание презентации
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
            "name": "hello-world 1"
        }
    }
}
```
Ответ:
```
{
    "data": {
        "type": "presentations",
        "id": "3",
        "attributes": {
            "created-at": "2019-01-09T13:35:47+00:00",
            "updated-at": "2019-01-09T13:35:47+00:00",
            "name": "hello-world 1"
        },
        "relationships": {
            "author": {
                "links": {
                    "related": "http://localhost:8000/v1/presentations/3/author"
                }
            },
            "slides": []
        },
        "links": {
            "self": "http://localhost:8000/v1/presentations/3"
        }
    }
}
```
### 2. Создание слайда
```
POST /api/v1/presentations HTTP/1.1
Host: localhost:8000
Authorization: Bearer {#token}
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
    "data": {
        "type": "slides",
        "attributes": {
            "name": "{#slide_name}",
            "position": "{#position}",
            "slide": "{#escaped_json}"
        },
        "relationships": {
            "presentation": {
            	"data": {
            		"type": "presentations",
            		"id": "{#presentation_id}"
            	}
            }
        }
    }
}
```
Ответ:
```
{
    "data": {
        "type": "presentations",
        "id": "3",
        "attributes": {
            "created-at": "2019-01-09T13:35:47+00:00",
            "updated-at": "2019-01-09T13:35:47+00:00",
            "name": "hello-world 1"
        },
        "relationships": {
            "author": {
                "links": {
                    "related": "http://localhost:8000/v1/presentations/3/author"
                }
            },
            "slides": []
        },
        "links": {
            "self": "http://localhost:8000/v1/presentations/3"
        }
    }
}
```
