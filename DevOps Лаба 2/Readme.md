# Лабораторная №2

## Задание
* Написать “плохой” Dockerfile, в котором есть не менее трех “bad practices” по написанию докерфайлов.
* Написать “хороший” Dockerfile, в котором эти плохие практики исправлены.

## Выполнили человеки
* Усольцева Дарья
* Забродский Александр
* Азатжонова Мехринисо

## Установка Docker
Первым делом устанавливаем Docker на операционную систему. В нашем случае это Linux, а именно Ubuntu 24.04, запущенная через VirtualBox. Для того чтобы установить Docker, нужно открыть терминал и поочерёдно ввести список команд:
* sudo apt update
* sudo apt install apt-transport-https ca-certificates curl software-properties-common
* sudo install -m 0755 -d /etc/apt/keyrings
* sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
* sudo chmod a+r /etc/apt/keyrings/docker.asc
* echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
* sudo apt update
* sudo apt install docker-ce
Готово, Docker установлен. Осталось только проверить это. Для начала увидим, что всё завелось и работает:

* photo 1

Теперь попробуем запустить какой-нибудь контейнер. Например, приветственный:

* photo 2

Теперь мы точно уверены, что всё скачалось, установилось и работает.

## Написание Dockerfile

### Плохой Dockerfile

Сначала опишем, какие "плохие практики" были использованы при первоначальном написании докерфайла.

1. Первая "плохая практика" - не указана конкретная версия базового образа. 

* FROM ubuntu:latest

В данном примере в качестве базового образа для докера используется ubuntu последней версии. Последняя версия может измениться, и написанный докерфайл может перестать работать в более новых версиях.

2. Вторая "плохая практика" заключается в том, что добавления файла в докер происходит из внешнего источника, через URL ссылку.

* ADD https://raw.githubusercontent.com/Darrifus/itmo-cloud-labs/main/DevOps%20%D0%9B%D0%B0%D0%B1%D0%B0%202/MyScript.bash .

В [документации](https://docs.docker.com/build/building/best-practices/) Docker не рекомендуется использование файлов, полученных по ссылке. Их нельзя удалить, и они увеличивают размер образа. Так же рекомендуется везде, где можно, использовать инструкцию COPY, а не ADD.

3. Следующая "плохая практика" заключается в том, что в контейнер копируется больше данных, чем нужно, а также это делается не очень удобно, указывая путь на папку в инструкции COPY.

