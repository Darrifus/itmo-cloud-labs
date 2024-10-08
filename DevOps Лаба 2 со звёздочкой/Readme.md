# Лабораторная №2 со звёздочкой

## Задание
* Написать “плохой” Docker compose файл, в котором есть не менее трех “bad practices” по их написанию.
* Написать “хороший” Docker compose файл, в котором эти плохие практики исправлены.
* В хорошем файле настроить сервисы так, чтобы контейнеры в рамках этого compose-проекта поднимались вместе, но не "видели" друг друга по сети.

## Выполнили человеки
* Усольцева Дарья
* Забродский Александр
* Азатжонова Мехринисо

## Установка Docker compose

Первым делом устанавливаем Docker compose на операционную систему (docker уже есть из прошлой лабы). В нашем случае это Linux, а именно Ubuntu:24.04, запущенная через VirtualBox. Для того чтобы установить Docker compose, нужно открыть терминал и поочерёдно ввести команды:

```
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```

Готово, Docker compose установлен. Осталось только проверить это. 

![downloading](https://github.com/user-attachments/assets/09ca9db6-5c05-48cb-9517-abd4eeebbb90)


Видим, что в консоли выводится версия установленного Docker compose. Значит, всё установленно и работает.

## Написание Docker compose файла

### Что такое Docker compose?

Docker compose - это инструмент для работы с контейнерами Docker. С его помощью можно эффективно и удобно управлять средами, в которых запущено более одного контейнера. Средами, в которых запущен только 1 контейнер, естественно тоже можно управлять с помощью упомянутого инструмента. Использование Docker compose позволяет обеспечивать изоляцию окружений, запускать, пересобирать и останавливать контейнеры одновременно, следить за их статусом и получить множество других удобных и полезных преимуществ.

### Как с ним работать?

Для работы с Docker compose необходимо создать текстовый файл docker-compose.yml, в котором будет осуществляться вся последующая работа с сервисом. Для поднятия всех сервисов, указанных в этом файле, нужно, находясь в одной дериктории с написанным файлом, написать в консоль команду:

```
sudo docker compose up
```

### Полезные команды

Далее при выполнении лабораторной работы так же использовались команды:

1. sudo docker system prune

Данная команда удаляет из системы всю информацию про докеры. Это позволяет запускать новые докеры, не опасаясь, что на устройстве закончится память. 

2. sudo docker stop <название первого контейнера> <название второго контейнера>

Вторая же команда позволяет останавливать контейнеры. После их остановки можно поменять файл docker-compose.yml и другие файлы, которые файлы, с которыми взаимодействуют контейнеры, и после этого запустить их снова. Это так же будет экономить время и место на компьютере, ведь контейнерам не придётся запускаться заново с нуля. Если нужно сделать незначительное изменение в коде, то при запуске на контейнер уже не будет скачиваться операционная система и ранее установленные утилиты. Важно не забывать, что к указанным в docker-compose.yml названиям сервисов в начале надо добавлять "<путь до директории, в которой лежит наш файл>-", а в конце - "-1".

## Плохой Docker compose файл

Плохой docker-compose.yml:

```
version: '2'
services:
  My_cont_1:
    image: ubuntu:22.04
    privileged: false
    volumes:
      - ./script1:/container_folder:ro
    command: >
      /bin/bash -c
      "apt-get update && apt-get install -y bash &&
      apt-get install -y iputils-ping && bash /container_folder/MyScript1.bash"
  My_cont_2:
    image: ubuntu:22.04
    privileged: false
    volumes:
      - ./script2:/container_folder:ro
    command: >
      /bin/bash -c
      "apt-get update && apt-get install -y bash &&
      apt-get install -y iputils-ping && bash /container_folder/MyScript2.bash"
```

Так как в последнем задании нам предстоит проверять связь между докерами, мы решили сделать два сервиса, которые выполняют простой скрипт на языке bash, который сначала выводит в консоль сообщение о том, что контейнер работает, а также его номер, а потом с помощью утилиты ping проверяет соединение с интернетом, выполняя команду:

```
ping -c 2 8.8.8.8
```

В данном случае опция -c 2 обозначает, что соединение с 8.8.8.8 будет происходить не до бесконечности, а только 2 раза.

Файл MyScript1.bash, используемый в первом контейнере:

```
echo "Container 1 is working!"
ping -c 2 8.8.8.8
```

Файл MyScript2.bash, используемый во втором контейнере:

```
echo "Container 2 is working!"
ping -c 2 8.8.8.8
```

Далее опишем плохие практики, которые мы использовали при написании плохого Docker compose файла.

```
image: ubuntu:latest
```

### Плохие практики

1. Первой плохой практикой при написании нашего Docker compose файла является использование тега :latest. Использование этого тега для загрузки последней версии является антипаттерном при написании как докерфайлов, так и Docker compose файлов. Использование последней версии образа дестабилизирует систему. Через время последняя версия образа скорее всего изменится, собираемый докер тогда будет устанавливать другой образ, и выполняемые команды могут начать исполняться совсем по-другому.

```
privileged: true
```

2. Второй плохой практикой при написании нашего Docker compose файла является запуск контейнера с привилегиями. В этом случае контейнер получает полный доступ к локальному устройству хоста. Это может нарушить безопасность хост-системы и повлиять на её работу.

```
./scripts:/scripts
```

3. Третьей плохой практикой является хранение данных на локальной системе без правильно организованных прав доступа. Это создаст угрозу уязвимости для хоста. Так же права доступа на хосте могут не совпасть с правами доступа внутри контейнера. Это может вызвать конфликт. В конце концов, контейнер становится менее переносимым с одной системы на другую, если он использует файлы на хосте.

Покажем, что плохой Dokcer compose работает.

![bad-1](https://github.com/user-attachments/assets/67f19b00-ccae-4df0-a1ef-151704184bdd)

![bad-2](https://github.com/user-attachments/assets/03c5f4be-2ea4-4558-bed3-cc97b9c5e56d)

Видим, что контейнеры работают, и выводят в консоль нужную информацию.

## Хороший Docker compose файл

Хороший docker-compose.yml:

```
version: '3'
services:
  My_cont_1:
    image: ubuntu:latest
    container_name: cont1
    privileged: true
    volumes:
      - ./scripts:/scripts
    command: /bin/bash -c "apt-get update && apt-get install bash && apt-get install -y iputils-ping && bash /scripts/MyScript1.bash"
  My_cont_2:
    image: ubuntu:latest
    container_name: cont2
    privileged: true
    volumes:
      - ./scripts:/scripts
    command: /bin/bash -c "apt-get update && apt-get install bash && apt-get install -y iputils-ping && bash /scripts/MyScript2.bash"
```

Файлы MyScript1.bash и MyScript2.bash остались такими же.

### Решения, принятые для исправления плохих практик

По порядку опишем решения.

```
image: ubuntu:22.04
```

1. Тег :latest был заменён на конкретную версию 22:04. Теперь система будет запускаться всегда одинаковая. Она стала более стабильной.

```
privileged: false
```

2. Контейнер теперь запускается без привилегий. Значит он не может повлиять на работу хоста.

```
./script1:/container_folder:ro
```

3. Флаг :ro (read only) обеспечивает гарантию того, что контейнер сможет только прочитать данные с хоста. Это увеличивает безопасность для хоста.

Покажем, что хороший Docker compose так же работает.

![good-1](https://github.com/user-attachments/assets/62a65c9c-b2c6-4a9c-9e41-c5c391ba8c1a)

![good-2](https://github.com/user-attachments/assets/59c24ad6-d662-49a6-b7e3-a9ffb45dc681)

Видим, что всё работает и нужные строчки выводятся на экран.

## Настройка сервисов для достижения изоляции работающих контейнеров друг от друга

### Зачем это нужно?

По умолчанию контейнеры в одном файле docker-compose.yml поднимаются в одной сети. Сеть принимает название директории, в которой находится наш файл. Поднимать контейнеры в Docker compose без связи друг с другом может быть полезным по нескольким причинам.

Во-первых, изолированные друг от друга контейнеры не могут повлиять друг на друга. Это делает систему более стабильной и более безопасной. Изоляция снижает вероятность потери важных данных, если нарушится безопасность одного из контейнеров, а также один контейнер не сможет помешать работе другого.

Во-вторых, может быть удобно и эффективно разбить одно приложение на 2 отдельные части. Например, при тестировании и отладке.

В-третьих, изоляция может помочь сэкономить ресурсы, используемые контейнерами. 

Мы перечислили далеко не все причины, по которым контейнеры могут подниматься без связи друг с другом, но мы указали главные из них: улучшение безопасности и устойчивости системы.

### Изолируем сервисы

Реализацию изоляции делаем на физике, в корпусе на Ломоносова (пытаюсь объяснить большую задержку при отправке пакетов на 8.8.8.8). Код, который обеспечивает изоляцию контейнеров очень простой. К каждому имеющемуся контейнеру мы просто приписываем собственную сеть таким образом:

```
networks:
  \- <название сети>
```

Далее, после описания сервисов, задаём наши сети. Указываем тип драйвера :bridge. Стандартный тип для локальных докеров. Задание сети:

```
networks:
  <название первой сети>:
    name: <название первой сети>
  driver: bridge
  <название второй сети>:
    name: <название второй сети>
    driver: bridge
```

Всё, контейнеры теперь будут подниматься без связи друг с другом. Добавим в наши скрипты MyScript1.bash и MyScript2.bash проверку связи контейнеров между друг другом. Для этого добавим в конец файла команду:

```
ping -c 2 <название директории>-<название контейнера>-1
```

Теперь наши файлы MyScript1.bash и MyScript2.bash выглядят так:

```
echo "Container 1 is working!"
ping -c 2 good-My_cont_2-1
ping -c 2 8.8.8.8
```

```
echo "Container 2 is working!"
ping -c 2 good-My_cont_2-1
ping -c 2 8.8.8.8
```

Файл docker-compose.yml выглядит так:

```
version: '3'
services:
  My_cont_1:
    networks:
      - network1
    image: ubuntu:22.04
    privileged: false
    volumes:
      - ./script1:/container_folder:ro
    command: >
      /bin/bash -c
      "apt-get update && apt-get install -y bash &&
      apt-get install -y iputils-ping && bash /container_folder/MyScript1.bash"
  My_cont_2:
    networks:
      - network2
    image: ubuntu:22.04
    privileged: false
    volumes:
      - ./script2:/container_folder:ro
    command: >
      /bin/bash -c
      "apt-get update && apt-get install -y bash &&
      apt-get install -y iputils-ping && bash /container_folder/MyScript2.bash"
networks:
  network1:
    name: network1
    driver: bridge
  network2:
    name: network2
    driver: bridge
```

Показываем результаты:

![seti-1](https://github.com/user-attachments/assets/368129a1-678d-48da-a6e4-9a7caa886598)

![seti-2](https://github.com/user-attachments/assets/ad07ebf5-7e91-47e5-b278-c9497a115c7a)

На скринах видно, что контейнеры друг между другом не пингуются. При этом соединение с интернетом у них есть и всё остальное тоже работает.

### Вдруг они и раньше были изолированы

Чтобы доказать, что раньше контейнеры пинговались, уберём все упоминания сетей из docker-compose.yml. Тогда контейнеры должны подсоединяться друг к другу по своей сети, значит и ping по имени контейнера должен работать. Из скриптов удаляем пинг к интернету, потому что он не нужен, а docker-compose.yml теперь такой же, как в примере с исправленными плохими практиками.

![ne-seti-1](https://github.com/user-attachments/assets/1987ced5-678e-44cb-8527-a2c92f755d24)

![ne-seti-1](https://github.com/user-attachments/assets/1d464ffd-479c-4ac3-9554-a77c06483c58)

На этих скринах видно, что без сетей контейнеры прекрасно связаны, и передают между друг другом пакеты. 1 пакет из 4 где-то потерялся, но это не важно.

В общем видно, что раньше контейнеры были связаны, а теперь каждый подключен к своей собственной сети и не видит других контейнеров.

## Результаты

Теперь подведём небольшие итоги. 

1. Мы написали "плохой" файл docker compose, использовав при его написании 3 "плохих" практики. Все 3 "плохие" практики описаны.
2. Мы написали "хороший" файл docker compose, в котором мы исправили возникшие ранее "плохие" практики. Так же мы описали каждое решение, и как оно повлияет на работу сервиса.
3. Мы создали docker compose, который одновременно запускает два контейнера, при этом эти контейнеры никак друг с другом не связаны и не видят друг друга при работе.

Итак, все задания из ТЗ сделаны, а значит лаба выполнена!

## Это мы отдыхаем

![коты](https://github.com/user-attachments/assets/15a6ac9a-b93f-446a-971e-8e130f9db8e9)

