# Домашнее задание к занятию "3.5. Файловые системы"

1. Узнайте о [sparse](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных) файлах.



1. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

		Нет не могут. В Linux каждый файл имеет уникальный идентификатор - индексный дескриптор (inode). Это число, которое однозначно идентифицирует файл в файловой системе. Жесткая ссылка и файл, для которой она создавалась имеют одинаковые inode. Поэтому жесткая ссылка имеет те же права доступа, владельца и время последней модификации, что и целевой файл.

1. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

    ```bash
    Vagrant.configure("2") do |config|
      config.vm.box = "bento/ubuntu-20.04"
      config.vm.provider :virtualbox do |vb|
        lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
        lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
        vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
        vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
      end
    end
    ```

    Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

1. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

			vagrant@vagrant:~$ sudo fdisk /dev/sdb
			
			Welcome to fdisk (util-linux 2.34).
			Changes will remain in memory only, until you decide to write them.
			Be careful before using the write command.
			
			Device does not contain a recognized partition table.
			Created a new DOS disklabel with disk identifier 0x9bc99e62.
			
			Command (m for help): n
			Partition type
			   p   primary (0 primary, 0 extended, 4 free)
			   e   extended (container for logical partitions)
			Select (default p): p
			Partition number (1-4, default 1): 1
			First sector (2048-5242879, default 2048): 2048
			Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): 4194303
			
			Created a new partition 1 of type 'Linux' and of size 2 GiB.
			
			Command (m for help): n
			Partition type
			   p   primary (1 primary, 0 extended, 3 free)
			   e   extended (container for logical partitions)
			Select (default p): e
			Partition number (2-4, default 2): 2
			First sector (4194304-5242879, default 4194304): 4194304
			Last sector, +/-sectors or +/-size{K,M,G,T,P} (4194304-5242879, default 5242879): 5242879
			
			Created a new partition 2 of type 'Extended' and of size 512 MiB.
			
			Command (m for help): w
			The partition table has been altered.
			Calling ioctl() to re-read partition table.
			Syncing disks.

1. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.

			vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb > sdb.txt
			vagrant@vagrant:~$ sudo sfdisk /dev/sdc < sdb.txt
			Checking that no-one is using this disk right now ... OK
			
			Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
			Disk model: VBOX HARDDISK
			Units: sectors of 1 * 512 = 512 bytes
			Sector size (logical/physical): 512 bytes / 512 bytes
			I/O size (minimum/optimal): 512 bytes / 512 bytes
			
			>>> Script header accepted.
			>>> Script header accepted.
			>>> Script header accepted.
			>>> Script header accepted.
			>>> Created a new DOS disklabel with disk identifier 0x9bc99e62.
			/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
			/dev/sdc2: Created a new partition 2 of type 'Extended' and of size 512 MiB.
			/dev/sdc3: Done.
			
			New situation:
			Disklabel type: dos
			Disk identifier: 0x9bc99e62
			
			Device     Boot   Start     End Sectors  Size Id Type
			/dev/sdc1          2048 4194303 4192256    2G 83 Linux
			/dev/sdc2       4194304 5242879 1048576  512M  5 Extended
			
			The partition table has been altered.
			Calling ioctl() to re-read partition table.
			Syncing disks.

1. Соберите `mdadm` RAID1 на паре разделов 2 Гб.

		vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sdb1 /dev/sdc1
		mdadm: Note: this array has metadata at the start and
		    may not be suitable as a boot device.  If you plan to
		    store '/boot' on this device please ensure that
		    your boot-loader understands md/v1.x metadata, or use
		    --metadata=0.90
		mdadm: size set to 2094080K
		Continue creating array? y
		mdadm: Defaulting to version 1.2 metadata
		mdadm: array /dev/md0 started.
		vagrant@vagrant:~$ lsblk
		NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
		loop0                       7:0    0 61.9M  1 loop  /snap/core20/1328
		loop1                       7:1    0 43.6M  1 loop  /snap/snapd/14978
		loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
		sda                         8:0    0   64G  0 disk
		├─sda1                      8:1    0    1M  0 part
		├─sda2                      8:2    0  1.5G  0 part  /boot
		└─sda3                      8:3    0 62.5G  0 part
		  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
		sdb                         8:16   0  2.5G  0 disk
		├─sdb1                      8:17   0    2G  0 part
		│ └─md1                     9:1    0    2G  0 raid1
		└─sdb2                      8:18   0  512M  0 part
		sdc                         8:32   0  2.5G  0 disk
		├─sdc1                      8:33   0    2G  0 part
		│ └─md1                     9:1    0    2G  0 raid1
		└─sdc2                      8:34   0  512M  0 part

1. Соберите `mdadm` RAID0 на второй паре маленьких разделов.

			vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md2 -l 0 -n 2 /dev/sd{b2,c2}
			mdadm: chunk size defaults to 512K
			mdadm: Defaulting to version 1.2 metadata
			mdadm: array /dev/md2 started.
			vagrant@vagrant:~$ lsblk
			NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
			loop0                       7:0    0 61.9M  1 loop  /snap/core20/1328
			loop1                       7:1    0 43.6M  1 loop  /snap/snapd/14978
			loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
			sda                         8:0    0   64G  0 disk
			├─sda1                      8:1    0    1M  0 part
			├─sda2                      8:2    0  1.5G  0 part  /boot
			└─sda3                      8:3    0 62.5G  0 part
			  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
			sdb                         8:16   0  2.5G  0 disk
			├─sdb1                      8:17   0    2G  0 part
			│ └─md1                     9:1    0    2G  0 raid1
			└─sdb2                      8:18   0  512M  0 part
			  └─md2                     9:2    0 1020M  0 raid0
			sdc                         8:32   0  2.5G  0 disk
			├─sdc1                      8:33   0    2G  0 part
			│ └─md1                     9:1    0    2G  0 raid1
			└─sdc2                      8:34   0  512M  0 part
			  └─md2                     9:2    0 1020M  0 raid0

1. Создайте 2 независимых PV на получившихся md-устройствах.

        vagrant@vagrant:~$ sudo pvcreate /dev/md1 /dev/md2
          Physical volume "/dev/md1" successfully created.
          Physical volume "/dev/md2" successfully created.

1. Создайте общую volume-group на этих двух PV.

        vagrant@vagrant:~$ sudo vgcreate vg1  /dev/md1 /dev/md2
        Volume group "vg1" successfully created

1. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

        vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md2
          Logical volume "lvol0" created.

1. Создайте `mkfs.ext4` ФС на получившемся LV.

        vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
        mke2fs 1.45.5 (07-Jan-2020)
        Creating filesystem with 25600 4k blocks and 25600 inodes
        
        Allocating group tables: done
        Writing inode tables: done
        Creating journal (1024 blocks): done
        Writing superblocks and filesystem accounting information: done

1. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.

        vagrant@vagrant:~$ mkdir /tmp/new
        vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/new

1. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.

        vagrant@vagrant:~$ sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
        --2022-11-02 04:55:34--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
        Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
        Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 22482610 (21M) [application/octet-stream]
        Saving to: ‘/tmp/new/test.gz’
        
        /tmp/new/test.gz                                            100%[=========================================================================================================================================>]  21.44M  4.91MB/s    in 4.5s
        
        2022-11-02 04:55:39 (4.75 MB/s) - ‘/tmp/new/test.gz’ saved [22482610/22482610]


1. Прикрепите вывод `lsblk`.

        vagrant@vagrant:~$ lsblk
        NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
        loop0                       7:0    0 61.9M  1 loop  /snap/core20/1328
        loop1                       7:1    0 43.6M  1 loop  /snap/snapd/14978
        loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
        loop3                       7:3    0 63.2M  1 loop  /snap/core20/1634
        loop4                       7:4    0   48M  1 loop  /snap/snapd/17336
        loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
        sda                         8:0    0   64G  0 disk
        ├─sda1                      8:1    0    1M  0 part
        ├─sda2                      8:2    0  1.5G  0 part  /boot
        └─sda3                      8:3    0 62.5G  0 part
          └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
        sdb                         8:16   0  2.5G  0 disk
        ├─sdb1                      8:17   0    2G  0 part
        │ └─md1                     9:1    0    2G  0 raid1
        └─sdb2                      8:18   0  512M  0 part
          └─md2                     9:2    0 1020M  0 raid0
            └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
        sdc                         8:32   0  2.5G  0 disk
        ├─sdc1                      8:33   0    2G  0 part
        │ └─md1                     9:1    0    2G  0 raid1
        └─sdc2                      8:34   0  512M  0 part
          └─md2                     9:2    0 1020M  0 raid0
            └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new

1. Протестируйте целостность файла:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
        vagrant@vagrant:~$ gzip -t /tmp/new/test.gz; echo $?
        0

1. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

        vagrant@vagrant:~$ sudo pvmove -n lvol0 /dev/md2 /dev/md1
          /dev/md2: Moved: 28.00%
          /dev/md2: Moved: 100.00%
        vagrant@vagrant:~$ lsblk
        NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
        loop0                       7:0    0 61.9M  1 loop  /snap/core20/1328
        loop1                       7:1    0 43.6M  1 loop  /snap/snapd/14978
        loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
        loop3                       7:3    0 63.2M  1 loop  /snap/core20/1634
        loop4                       7:4    0   48M  1 loop  /snap/snapd/17336
        loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
        sda                         8:0    0   64G  0 disk
        ├─sda1                      8:1    0    1M  0 part
        ├─sda2                      8:2    0  1.5G  0 part  /boot
        └─sda3                      8:3    0 62.5G  0 part
          └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
        sdb                         8:16   0  2.5G  0 disk
        ├─sdb1                      8:17   0    2G  0 part
        │ └─md1                     9:1    0    2G  0 raid1
        │   └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
        └─sdb2                      8:18   0  512M  0 part
          └─md2                     9:2    0 1020M  0 raid0
        sdc                         8:32   0  2.5G  0 disk
        ├─sdc1                      8:33   0    2G  0 part
        │ └─md1                     9:1    0    2G  0 raid1
        │   └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
        └─sdc2                      8:34   0  512M  0 part
          └─md2                     9:2    0 1020M  0 raid0

1. Сделайте `--fail` на устройство в вашем RAID1 md.

        vagrant@vagrant:~$ sudo mdadm --fail /dev/md1 /dev/sdb1
        mdadm: set /dev/sdb1 faulty in /dev/md1

1. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.

        vagrant@vagrant:~$ dmesg | grep md1
        [  223.067133] md/raid1:md1: not clean -- starting background reconstruction
        [  223.067136] md/raid1:md1: active with 2 out of 2 mirrors
        [  223.067164] md1: detected capacity change from 0 to 2144337920
        [  223.069844] md: resync of RAID array md1
        [  233.568912] md: md1: resync done.
        [14766.621058] md/raid1:md1: Disk failure on sdb1, disabling device.
                       md/raid1:md1: Operation continuing on 1 devices.

1. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    ```bash
    gzip -t /tmp/new/test.gz
   echo $?
    0
    ```
        vagrant@vagrant:~$ gzip -t /tmp/new/test.gz; echo $?
        0
 
1. Погасите тестовый хост, `vagrant destroy`.

 
 