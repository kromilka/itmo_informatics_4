# Отчет по Лабораторной №4
## Установка Docker
Сначала был установлен Docker на виртуальную машину с Ubuntu 24.04 (рисунки 1-3).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/b2fa5bca-bd7c-49a9-97d0-cd9671fd46f5)

Рисунок 1 - Установка Docker

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/7528cf55-6cc7-4413-a6d0-8b04cf16562b)

Рисунок 2 - Обновление репозиториев для скачивания и настройки

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/af9519f1-3d52-4d40-b549-0293b701a43d)

Рисунок 3 - Доустановка библиотек

Проверка статуса демона и работы Docker (рисунки 4-5).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/3c459e6e-d44b-4ac3-9ff7-083b352daeb9)

Рисунок 4

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/f6575841-df75-422b-a508-9f2228974480)

Рисунок 5

## Пример
Сначала необходимо было создать Dockerfile, в котором прописать по заданию на основе какого образа будет работать decker image. В данном случае был прописан ubuntu:24.04 для корректной работы. Также обновляем пакетный менеджер (apt-get update) и устанавливаем вакеты cowsay и fortune (для длинных сообщений) (рисунок 6).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/9fa1ebb4-7742-4669-815f-14cbf771ba59)

Рисунок 6

Далее на основе этого Dockerfile собираем наш образ с тегом cowsay (рисунок 7).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/4fccdbb4-0f2c-463d-8941-16524652c35f)

Рисунок 7

Командой `docker run cowsay /usr/games/cowsay "Moo"` запускаем контейнер и передаем строку "Moo" (рисунок 8).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/aa496133-0f38-4186-8d74-878bd586a83f)

Рисунок 8

Далее запускаем контейнер с флагами -it, который позволяет взаимодействовать с /bin/bash контейнера (рисунок 9) и в консоли опять передаем строку "Moo".

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/14fdd367-380a-4819-af97-64f55c015fa2)

Рисунок 9

Проверяем запущенные контейнеры командой `docker ps` (рис. 10)

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/6e9850d1-8e23-4b26-9fd5-d4a91476aaa2)

Рисунок 10

## Задание и решение

По условию задания сначала необходимо запустить в контейнере приложение `aafire`. Для этого прописываем в Dockerfile команду установки пакета liiba-bin, которая необходима для работы aafire, а также ENTRYPOINT, которая выполнит aafire при запуске контейнера (рисунок 11).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/2014cbf9-d797-405a-a28a-8be8f1dfa3d8)

Рисунок 11

Собираем образ (рисунок 12) и запускаем с помощью команды `sudo docker run -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -it aafire`, которая передает данные о графическом интерфейсе (рисунок 13).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/5013de8e-6c0a-41ed-aefd-8cfe9376d7fa)

Рисунок 12

Наблюдаем выполнение aafire (рисунки 13-14).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/d5501725-ecde-4f83-964c-4fbdc04c8981)

Рисунок 13

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/a34b5ddc-704e-4bdc-90d2-b0334dac6ce8)

Рисунок 14

Далее необходимо создать два контейнера с aafire, которые мы свяжем сетью и проверим их связь с помощью ping. Для этого в Dockerfile добавляем iputils-ping для установки (чтобы работал ping) (Рисунок 15). 

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/6bbb911c-ea54-49f2-a023-42879ed5b7b9)

Рисунок 15

Собираем 2 образа: первый будет называться aafire, а второй - aafire2 и запускаем командами `sudo docker run -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -it aafire` и `sudo docker run -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -it aafire2`, соответственно. Наблюдаем за результатом (рисунки 16-17).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/f15dacb4-b7d1-4ed1-a794-0f49e36a0f4e)

Рисунок 16

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/19e889ed-f872-4a18-baef-c0fd8b98eaf0)

Рисунок 17

Далее открываем еще одно окно терминала и создаем сеть командой `docker network create myNetwork` (Рисунок 18).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/eb136aaa-aab9-4ee3-b331-c4d741ecb769)

Рисунок 18

Далее просматриваем какие именно контейнеры запущены (их полные номера) командой `sudo docker ps` и подключаем к сети myNetwork контейнеры с помощью команды `docker network connect myNetwork #имяКонтейнера` (рисунок 19).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/619401ee-537a-4eda-b2e9-fb0a4f719ecf)

Рисунок 19

Теперь просматриваем и проверяем подключились ли контенеры к сети и узнаем их ip-адреса в этой сети командой `docker network inspect myNetwork` (рисунок 20).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/54a9389b-8968-4e39-b0c6-fad404d1522e)

Рисунок 20

Для проверки сети подключаемся в свободном терминале к консоли контейнера `79c5e1ea78c4` и выполняем ping другой другого контейнера по ip указанному на предыдущем скрине (рисунок 21).

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/a95dd612-dbca-4d55-a388-425975adb4d3)

Рисунок 21

То же самое проделываем со вторым контейнером и проверяем связь утилитой ping (рисунок 22)

![image](https://github.com/kromilka/itmo_informatics_4/assets/60718613/a191a80c-f5ba-4cc8-ad5b-87be739c7916)

Рисунок 22

