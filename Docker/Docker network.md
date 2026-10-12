### Что такое сеть Docker

Сеть Docker – это общий канал, через который контейнеры видят друг друга. Если два контейнера подключены к одной сети, они обращаются друг к другу по имени контейнера, как по адресу.

Создаем сеть
```
docker network create mynetwork
```
Проверь, что сеть появилась:
```
docker network ls
```
Теперь поднимай базу заново, уже в сети:
```
docker run -d --name mydb --network mynetwork -e POSTGRES_PASSWORD=secret postgres:17-alpine
```
Теперь у базы есть имя `mydb` внутри сети `mynetwork`. Проброс `-p` тут уже не нужен, к базе будет обращаться не твой компьютер, а соседний контейнер по сети.

Меняем в программе одну строчку
Код программы тот же, что и в прошлом уроке. Меняем ровно одно место в строке подключения. Вместо `localhost` пишем имя контейнера базы, `mydb`:
```
// было
var connString = "Host=localhost;Port=5432;Database=postgres;Username=postgres;Password=secret";

// стало
var connString = "Host=mydb;Port=5432;Database=postgres;Username=postgres;Password=secret";
```

Запускаем второй контейнер, в той же сети
```
docker build -t myapp .
docker run --network mynetwork myapp
```
Таким образом, мы запускаем несколько контейнеров в одной сети и они обращаются друг другу по имени контейнера. Здесь `Host=mydb` mydb - это название контейнера из прошлого шага. Ко второму контейнеру так же можно подключаться по имени myapp, находясь в нашей сети (mynetwork)
