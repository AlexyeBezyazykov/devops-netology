# Домашнее задание к занятию "3.4. Операционные системы. Лекция 2"



## Задание

1. На лекции мы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку,
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`),
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

            vagrant@vagrant:~$ sudo systemctl status  node_exporter
            ● node_exporter.service - Node Exporter
                 Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
                 Active: active (running) since Mon 2022-10-31 04:20:38 UTC; 6min ago
               Main PID: 643 (node_exporter)
                  Tasks: 6 (limit: 1066)
                 Memory: 19.4M
                 CGroup: /system.slice/node_exporter.service
                         └─643 /usr/local/bin/node_exporter
            
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.307Z caller=node_exporter.go:115 level=info collector=thermal_zone
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.307Z caller=node_exporter.go:115 level=info collector=time
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.307Z caller=node_exporter.go:115 level=info collector=timex
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.311Z caller=node_exporter.go:115 level=info collector=udp_queues
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.315Z caller=node_exporter.go:115 level=info collector=uname
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.315Z caller=node_exporter.go:115 level=info collector=vmstat
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.315Z caller=node_exporter.go:115 level=info collector=xfs
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.315Z caller=node_exporter.go:115 level=info collector=zfs
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.316Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
            Oct 31 04:20:38 vagrant node_exporter[643]: ts=2022-10-31T04:20:38.318Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
            vagrant@vagrant:~$ sudo systemctl reboot
            Connection to 127.0.0.1 closed by remote host.
            Connection to 127.0.0.1 closed.
            natalia@mbp-natalia ~ % vagrant ssh
            Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-110-generic x86_64)
            
             * Documentation:  https://help.ubuntu.com
             * Management:     https://landscape.canonical.com
             * Support:        https://ubuntu.com/advantage
            
              System information as of Mon 31 Oct 2022 04:29:51 AM UTC
            
              System load:  1.04               Processes:             126
              Usage of /:   13.1% of 30.63GB   Users logged in:       0
              Memory usage: 19%                IPv4 address for eth0: 10.0.2.15
              Swap usage:   0%                 IPv4 address for eth1: 192.168.137.16
            
            
            This system is built by the Bento project by Chef Software
            More information can be found at https://github.com/chef/bento
            Last login: Mon Oct 31 04:20:50 2022 from 10.0.2.2
            vagrant@vagrant:~$ sudo systemctl status  node_exporter
            ● node_exporter.service - Node Exporter
                 Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
                 Active: active (running) since Mon 2022-10-31 04:28:51 UTC; 1min 4s ago
               Main PID: 641 (node_exporter)
                  Tasks: 4 (limit: 1066)
                 Memory: 14.4M
                 CGroup: /system.slice/node_exporter.service
                         └─641 /usr/local/bin/node_exporter
            
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=thermal_zone
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=time
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=timex
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=udp_queues
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=uname
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=vmstat
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=xfs
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:115 level=info collector=zfs
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.766Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
            Oct 31 04:28:51 vagrant node_exporter[641]: ts=2022-10-31T04:28:51.773Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false

1. Ознакомьтесь с опциями node_exporter и выводом `/metrics` по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

        CPU node_cpu_seconds_total

        Filesystem node_filesystem_avail_bytes
        
        RAM node_memory_MemAvailable_bytes
        
        NETWORK node_network_receive_bytes_total

1. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). 
   
   После успешной установки:
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`,
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере *на своем ПК* (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

![vagrant netdata dashboard 2022-10-31 12-44-42.jpg](https://github.com/AlexyeBezyazykov/devops-netology/blob/main/03-sysadmin-04-os/vagrant%20netdata%20dashboard%202022-10-31%2012-44-42.jpg))

1. Можно ли по выводу `dmesg` понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
           
            Да можно.
            
            vagrant@vagrant:~$ dmesg | grep virt
            [    0.002820] CPU MTRRs all blank - virtualized system.
            [    0.134526] Booting paravirtualized kernel on KVM
            [    2.968361] systemd[1]: Detected virtualization oracle.

1. Как настроен sysctl `fs.nr_open` на системе по-умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?

        vagrant@vagrant:~$ sysctl  fs.nr_open
        fs.nr_open = 1048576
        
        Это лимит на количество открытых дескрипторов
        Другой лимит можно посмотреть так.
        
        vagrant@vagrant:~$ ulimit -Sn
        1024    
        
        

1. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в данном задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т.д.

        vagrant@vagrant:~$ screen -S sleep 1h
        vagrant@vagrant:~$ sudo -i
        root@vagrant:~# unshare -f --pid --mount-proc sleep 1h
        vagrant@vagrant:~$ ps aux | grep sleep
        vagrant    41243  0.0  0.2   7248  2844 ?        Ss   07:47   0:00 SCREEN -S sleep_1h
        root       41279  0.0  0.0   5480   580 pts/1    S+   07:48   0:00 unshare -f --pid --mount-proc sleep 1h
        root       41280  0.0  0.0   5476   580 pts/1    S+   07:48   0:00 sleep 1h
        vagrant    41283  0.0  0.0   6432   720 pts/0    S+   07:48   0:00 grep --color=auto sleep
        vagrant@vagrant:~$ screen -S sudo_session
        vagrant@vagrant:~$ sudo -i
        root@vagrant:~# nsenter --target 41280 --pid --mount
        root@vagrant:/# ps aux
        USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
        root           1  0.0  0.0   5476   580 pts/1    S+   07:48   0:00 sleep 1h
        root           2  0.0  0.3   7236  3968 pts/2    S    07:51   0:00 -bash
        root          13  0.0  0.3   8888  3240 pts/2    R+   07:51   0:00 ps aux
   

1. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации.  
Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?
    
    
        
        :(){ :|:& };: - Логическая бомба (известная также как fork bomb), забивающая память системы, что в итоге приводит к её зависанию. 
        Этот Bash код создаёт функцию, которая запускает ещё два своих экземпляра, которые, в свою очередь снова запускают эту функцию и так до тех пор, пока этот процесс не займёт всю физическую память компьютера, и он просто не зависнет.
        Из вывода dmesg видно, что сработало ограничение контрольной группы (cgroup), на количество процессов пользователя.
        
        [ 5275.585585] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope
        
        
        Параметры по умолчанию можно глянуть командой ulimit -a:
        vagrant@vagrant:~$ ulimit -a
        core file size          (blocks, -c) 0
        data seg size           (kbytes, -d) unlimited
        scheduling priority             (-e) 0
        file size               (blocks, -f) unlimited
        pending signals                 (-i) 3554
        max locked memory       (kbytes, -l) 65536
        max memory size         (kbytes, -m) unlimited
        open files                      (-n) 1024
        pipe size            (512 bytes, -p) 8
        POSIX message queues     (bytes, -q) 819200
        real-time priority              (-r) 0
        stack size              (kbytes, -s) 8192
        cpu time               (seconds, -t) unlimited
        max user processes              (-u) 3554
        virtual memory          (kbytes, -v) unlimited
        file locks                      (-x) unlimited
        
        Ограничить лимит можно командой ulimits -S -u 1000
        Либо в конфиг файле /etc/security/limits.conf
        
      
