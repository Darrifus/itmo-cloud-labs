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


