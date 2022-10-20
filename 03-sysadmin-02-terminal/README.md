#Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

  #### 1.    Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

    vagrant@vagrant:~$ type -a cd
    cd is a shell builtin

  #### 2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.

     Команда wc -l выводит количество строк в в объекте.
     Альтернативой может послужить команда grep -c

  ####3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?

    vagrant@vagrant:~$ lsof -p 1
    COMMAND PID USER   FD      TYPE DEVICE SIZE/OFF NODE NAME
    systemd   1 root  cwd   unknown                      /proc/1/cwd (readlink: Permission denied)
    systemd   1 root  rtd   unknown                      /proc/1/root (readlink: Permission denied)
    systemd   1 root  txt   unknown                      /proc/1/exe (readlink: Permission denied)
    systemd   1 root NOFD                                /proc/1/fd (opendir: Permission denied)

Проверить можно командой pstree -p

    vagrant@vagrant:~$ pstree
    systemd─┬─ModemManager───2*[{ModemManager}]
                        ├─VBoxService───8*[{VBoxService}]
                        ├─accounts-daemon───2*[{accounts-daemon}]
                        ├─agetty
                        ├─apache2───2*[apache2───26*[{apache2}]]
                        ├─atd
                        ├─cron
                        ├─dbus-daemon
                        ├─irqbalance───{irqbalance}
                        ├─multipathd───6*[{multipathd}]
                        ├─networkd-dispat
                        ├─polkitd───2*[{polkitd}]
                        ├─rsyslogd───3*[{rsyslogd}]
                        ├─snapd───12*[{snapd}]
                        ├─sshd───sshd───sshd───bash───pstree
                        ├─systemd───(sd-pam)
                        ├─systemd-journal
                        ├─systemd-logind
                        ├─systemd-network
                        ├─systemd-resolve
                        ├─systemd-udevd
                        └─udisksd───4*[{udisksd}] 
 
   ####4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

    vagrant@vagrant:~$ who -a
           system boot  2022-08-24 06:34
           run-level 5  2022-08-24 06:34
    LOGIN      tty1         2022-08-24 06:34               682 id=tty1
    vagrant  + pts/0        2022-10-18 04:29   .          7530 (10.0.2.2) 
    vagrant@vagrant:~$ screen -d -m -S root
    vagrant@vagrant:~$ who -a
           system boot  2022-08-24 06:34
           run-level 5  2022-08-24 06:34
    LOGIN      tty1         2022-08-24 06:34               682 id=tty1
    vagrant  + pts/0        2022-10-18 04:29   .          7530 (10.0.2.2)
               pts/1        2022-10-18 04:40              7267 id=ts/1  term=0 exit=0
    vagrant@vagrant:~$ ls 2>/dev/pts/1

   ####5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

    cat < test_in.txt > test_out.txt 

   ####6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?

    Можно 
    ls > /dev/pts/1

   ####7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

    Первая команда создала дискриптор 5 и перенаправила вывод в stdout
    Вторая команда направляет в это файловый дескриптор вывод echo. 

   ####8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

    vagrant@vagrant:~$ ping -c 10 ya.ru 6>&2 2>&1 1>&6 | wc -l
    PING ya.ru (87.250.250.242) 56(84) bytes of data.
    64 bytes from ya.ru (87.250.250.242): icmp_seq=1 ttl=63 time=180 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=2 ttl=63 time=77.1 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=3 ttl=63 time=74.5 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=4 ttl=63 time=75.2 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=5 ttl=63 time=78.2 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=6 ttl=63 time=74.3 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=7 ttl=63 time=74.3 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=8 ttl=63 time=74.6 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=9 ttl=63 time=77.2 ms
    64 bytes from ya.ru (87.250.250.242): icmp_seq=10 ttl=63 time=74.3 ms
    
    --- ya.ru ping statistics ---
    10 packets transmitted, 10 received, 0% packet loss, time 9138ms
    rtt min/avg/max/mdev = 74.273/85.993/180.169/31.421 ms
    0
    
   ####9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?

    Выводит переменные окружения процесса.
    Аналогичный вывод можно получить с помощью команд env и printenv

   ####10.Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.

    /proc/[pid]/cmdline
          This  read-only file holds the complete command line for the process, unless the process is a zombie.  In the latter case, there is nothing in this file: that is, a read on this file will return 0 characters.  The com‐
          mand-line arguments appear in this file as a set of strings separated by null bytes ('\0'), with a further null byte after the last string.
    /proc/[pid]/cmdline - полный путь до файла процесса      
    
     /proc/[pid]/exe
           Under Linux 2.2 and later, this file is a symbolic link containing the actual pathname of the executed command.  This symbolic link can be dereferenced normally; attempting to open it will open the executable.  You can
           even type /proc/[pid]/exe to run another copy of the same executable that is being run by process [pid].  If the pathname has been unlinked, the symbolic link will contain the string '(deleted)' appended to the  origi‐
           nal pathname.  In a multithreaded process, the contents of this symbolic link are not available if the main thread has already terminated (typically by calling pthread_exit(3)).
    /proc/[pid]/exe - символьная ссылка на выполняемый файл запущенного процесса.

   #### 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.

     sse4_2
    vagrant@vagrant:~$ cat /proc/cpuinfo | grep sse
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti fsgsbase avx2 invpcid rdseed clflushopt md_clear flush_l1d arch_capabilities
    flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti fsgsbase avx2 invpcid rdseed clflushopt md_clear flush_l1d arch_capabilities


   #### 12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:

    vagrant@netology1:~$ ssh localhost 'tty'
    not a tty

   Почитайте, почему так происходит, и как изменить поведение.

    По умолчанию ssh не создает tty
    При этом если запустить ssh с ключом -t, то принудительно запустится псевдотерминал.
    
    vagrant@vagrant:~$ ssh -t localhost tty
    /dev/pts/1
    Connection to localhost closed.
  
  ####13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.

    получилось сделать по этой инструкции.
    https://github.com/nelhage/reptyr#readme

   ####14.sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.

    Строка sudo echo string > /root/new_file не отработает, т.к. echo запускается с правами root, а вот перенаправление уже запускается под обычными правами.
    tee как раз может писать в файл и на экран и вот она уже запускает с правами root