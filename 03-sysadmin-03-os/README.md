##Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

#####1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.

	    выполним strace на /bin/bash -c 'cd /tmp, при этом перенаправим потоки в stdout, и выполним grep по папке /tmp, в которую мы выполняем cd
	     
        vagrant@vagrant:~$ strace /bin/bash -c 'cd /tmp' 2>&1 | grep tmp
        execve("/bin/bash", ["/bin/bash", "-c", "cd /tmp"], 0x7ffd7cfa30f0 /* 25 vars */) = 0
        stat("/tmp", {st_mode=S_IFDIR|S_ISVTX|0777, st_size=4096, ...}) = 0
        chdir("/tmp")
        
        vagrant@vagrant:~$ man 2 chdir
        chdir()  changes  the current working directory of the calling process to the di‐
       rectory specified in path.
        
        Полагаю правильным ответом будет chdir("/tmp")

#####2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:

        vagrant@netology1:~$ file /dev/tty
        /dev/tty: character special (5/0)
        vagrant@netology1:~$ file /dev/sda
        /dev/sda: block special (8/0)
        vagrant@netology1:~$ file /bin/bash
        /bin/bash: ELF 64-bit LSB shared object, x86-64 

Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.

        openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3

#####3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
     
     Выполним ping ya.ru >test_in.txt, а затем удалим файл test_in.tx
     Поищем PID процесса, который пишет в удаленный файл с помощью  lsof 
     vagrant@vagrant:~/tmp$ sudo lsof | grep deleted 
     ping      2494                        vagrant    1w      REG              253,0    38097    1317393 /home/vagrant/tmp/test_in.txt (deleted)
     
     и очистим удаленный файл
     echo "" > /proc/2494/fd/1
     
#####4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

    Нет, Зомби-процессы занимают только место в таблице процессов (PID) 

#####5. В iovisor BCC есть утилита opensnoop:

root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc

На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04. Дополнительные сведения по установке.

    vagrant@vagrant:~/tmp$ sudo opensnoop-bpfcc
    PID    COMM               FD ERR PATH
    984    vminfo              4   0 /var/run/utmp
    634    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
    634    dbus-daemon        21   0 /usr/share/dbus-1/system-services
    634    dbus-daemon        -1   2 /lib/dbus-1/system-services
    634    dbus-daemon        21   0 /var/lib/snapd/dbus-1/system-services/
    383    systemd-udevd      14   0 /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.procs
    383    systemd-udevd      14   0 /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.threads
    984    vminfo              4   0 /var/run/utmp
    634    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
    634    dbus-daemon        21   0 /usr/share/dbus-1/system-services
    634    dbus-daemon        -1   2 /lib/dbus-1/system-services
    634    dbus-daemon        21   0 /var/lib/snapd/dbus-1/system-services/

#####7. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

    uname({sysname="Linux", nodename="vagrant", ...}) = 0
    
     Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.

#####8. Чем отличается последовательность команд через ; и через && в bash? Например:

root@netology1:~# test -d /tmp/some_dir; echo Hi
Hi
root@netology1:~# test -d /tmp/some_dir && echo Hi
root@netology1:~#

Есть ли смысл использовать в bash &&, если применить set -e?

    Команды через ; выполняются почередно, не зависимо от статуса их завершения,
    Команда после && выполнится после успешного завершения предыдущей команды.
    Опция  set -e прервет исполнение, если любая команда из очереди завершится с ошибкой, поэтому использовать && не имеет смысла.
    
#####9. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?

    Опция set -e завершает работу, если какая то из команд выполняется с ошибкой.
    Опция set -u завершает работу, если не инициализирована переменная в сценарии.
    Опция set -x включает режим оболочки, в котором все выполняемые команды выводятся на терминал
    Опция set -o pipefail позволяет убедиться, что все команды в сценарии завершились успешно.

#####10. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).
    
    vagrant@vagrant:~/tmp$ ps -o stat
    STAT
    Ss
    R+
    
    
      D    uninterruptible sleep (usually IO)
      I    Idle kernel thread
      R    running or runnable (on run queue)
      S    interruptible sleep (waiting for an event to complete)
      T    stopped by job control signal
      t    stopped by debugger during the tracing
      W    paging (not valid since the 2.6.xx kernel)
      X    dead (should never be seen)
      Z    defunct ("zombie") process, terminated but not reaped by its parent
    