# Django + Docker + Github actions + EC2 + Watchtower

Этот репозиторий - пример того как можно докерезировать ваш Джанго проект, добавить автоматическую сборку докер образа через Github Actions, и как всё это автоматически деплоить на aws ec2.


## Настройка сервера EC2

* Откройте раздел ЕС2 из вашей консоли AWS
* Создайте новый инстанс со следующими параметрами:
  * ОС - Амазон Линукс
  * Ключ - ваш заранее загруженный ключ
  * Файрвол - включите SSH and HTTP трафик, т.е. разрешите порты 22 и 80
* Подключитесь по SSH `ssh ec2-user@x.x.x.x`
* Установите Докер 
  * `sudo yum install -y docker`
  * ` sudo service docker start`
  * `sudo usermod -a -G docker ec2-user`
  * Пере-подключитесь через SSH
  * Проверяем что докер работает `docker run hello-world`
* Запустите ваш контейнер `docker run -d --name django -p 80:8000 slavalab/django-docker`
* Проверьте подключение к вашему серверу по адресу http://x.x.x.x
* Настройте авто-обновление вашего контейнера `docker run -d --name watchtower --restart always -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --interval 60 XXXXX`, где ХХХХХ - имя вашего контейнера