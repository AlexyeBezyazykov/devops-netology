# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

 5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
    
        Processor	: 1
        vendor_id	: GenuineIntel
        cpu family	: 6
        model		: 142
        model name	: Intel(R) Core(TM) i5-8257U CPU @ 1.40GHz
        stepping	: 10
        cpu MHz		: 1391.999
        cache size	: 6144 KB
        
        MemTotal:1000068 kB
        MemFree:122620 kB
        MemAvailable: 572896 kB

6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

    Объём оперативной памятии ресурсы процессора задаются в файле Vagrantfile. 

 8.Ознакомиться с разделами man bash, почитать о настройках самого bash:
какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?

    Длинна журнала устанавливается переменной HISTSIZE, по умолчанию длинаа установлена на 500 последних команд. (указано на стр. 2147 мануала).

 что делает директива ignoreboth в bash?
 
     ignoreboth отключает вывод одинаковых команд (флаг - ignoredups) и вывод команд начинающихся с пробела (флаг - ignorespace). Описано на стр. 579 мануала.

 9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
 
     Резервирование символов (стр. 343 мануала)
     Выражение последовательности как числовое, так и символьное (стр. 1522 мануала)

 10. Основываясь на предыдущем вопросе, как создать однократным вызовом touch 100000 файлов?  
 
     touch file{1..100000}

А получилось ли создать 300000? Если нет, то почему?
 
     touch file{1..300000}
    -bash: /usr/bin/touch: Argument list too long

11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

    проверяет, существует ли католог /tmp

 12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

    bash is /tmp/new_path_directory/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash

(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.
    
    vagrant@vagrant:~$ type -a bash
    bash is /usr/bin/bash 
    bash is /bin/bash
    vagrant@vagrant:~$ mkdir /tmp/new_path_directory/
    vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_directory/
    vagrant@vagrant:~$ type -a bash 
    bash is /usr/bin/bash
    bash is /bin/bash
    vagrant@vagrant:~$ PATH=/tmp/new_path_directory/:$PATH
    vagrant@vagrant:~$ type -a bash
    bash is /tmp/new_path_directory/bash
    bash is /usr/bin/bash
    bash is /bin/bash

13.Чем отличается планирование команд с помощью batch и at?

    at выполняет команды в указанное время.
    batch выполняет команды когда позволяют уровни загрузки системы; другими словами, когда среднее значение нагрузки падает ниже 1,5 или значения, указанного при вызове atd.
