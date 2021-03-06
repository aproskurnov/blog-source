---
layout: post
post_title: Настраиваем деплой с помощью capistrano с локальной машины на примере машины с установленной ubuntu
description: Настраиваем локальную машину для деплоя на сервер разработки с помощью capistrano
keywords: настраиваем, capistrano, git, deploy, ssh-keygen, ubuntu, ssh-copy-id, proskurnov, aproskurnov
---
<ol>
<li>
Для начала генерим пару ssh-ключей, если они у вас еще не сгенерены. Это возможно с помощью команды
<pre class="prettyprint">ssh-keygen</pre>
на все вопросы коммандной строки жмем просто [enter]
</li>
<br>
<li>
Добавляем публичный ключ, созданный на предыдущем шаге в список delpoyment ключей bitbucket.org Для этого либо открываем файл 
<pre class="prettyprint">~/.ssh/id_rsa.pub</pre> и копируем его содержимое в буффер обмена,
 либо выполняем команду <pre class="prettyprint">xclip -sel clip < ~/.ssh/id_rsa.pub</pre> после чего вставляем его в веб-интерфейсе битбакета.
 </li>
 <br>
<li>
Открываем файл <pre class="prettyprint">~/.ssh/config</pre> (или создаем его, если такого файла нет) и добавляем в конец алиас для сервера на который будет деплоиться наше приложение 
+ подключаем forward agent для того чтобы наш ключ прокидывался на сервер.
<pre class="prettyprint">
    Host [алиас]
        User [имя юзера]
        Hostname [адрес хоста]
        ForwardAgent yes
</pre>
</li>

<li>
Добавляем ключ в агента
<pre class="prettyprint">ssh-add</pre>
</li>

Шаги 1-4 Нужны чтобы при деплое capistrano мог авторизоваться с удаленного сервера на http://bitbucket.org без ввода логина/пароля.<br>
<br>
<li>
Далее прокидываем публичный ключ на наш deploy-сервер:
<pre class="prettyprint">ssh-copy-id [введенный в п.3 алиас]</pre>
</li>
Этот шаг нужен чтобы capistrano при деплое мог коннектиться к серверу без ввода пароля<br>
Теперь пришло время установить сам capistrano<br>
<br>
<li>
Для этого воспользуемся пакетным менеджером ruby - gem. <pre class="prettyprint">sudo gem install capistrano</pre>
</li>
<li>
Осталось только скачать сам проект из git-репозитория <pre class="prettyprint">git clone [путь к репозиторию git]</pre>
</li>
<li>
Зайти в директорию с проектом и если он сконфигурирован для деплоя с помощью capistrano, просто набрать <pre class="prettyprint">cap production deploy</pre>
</li>
</ol>