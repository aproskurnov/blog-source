---
layout: post
post_title: Homestead обновление до версии 2.0.8
description: Решаем ошибку "couldn't find HOME environment" при запуске команд homestead 2.0.8 в ubunt'e
keywords: ubuntu, homestead, laravel, не запускается, environment, proskurnov, aproskurnov
---
После обновления до последней версии homestead неожиданно перестали работать команды в консоли. При запуске какой-нибудь
 
<pre class="prettyprint">homestead up</pre>
 
выводится ошибка
 
<pre class="prettyprint">"couldn't find HOME environment -- expanding `~' (ArgumentError)"</pre>

Это происходит из-за того, что по умолчанию php настроенно таким образом что не заполняет массив $_ENV переменными окружения. 
Открываем файл php.ini, находим строчку 
<pre class="prettyprint">variables_order = "GPCS"</pre>

и правим ее на следующее:

<pre class="prettyprint">variables_order = "EGPCS"</pre>

Все, теперь осталось перезапустить php, набрать в консоли
 
<pre class="prettyprint">homestead up</pre>

и продолжить наслаждаться разработкой.
