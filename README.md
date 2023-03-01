![workflow status](https://github.com/klinf1/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Информация о проекте дополнительно доступна по адресу:
```
http://62.84.120.123/redoc/
```

## Описание
Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка.
Произведению может быть присвоен жанр из списка предустановленных. Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти; из пользовательских оценок формируется усреднённая оценка произведения — рейтинг. На одно произведение пользователь может оставить только один отзыв.

## Пользовательские роли
* Аноним — может просматривать описания произведений, читать отзывы и комментарии.
* Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
* Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
* Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
* Суперюзер Django - обладает правами администратора (admin)
## Установка
* Сделать fork репозитория
```
https://github.com/klinf1/yamdb_final/fork
```
* В Settings - Secrets - Actions добавить переменные
```
DOCKER_USERNAME - логин на DockerHub
DOCKER_PASSWORD - пароль на DockerHub
DB_ENGINE - django.db.backends.postgresql
DB_NAME - db
POSTGRES_USER - postgres
POSTGRES_PASSWORD - postgres
DB_HOST - IP-адрес или доменное имя вашего сервера
DB_PORT - 5432
TELEGRAM_TO - id Telegram-аккаунта для уведомлений
TELEGRAM_TOKEN - токен Telegram-бота
```
* Остановить службу nginx
```
sudo systemctl stop nginx
```
* Установить docker
```
sudo apt install docker.io 
```
* Установить docker-compose. [Официальная документация](https://docs.docker.com/compose/install/)
* Скопировать файлы docker-compose.yaml и nginx/default.conf
```
scp ./infra/docker-compose.yaml <ваш_username>@<ваш_сервер>:
scp ./infra/nginx/default.conf <ваш_username>@<ваш_сервер>:/nginx/
```
* Настроить .env файл, добавив в него следующую информацию и поместить его в директорию с docker-compose.yaml
- DB_ENGINE=django.db.backends.postgresql
- DB_NAME=postgres
- POSTGRES_USER=postgres
- POSTGRES_PASSWORD=postgres
- DB_HOST=db
- DB_PORT=5432
- DJANGO_SECRET_KEY=секретный ключ Django
- EMAIL_HOST_USER=адрес вашего google smtp сервера. Необходимо [настроить двухфакторную авторизацию](https://support.google.com/accounts/answer/185839#) и [подключить пароль приложения](https://support.google.com/accounts/answer/185833#)
- EMAIL_HOST_PASSWORD=пароль приложения
- EMAIL_PORT=587
* Внести изменение в репозиторий и сделать push (для активации деплоя)
* После успешного деплоя подключиться к серверу 

* Произвести миграции
```
docker-compose exec web python3 manage.py migrate
```
* Создать суперпользователя
```
docker-compose exec web python3 manage.py createsuperuser
```
* Собрать статику
```
docker-compose exec web python3 manage.py collectstatic --noinput
```
## После запуска контейнеров проект доступен по адресам:
* REST API http://<ваш_сервер>/api/v1/
* ReDoc http://<ваш_сервер>/redoc/
* Администрирование Django http://<ваш_сервер>/admin/


Соавтотры:
* [Vitaly Gudkov](https://github.com/VorVorsky)
* [Gritsenko Sergei](https://github.com/OrdinaryWorker)
