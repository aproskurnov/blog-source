---
layout: post
post_title: Illegal token при редактировании js файла на vagrant
description: Фиксим баг virtualbox'a "Unexpected token ILLEGAL"
keywords: устраняем, лечим, проблема, js, virtualbox, token, illegal, shared, папки, монтирование, sendfile, nginx, apache, proskurnov, aproskurnov
---
Наткнулся на баг virtualbox'а. Почему только сейчас, для меня загадка. После редактирования проекта, js консоль браузера начала сыпать ошибками
<pre class="prettyprint">Uncaught SyntaxError: Unexpected token ILLEGAL</pre>
В отладчике файл выглядел как поврежденный и вместо части файла отображалось:
<pre class="prettyprint">* * *</pre>

Оказалось что это <a href="https://github.com/mitchellh/vagrant/issues/351#issuecomment-1339640">старый баг VirtualBox'a</a> и возникает при стандартном механизме монтирования shared папки vagrant'а
Лечится двумя способами.
<ul>
<li>1. <a href="http://aproskurnov.github.io/2015/05/28/1.html">Примонтировать shared папку как nfs.</a> В большинстве случаев это лучшее решение.</li>
<li>2. Отключить директиву sendfile в конфиге для разработки веб-сервера который вы используете. Если у вас nginx, то это делается так:
<pre class="prettyprint">sendfile off;</pre> 
Если apache, то таким образом:
<pre class="prettyprint">EnableSendfile Off</pre>
</li>
</ul>