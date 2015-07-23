---
layout: post
post_title: Homestead 2.0 и возникшие проблемы в ubuntu 14.04
description: Решаем проблему с выполнением homestead edit на ubuntu 14.04
keywords: ubuntu, homestead, laravel, edit, не открывается, descriptor, reffering, proskurnov, aproskurnov
---
После установки homestead 2.0 процесс которой прекрасно описан в официальной документации laravel возникли 2 проблемы про которые там не упомянули. 
Во-первых, нужно создать новую папку для синхронизации гостевой и хостовой системой. По умолчанию это 
<pre class="prettyprint">~/Code</pre>
Во-вторых, при выполнении команды 
<pre class="prettyprint">homestead edit</pre>для редактирования конфига, на ubuntu 14.04 вылезла ошибка 
<pre class="prettyprint">couldn't get a file descriptor referring to console</pre>, это легко поправить. Для этого открываем файл <pre class="prettyprint">~/.composer/vendor/laravel/homestead/src/EditCommand.php</pre> И заменяем 47 строчку 
<pre class="prettyprint">return strpos(strtoupper(PHP_OS), 'WIN') === 0 ? 'start' : 'open';</pre>
на 
<pre class="prettyprint">return 'xdg-open';</pre>
Все, теперь при выполнении homestead edit в редакторе открывается конфиг Homestead.yaml
