## Отчёт по лабораторной работе lab11

## Tasks

- [x] 1. Ознакомиться со ссылками учебного материала
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Были выполнены команды из tutorial
Переходим в домашнюю директорию, создаём необходимые директории и устанавливаем переменные.
```sh
$ cd ~
$ mkdir install
$ mkdir tmp
$ export HOME_PREFIX=`pwd`/install
$ echo $HOME_PREFIX
$ export USERNAME=`whoami`
```
Переходим во временну папку
```sh
$ cd tmp
```
Скачиваем и выполняем установку libevent.
```sh
$ wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
$ tar -xvzf libevent-2.1.8-stable.tar.gz
$ cd libevent-2.1.8-stable
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install
$ cd ..
```
Скачиваем и выполняем установку ncurses.
```sh
$ wget http://invisible-island.net/datafiles/release/ncurses.tar.gz
$ tar -xvzf ncurses.tar.gz
$ cd ncurses-5.9
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install
$ cd ..
```
Скачиваем и выполняем установку tmux. В качестве флагов для конфигурационного файла указываем пути до библиотек, установленных ранее программ. Это необходимо для установления зависимостей.
```sh
$ wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
$ tar -xvzf tmux-2.5.tar.gz
$ cd tmux-2.5
$ ./configure --prefix=${HOME_PREFIX} CFLAGS="-I${HOME_PREFIX}/include -I${HOME_PREFIX}/include/ncurses" LDFLAGS="-L${HOME_PREFIX}/lib"
$ make && make install
$ cd ..
```
Скачиваем и выполняем установку ngrok.
```sh
$ wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
$ unizp ngrok-stable-linux-amd64.zip
$ mv ngrok ${HOME_PREFIX}/bin
```
Задаём необходимые переменные и запускаем tmux.
```sh
$ export LD_LIBRARY_PATH=${HOME_PREFIX}/lib
$ export PATH="${HOME_PREFIX}/bin:${PATH}"
$ tmux
```
Удаляем временные папку и папку куда скачивались необходимые файл для программ.
```sh
$ cd ~
$ rm -rf tmp install
```
Создаём создания сеансов совместной разработки.
```sh
$ tmux new -s session_with_group
```
Создаём "тонель" для установки соединения, используя сервис ngrok.
```sh
# Alisa:
$ open https://ngrok.com/signup
$ export NGROK_TOKEN=1seZMgZFCQ8sYEh9HMLQsaWLtCa_2GWLjdrmD1eaZtJecVQ8T
$ ngrok authtoken ${NGROK_TOKEN}
$ ngrok tcp 22
15634
```