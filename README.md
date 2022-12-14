# Проект YaMDb
[![Django-app workflow](https://github.com/IlyaER/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/IlyaER/yamdb_final/actions/workflows/yamdb_workflow.yml)

## Описание

Проект YaMDb собирает отзывы пользователей на различные произведения.

### Алгоритм регистрации пользователей

1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт ```/api/v1/auth/signup/```.
2. YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт ```/api/v1/auth/token/```, в ответе на запрос ему приходит token (JWT-токен).
4. При желании пользователь отправляет PATCH-запрос на эндпоинт ```/api/v1/users/me/``` и заполняет поля в своём профайле (описание полей — в документации).

### Пользовательские роли

+ **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
+ **Аутентифицированный пользователь (user)** — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
+ **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
+ **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
+ **Суперюзер Django** — обладет правами администратора (admin)


### API
Основной функционал.

## Основные технологии

+ Django
+ djangorestframework
+ djangorestframework-simplejwt
+ PyJWT

## Запуск проекта

Для запуска необходим Docker и docker-compose

####Шаблон заполнения файла ответов
Наполните файл своими данными (тип, имя, пользователь БД, пароль, ключ доступа)
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=api_yamdb
POSTGRES_DB=api_yamdb
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=p&l%385148kslhtyn^##a1)ilz@4zqj=rq&agdol^##zgl9(vs
```
Зайдите в папку infra и запустите средство управления контейнерами docker-compose
```
cd infra
docker-compose up -d
```

Загрузка тестовых данных в базу штатным средством приложения
```
docker-compose exec web sh -c "chmod +x fixtures/migrate.sh"
docker-compose exec web sh -c "fixtures/migrate.sh"
```

Суперпользователь для тестов.
Имя:    admin
Пароль: admin


Если вы не хотите пользоваться тестовыми данными, то можете самостоятельно
применить миграции, создать суперпользователя и наполнить базу данных.

- Применение миграций:
```
docker-compose exec web python api_yamdb\manage.py migrate
```
- создание суперпользователя:
```
docker-compose exec web python api_yamdb\manage.py createsuperuser
```
- сбор статики:
```
docker-compose exec web python manage.py collectstatic --no-input 
```

## Выключение сервера
```
docker-compose down -v 
```

##
[Документация проекта http://localhost/redoc/](http://localhost/redoc/)

----

Проект: [Яндекс.Практикум](https://practicum.yandex.ru)  

| Разработчик  | Роль  |
|:---:|:---|
|[Feliks](https://github.com/feel2code)	| управление пользователями (Auth и Users): система регистрации и аутентификации, <br>права доступа, работа с токеном, система подтверждения e-mail, поля. |
|[IlyaER](https://github.com/IlyaER)	| категории (Categories)<br>жанры (Genres) <br>произведения (Titles): модели, view и эндпойнты для них.|
|[gseldon](https://github.com/gseldon) 	|отзывы (Review) и комментарии (Comments): модели и view, эндпойнты, права доступа для запросов. <br>Рейтинги произведений. <br> Тимлид. Ревью кода.|

Инфраструктура: [IlyaER](https://github.com/IlyaER)