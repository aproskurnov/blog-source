---
layout: post
post_title: Лечим хоткеи в phpstorm при русской раскладке ubuntu 13.10(14.04, 14.10, 15.04)
description: Фиксим хоткеи в phpstorm
keywords: фиксим, лечим, проблема, русская, раскладка, layout, phpstorm, горячие, клавиши, хоткейс, hotkeys, ubuntu, fix, proskurnov, aproskurnov
---
Недавно вышел phpstorm 9 с чем всех и поздравляю. Пока тестил новый инструмент, нашел способ вылечить проблему которая существует уже как минимум 2 года и описанная <a href="https://bugs.launchpad.net/unity/+bug/1226962">здесь</a> Если вкратце, то все хоткеи(включая Ctrl+V и Ctrl+C) в phpstorm'е работают только на латинской раскладке, соответственно приходится постоянно переключаться, что неимоверно бесит и вызывает желание сменить IDE/рабочую систему/профессию.
Благодаря товарищу <a href="https://github.com/zheludkovm">zheldukovm</a> можно на время забыть об этой проблеме. Его <a href="https://github.com/zheludkovm/LinuxJavaFixes">LinuxJavaFixes</a> и есть та самая волшебная пилюлька.
Скачиваем два файла:

<pre class="prettyprint">LinuxJavaFixes-1.0.0-SNAPSHOT.jar</pre>
и
<pre class="prettyprint">javassist-3.12.1.GA.jar</pre>

Кладем их куда-нибудь, например 

<pre class="prettyprint">/[путь_к_phpstorm]/fix/</pre>

и прописываем в файлах phpstorm64.vmoptions для 64-битных или phpstorm.vmoptions для 32-битных систем следующую строчку:

<pre class="prettyprint">-javaagent:/<путь_к_файлам_фикса/LinuxJavaFixes-1.0.0-SNAPSHOT.jar</pre>
Запускаем phpstorm,<br>
вуаля, все работает, наслаждаемся
