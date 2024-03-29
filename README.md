![Foodgram](https://github.com/BassDriver/foodgram-project-react/actions/workflows/foodgram_workflow.yml/badge.svg)

  

Foodgram - продуктовый помощник с базой кулинарных рецептов. Позволяет публиковать рецепты, сохранять избранные, а также формировать список покупок для выбранных рецептов. Можно подписываться на любимых авторов/

  

Проект доступен по адресу: [ссылка](http://130.193.39.247/)

  

Документация к API доступа по адресу: [ссылка](http://130.193.39.247/api/docs/)

  

В документации описаны возможные запросы к API и структура ожидаемых ответов. Для каждого запроса указаны уровни прав доступа.

  

## Системные требования/технологии:

 Python, Django, Django Rest Framework, Docker, Gunicorn, NGINX, PostgreSQL, Yandex Cloud, Continuous Integration, Continuous Deployment

  

## Запуск проекта в контейнерах:

* Сначала нужно клонировать репозиторий:

```
git clone git@github.com:BassDriver/foodgram-project-react.git
```

* Установить на сервере Docker, Docker Compose:

```
sudo apt install curl                                   # установка утилиты для скачивания файлов
curl -fsSL https://get.docker.com -o get-docker.sh      # скачать скрипт для установки
sh get-docker.sh                                        # запуск скрипта
sudo apt-get install docker-compose-plugin              # последняя версия docker compose
```
* Скопировать на сервер файлы docker-compose.yml, nginx.conf из папки infra (команды выполнять находясь в папке infra):
```
scp docker-compose.yml nginx.conf username@IP:/home/username/   # username - имя пользователя на сервере
                                                                # IP - публичный IP сервера
```
* Для работы с GitHub Actions необходимо в репозитории в разделе Secrets > Actions создать переменные окружения:
```
SECRET_KEY              # секретный ключ Django проекта
DOCKER_PASSWORD         # пароль от Docker Hub
DOCKER_USERNAME         # логин Docker Hub
HOST                    # публичный IP сервера
USER                    # имя пользователя на сервере
PASSPHRASE              # *если ssh-ключ защищен паролем
SSH_KEY                 # приватный ssh-ключ
TELEGRAM_TO             # ID телеграм-аккаунта для посылки сообщения
TELEGRAM_TOKEN          # токен бота, посылающего сообщение

DB_ENGINE               # django.db.backends.postgresql
DB_NAME                 # postgres
POSTGRES_USER           # postgres
POSTGRES_PASSWORD       # postgres
DB_HOST                 # db
DB_PORT                 # 5432 (порт по умолчанию)
```
* Создать и запустить контейнеры Docker, выполнить команду на сервере  _(версии команд "docker compose" или "docker-compose" отличаются в зависимости от установленной версии Docker Compose):_
```
sudo docker-compose up -d
```
* После успешной сборки выполнить миграции:
```
sudo docker-compose exec backend python manage.py migrate
```
* Создать суперпользователя:
```
docker-compose exec web backend manage.py createsuperuser
```
* Собрать статику:
 ```
docker-compose exec backend python manage.py collectstatic --no-input
```
* Наполнить базу данных ингредиентов из файла ingredients.csv:
```
sudo docker-compose exec backend python manage.py from_csv_to_data
```
* Для остановки контейнеров Docker:
```
sudo docker compose down -v      # с их удалением
sudo docker compose stop         # без удаления
```
 ### После каждого обновления репозитория (push в ветку master) будет происходить:
1.  Проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8)
2.  Сборка и доставка докер-образов frontend и backend на Docker Hub
3.  Разворачивание проекта на удаленном сервере
4.  Отправка сообщения в Telegram в случае успеха

### Запуск проекта на локальной машине:
* Клонировать репозиторий:
```
git clone git@github.com:BassDriver/foodgram-project-react.git
```
* В директории infra_dev файл .env заполнить своими данными:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='секретный ключ Django'
```
* Создать и запустить контейнеры Docker из папки infra_dev, последовательно выполнить команды по созданию миграций, сбору статики, созданию суперпользователя, как указано выше
```
docker-compose -f docker-compose.yml up -d
```
* После запуска проект будут доступен по адресу:  [http://localhost/](http://localhost/)
    
* Документация будет доступна по адресу:  [http://localhost/api/docs/](http://localhost/api/docs/)
* 

## Команда разработки backend'а

- :white_check_mark: [Сергей Шубин](https://github.com/BassDriver)