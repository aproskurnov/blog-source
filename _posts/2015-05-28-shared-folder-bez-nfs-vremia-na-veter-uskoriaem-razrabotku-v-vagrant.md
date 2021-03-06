---
layout: post
post_title: Shared folder без nfs - время на ветер, ускоряем разработку в vagrant
description: Увеличиваем скорость доступа к файловой системе в vagrant
keywords: ubuntu, vagrant, nfs, настройка, конфиг, виртуальная машина, скорость, ускоряем, proskurnov, aproskurnov
---
Долгое время пользовался стандартным механизмом создания общей папки в vagrant. Т.е. в каждом конфиге у меня прописана строчка:
<pre class="prettyprint">config.vm.synced_folder "host_path/", "/guest_path"</pre>
При этом все работало ужасно медленно. Сборка gulp, копирование файлов, сборка БЭМ-проекта или другие подобные операции позволяли отлучиться попить кофе или вздремнуть полчасика.
В конце концов это все мне надоело и я полез искать решение. Оказывается оно давно есть и описано в документации.
Нужно использовать вместо стандартного механизма - nfs.
В ubuntu это делается так:

<ul>
<li>
1. Устанавливаем nfs сервер
<pre class="prettyprint">sudo apt-get install nfs-kernel-server nfs-common</pre>
</li>
<li>
2. Изменяем vagrant конфиг
<pre class="prettyprint">config.vm.synced_folder "host_path/", "/guest_path", type: "nfs"</pre>
</li>
<li>
3. Для того чтобы нам не требовалось каждый раз при запуске виртуальной машины вводить пароль администратора, открываем файл 
<pre class="prettyprint">/etc/sudoers</pre>
и прописываем
<pre class="prettyprint">
Cmnd_Alias VAGRANT_EXPORTS_ADD = /usr/bin/tee -a /etc/exports
Cmnd_Alias VAGRANT_NFSD_CHECK = /etc/init.d/nfs-kernel-server status
Cmnd_Alias VAGRANT_NFSD_START = /etc/init.d/nfs-kernel-server start
Cmnd_Alias VAGRANT_NFSD_APPLY = /usr/sbin/exportfs -ar
Cmnd_Alias VAGRANT_EXPORTS_REMOVE = /bin/sed -r -e * d -ibak /etc/exports
%sudo ALL=(root) NOPASSWD: VAGRANT_EXPORTS_ADD, VAGRANT_NFSD_CHECK, VAGRANT_NFSD_START, VAGRANT_NFSD_APPLY, VAGRANT_EXPORTS_REMOVE
</pre>
</li>
</ul>

Запускаем виртуальную машину и наслаждаемся приростом скорости.