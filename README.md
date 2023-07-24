Запуск проекта вручную
Клонировать репозиторий и перейти в него в командной строке:

git clone git@github.com:SkogorevEduard/kittygram_final.git
cd kittygram_final
В корне проекта создать файл .env и прописать в него свои данные. Пример:

POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
Запустить проект через docker-compose:

docker compose -f docker-compose.yml up
Выполнить миграции:

docker compose -f docker-compose.yml exec backend python manage.py migrate
Создать суперюзера:

sudo docker compose -f docker-compose.yml exec backend python manage.py createsuperuser
Собрать статику и скопировать ее:

docker compose -f docker-compose.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.yml exec backend cp -r /app/static_backend/. /backend_static/static/



Автоматический запуск проекта
В файле kittygram_workflow.yml записан сценарий автоматического тестирования, доставки и развертывания проекта. Сценарий активируется при сохранении коммита в ветку main. Для использования сценария необходимы нижеописанные действия.

Зарегиструйтесь на сервисах Github https://github.com/ и DockerHub https://hub.docker.com/

Создайте на сервере папку kittygram.

Сделайте форк проекта. Создайте в репозитории папку kittygram_final/.github/workflows/. Скопируйте содержимое файла kittygram_workflow.yml в файл main.yml в этой папке.

В настройках своего репозитория в разделе Secrets and variables -> Actions создайте следующие Secrets:

DOCKER_USERNAME - имя пользователя на DockerHub

DOCKER_PASSWORD - пароль пользователя на DockerHub

HOST - адрес вашего сервера

SSH_KEY - секретный ssh-ключ для дорступа к вашему серверу

HOST_USER - пользователь на вашем сервере (необходимы права на выполнение команды sudo)

SSH_PASSPHRASE - пароль пользователя

TELEGRAM_TO - id пользователя Телеграм, которому будет отправлено сообщение об успешном исполнении сценария.

TELEGRAM_TOKEN - токен бота, от имени которого будет отпарвлено сообщение.

POSTGRES_DB= имя базы данных

POSTGRES_USER=пользователь базы данных

POSTGRES_PASSWORD=пароль пользователя базы данных

DB_HOST=имя контейнера базы данных DB_PORT=порт базы данных

SECRET_KEY=секретный ключ Джанго

ALLOWED_HOSTS=разрешенные хосты, 127.0.0.1,localhost,ваш-домен

DEBUG = режим отладки

Запуште проект. Проект будет автоматичесмки протестирован, доставлен на ваш сервер и развернут. Проект будет доступн по адресу http://127.0.0.1:9000.