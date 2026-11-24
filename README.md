# ДЗ 3 Docker

## На 4 балла

Собрал образ и запустил контейнер.

docker build -t cnt:1 .
docker run -d --name cnt \
  -p 127.0.0.1:8080:8000 \
  -e GREETING='Hello from a container!' \
  cnt:1


Проверил работу счетчика через curl.

curl http://127.0.0.1:8080/
curl -X POST http://127.0.0.1:8080/increment

В начале счетчик был равен 0. После пяти POST-запросов получил 5.

Результат сохранил в `logs/res.txt`, логи в `logs/cnt.txt`.

docker stop cnt
docker rm cnt


## На 6 баллов

Для запуска приложения через Docker Compose используется файл `compose.yaml`.

cp .env.example .env
docker compose up -d --build


В файле `.env` можно поменять порт через WEB_PORT и приветствие через MSG. У меня порт 8080 и приветствие Hello!

Для хранения данных подключил volume `sql-data` к каталогу `/data`.

Сделал 6 POST-запросов, сохранил значение счетчика и пересоздал контейнер.

curl http://127.0.0.1:8080/ > logs/sql1.txt
docker compose down
docker compose up -d
curl http://127.0.0.1:8080/ > logs/sql2.txt


До удаления контейнера счетчик был равен 6. После повторного запуска значение осталось таким же. При этом hostname поменялся с `fe87a15d75a4` на `29d50ddafb50`.

Результаты проверок находятся в `logs/sql1.txt` и `logs/sql2.txt`.

Остановка приложения:

docker compose down