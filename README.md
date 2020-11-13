# Debian Server Set Up for Python or Django

**Создание пользователя и настройка ssh**
Подключаемся через SSH к удаленному серверу Debian, обновляем репозитории и устанавливаем несколько начальных необходимых пакетов:
```
sudo apt-get update ; \
sudo apt-get install -y vim mosh tmux htop git curl wget unzip zip gcc build-essential mc make
```

Конфигурируем ssh
```
sudo vim /etc/ssh/sshd_config
    AllowUsers www
    PermitRootLogin no
    PasswordAuthentication no
```

Перезапускаем ssh и задаем пароль пользователю
```
sudo service ssh restart
sudo passwd www
```
**Обязательные пакеты и ZSH**
*Установка Zshell является необязательной, некоторые считают это не безопастным. Мне с ней комфортно и более удобно работать.
```
sudo apt-get install -y zsh tree redis-server nginx  libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python3-dev python-pil python3-lxml libxslt1-dev python-libxml2 python-libxslt1 libffi-dev libssl-dev python-dev gnumeric libsqlite3-dev libpq-dev libxml2-dev libxslt1-dev libjpeg-dev libfreetype6-dev libcurl4-openssl-dev supervisor
```

Устанавливаем фреймворк oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Конфигурируем нужные алиасы и сразу пропишем python в путь
```
vim ~/.zshrc
    alias cls="clear"
		export PATH=$PATH:/home/killery/.python/bin
```
*не забудьте обновитьб конфиг `$ . ~/.zshrc`

**Установка python 3.9 из искходников**
Собераем из исходного кода Python 3.9, устанавливаем с префиксом в папку ~/.python:
В make в параметре -j укажите количество доступных ядер для задачи
```
wget https://www.python.org/ftp/python/3.9.0/Python-3.9.0.tgz ; \
tar xvf Python-3.9.0.tgz ; \
cd Python-3.9.0 ; \
mkdir ~/.python ; \
./configure --enable-optimizations --prefix=/home/www/.python ; \
make -j8 ; \
sudo make altinstall
```

Обновим pip'ку
```
sudo /home/www/.python/bin/python3.9 -m pip install -U pip
```
