# devops-netology
## Задание

Исключить скрытую папку .terraform на любом уровне вложенности. А так же исключить всё, что в этой папке
Исключить все файлы, которые имеют расширение .tfstate, а так же все файлы, у которых в названии есть .tfstate.
Исключить файлы crash.log и crash.что-либо ещё.log
Исключить файлы, которые имеют расширение .tfvars и файлы, имеющие расширение .json у которых в названии есть .tfvars
Исключить файлы override.tf и override.tf.json. а так же все файлы с расширением .tf, у которых в названии есть _override и файлы с расширением .json, у которых в названии есть _override.tf  

## Задание 3.1. "Работа в терминале, лекция 1"
переменной HISTSIZE можно задать длину журнала history. Строка - 621
ignoreboth - сокращение 2х команд ignorespace и ignoredups
    ignorespace - Не сохранять в журнал команды, которые начинаются с пробела
    ignoredups - Не сохранять в журнал новую команду, если прошлая была такая же.
{}  - Список, содержимое которого можно использовать в цикле. В отличии от () используется в текущем окружении
touch {1..100000}
300000 Сделать не получилось (bash: /usr/bin/touch: Argument list too long). Выходит за пределы максимального количества аргументов
[[ -d /tmp ]] проверяет, есть ли папка /tmp

mkdir /tmp/new_path_directory
cp /bin/bash /tmp/new_path_directory
PATH=/tmp/new_path_directory/:$PATH

pomortsev@IT-POMOR-UBU:~$ type -a bash
bash is /tmp/new_path_directory/bash
bash is /usr/bin/bash
bash is /bin/bash

at - запускает задачу один раз в указанное время
batch - запускает задачу, когда снижается нагрузка на систему


## Задание "3.2. Работа в терминале, лекция 2"

1. Команда cd типа stdin, поскольку она принимает в качестве параметра путь и переходит в эту директорию

2. grep -c (--count) <some_string> <some_file> 

3. ps -p 1 (systemd)

4. ls -l /root 2> /dev/tty3

5. cat < /etc/issue > ./outcat
    
6. Перенаправить вывод echo hellow > /dev/tty3 возможно, но в текущей сессии этот вывод не будет виден

7. bash 5>&1 Создаст новый дискриптор с номером 5 и заменит им stdout. echo netology > /proc/$$/fd/5 - говорим bash чтобы он вывел netology через 5ый fd который по сути является stdout текущего терминала (5 -> /dev/pts/9
)

8. ll /root 6>&2 2>&1 1>&6 | awk '{print $2" "$3}'  вывод cannot open

9. cat /proc/$$/environ - Выведет переменные окружения, альтернатива - env

10. /proc/<PID>/cmdline - путь до бинарника и его параметры запуска (/usr/bin/pulseaudio--daemonize=no--log-target=journal)  /proc/<PID>/exe ссылка на бинарник

11. grep sse /proc/cpuinfo sse4_2

12. Я не залогинился, а просто передал команду через ssh, tty при этом не создаётся. Но можно указать -t и tty будет

13. echo 0 > /proc/sys/kernel/yama/ptrace_scope (без этого не получилось), htop, ctrl+z, jobs -l([1]+ 3438845 Stopped (tty output)    htop), screen -S test, reptyr 3438845

14. echo просто выводит stdout в консоль, у нее нет прав на создание файла в /root, а tee читает stdin и записывает в stdout и в файл, поэтому у нее есть sudo права

## Задание "2.4. Инструменты Git"

1. git show aefea (commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545  Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>    Update CHANGELOG.md

2. git show 85024d3 (commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23))

3. git log b8d720^2 --pretty=oneline --graph (Первый коммит - 9ea88f22fc6269854151c571162c5bcf958bee2b add/update community provider listings. Второй коммит - 56cd7859e05c36c06b56d013b55a252d0bb7e158 Merge pull request #23857 from hashicorp/cgriggs01-stable)

4. git log --oneline v0.12.23..v0.12.24
33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release

5.  git log -S'func providerSource' --oneline
5af1e6234 main: Honor explicit provider_installation CLI config when present
8c928e835 main: Consult local directories as potential mirrors of providers
Получим 2 коммита где эта функция появлялась, далее 
git show 8c928e835 
-       providerSrc := getproviders.NewRegistrySource(services)
+       providerSrc := providerSource(services)
В коммите 8c928e835 функция была создана.

6. git log -S'globalPluginDirs' --oneline
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package

7. git log -S'func synchronizedWriters' --pretty=format:'%h - %an %ae %ad'
bdfea50cc - James Bardin j.bardin@gmail.com Mon Nov 30 18:02:04 2020 -0500
5ac311e2a - Martin Atkins mart@degeneration.co.uk Wed May 3 16:25:41 2017 -0700

Создал Martin Atkins Wed May 3 16:25:41 2017


## Задание "3.3. Операционные системы, лекция 1"

1. strace -e file /bin/bash -c 'cd /tmp' Отформатировал по обращениям к файлам, увидел в самом конце chdir("/tmp"), видимо это и есть системный вызов cd

2. strace -e trace=openat file /dev/sda Отформатировал по открытию файлов, единственной не библиотекой бый файл magic.mgc. Сперва find её ищет её в /etc 
    openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (No such file or directory)
Но не находит и ищет в  
    openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3

3.  ping localhost > ping.log &
    
    lsof -p 1119313 (ping    1119313 pomortsev    1w   REG  253,1    26281 3699871 /home/pomortsev/ping.log (deleted))

    cat /proc/1119313/fd/1 > ./ping.log
    
    ### Доработка
    Получилось только когда зашел под рутом
    ``` su root
        ping 8.8.8.8 >> ping.log &
        rm ping.log
        lsof -p 11511
        ping    11511 root    1w   REG  253,1    31119 3670210 /home/pomortsev/ping.log (deleted)
        > /proc/11511/fd/1
        lsof -p 11511
        ping    11511 root    1w   REG  253,1      228 3670210 /home/pomortsev/ping.log (deleted)
    ```
4. Зомби не занимают памяти (как процессы-сироты), но блокируют записи в таблице процессов, размер которой ограничен для каждого пользователя и системы в целом.

5. PID    COMM               FD ERR PATH
766    vminfo              6   0 /var/run/utmp
562    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
562    dbus-daemon        18   0 /usr/share/dbus-1/system-services
562    dbus-daemon        -1   2 /lib/dbus-1/system-services
562    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/

6.  strace uname -a (uname({sysname="Linux", nodename="IT-POMOR-UBU", ...}) = 0). 
    man 2 uname ( Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.)

7.  ; - команды выполнятся последовательно одна за другой (test -d /tmp/some_dir; echo Hi) первая команда ничего нв выведет, а вторая напишет Hi
    && - условие, если первая команда выполнена успешно, то выполняется следующая. 
    Из help set (-e  Exit immediately if a command exits with a non-zero status.) Поэтому применять && вместе с set -e не имеет смысла, поскольку и так операция прервется

8. e - Выйти немедленно, если команда завершается с ненулевым статусом
   u - Рассматривать незаданные переменные как ошибку при замене
   x - Печатать команды и их аргументы по мере их выполнения
   o pipefail - возвращаемое значение конвейера — это статус последней команды для выхода с ненулевым статусом или ноль, если ни одна команда не вышла с ненулевым статусом
   set -euxo pipefail можно применять в качестве логирования работы отдельных функций скрипта. Завершить выполнение скрипта, если отдельная фукция завершилась с ошибкой.

9. ps ax -o stat | sort -n Чаще всего значение STAT - S (Прерываемый сон, ожидание завершения события) с различными доп. характеристиками (S< (высокоприоритетный), Sl(многопоточный) , Ss(Лидер сеанса), Ssl многопоточный лидер сеанса)   


## Задание  "3.4. Операционные системы, лекция 2"

1. * wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
   * tar -zxvf node_exporter-1.3.1.linux-amd64.tar.gz
   * cd node_exporter-1.3.1.linux-amd64
   * sudo cp node_exporter /usr/local/bin
   * sudo useradd --no-create-home --shell /bin/false node_exporter
   * sudo vim /etc/systemd/system/node_exporter.service
    ```[Unit]
    Description=Prometheus Node Exporter
    Wants=network-online.target
    After=network-online.target
    [Service]
    User=node_exporter
    Group=node_exporter
    Type=simple
    ExecStart=/usr/local/bin/node_exporter
    [Install]
    WantedBy=multi-user.target
    ```
   * sudo systemctl daemon-reload
   * sudo systemctl enable --now node_exporter
   * https://disk.yandex.ru/i/VtnUZQGpfIv7Qg
Добавляем опции:
   * vim /etc/systemd/system/node_exporter.service
   ```
   EnvironmentFile=/etc/default/node_exporter
   ExecStart=/usr/local/bin/node_exporter $ARGS
   ```
   * vim /etc/default/node_exporter
   ```
   ARGS=--web.listen-address=localhost:9101
   ```
   * https://disk.yandex.ru/i/mYpYyYla4U9efg

2. Процессор:
```
# HELP node_cpu_seconds_total Seconds the CPUs spent in each mode.
# TYPE node_cpu_seconds_total counter
node_cpu_seconds_total{cpu="0",mode="idle"} 587.9
node_cpu_seconds_total{cpu="0",mode="iowait"} 10.09
node_cpu_seconds_total{cpu="0",mode="irq"} 0
node_cpu_seconds_total{cpu="0",mode="nice"} 1.56
node_cpu_seconds_total{cpu="0",mode="softirq"} 1.38
node_cpu_seconds_total{cpu="0",mode="steal"} 0
node_cpu_seconds_total{cpu="0",mode="system"} 29.49
node_cpu_seconds_total{cpu="0",mode="user"} 85.36
node_cpu_seconds_total{cpu="1",mode="idle"} 593.21
node_cpu_seconds_total{cpu="1",mode="iowait"} 8.28
node_cpu_seconds_total{cpu="1",mode="irq"} 0
node_cpu_seconds_total{cpu="1",mode="nice"} 2.96
node_cpu_seconds_total{cpu="1",mode="softirq"} 0.74
node_cpu_seconds_total{cpu="1",mode="steal"} 0
node_cpu_seconds_total{cpu="1",mode="system"} 29.86
node_cpu_seconds_total{cpu="1",mode="user"} 82.87
node_cpu_seconds_total{cpu="2",mode="idle"} 595.99
node_cpu_seconds_total{cpu="2",mode="iowait"} 6.72
node_cpu_seconds_total{cpu="2",mode="irq"} 0
node_cpu_seconds_total{cpu="2",mode="nice"} 1.07
node_cpu_seconds_total{cpu="2",mode="softirq"} 0.38
node_cpu_seconds_total{cpu="2",mode="steal"} 0
node_cpu_seconds_total{cpu="2",mode="system"} 32.88
node_cpu_seconds_total{cpu="2",mode="user"} 76.54
node_cpu_seconds_total{cpu="3",mode="idle"} 586.57
node_cpu_seconds_total{cpu="3",mode="iowait"} 6.85
node_cpu_seconds_total{cpu="3",mode="irq"} 0
node_cpu_seconds_total{cpu="3",mode="nice"} 2.37
node_cpu_seconds_total{cpu="3",mode="softirq"} 0.06
node_cpu_seconds_total{cpu="3",mode="steal"} 0
node_cpu_seconds_total{cpu="3",mode="system"} 28.54
node_cpu_seconds_total{cpu="3",mode="user"} 93.03
```
Память:
```
# HELP node_memory_MemAvailable_bytes Memory information field MemAvailable_bytes.
# TYPE node_memory_MemAvailable_bytes gauge
node_memory_MemAvailable_bytes 1.17837824e+10
# HELP node_memory_MemFree_bytes Memory information field MemFree_bytes.
# TYPE node_memory_MemFree_bytes gauge
node_memory_MemFree_bytes 7.717900288e+09
# HELP node_memory_MemTotal_bytes Memory information field MemTotal_bytes.
# TYPE node_memory_MemTotal_bytes gauge
node_memory_MemTotal_bytes 1.6711028736e+10
```
Диск:
```
node_disk_io_time_seconds_total{device="sda"} 70.08
node_disk_read_time_seconds_total{device="sda"} 91.98700000000001
node_disk_write_time_seconds_total{device="sda"} 58.635
```
Сеть:
```
node_network_receive_bytes_total{device="eno1"} 2.3929992e+07
node_network_receive_packets_total{device="eno1"} 36283
node_network_receive_drop_total{device="eno1"} 24
node_network_transmit_bytes_total{device="eno1"} 4.235203e+06
node_network_transmit_drop_total{device="eno1"} 0
node_network_transmit_packets_total{device="eno1"} 24274
node_network_up{device="docker0"} 1
node_network_up{device="dockernet"} 0
node_network_up{device="eno1"} 1
node_network_up{device="lo"} 0
node_network_up{device="ppp0"} 0
node_network_up{device="tun0"} 0
node_network_up{device="tun1"} 0
```
3. apt install -y netdata

netstat -tulpn

vim /etc/netdata/netdata.conf
```
[global]
        run as user = netdata
        web files owner = root
        web files group = root
        # Netdata is not designed to be exposed to potentially hostile
        # networks. See https://github.com/netdata/netdata/issues/164
        #bind socket to IP = 127.0.0.1
        bind socket to IP = 0.0.0.0
```
lsof -i :19999
```
netdata 21129 netdata    5u  IPv4 100041      0t0  TCP *:19999 (LISTEN)
```
У меня нет vagrant, крутится виртуалка с ubuntu, подключаюсь по IP - https://disk.yandex.ru/i/7OXsN4wkmcE23A

4. dmesg | grep virt

[3.810381] systemd[1]: Detected virtualization kvm.

5. sysctl fs.nr_open (fs.nr_open = 1048576) Ограничение максимального числа открытых дескрипторов для ядра. ulimit -Hn (1048576)
- Жесткое ограничение максимального числа открытых дескрипторов для пользователя

6. unshare --fork --pid --mount-proc ping 8.8.8.8 (Запускаю процесс)
Затем переключаюсь на другой tty и там выполняю:
```
ps aux (чтобы узнать pid пинга)
root 1379043 0.0 0.0 9936 860 pts/5 S+ 20:07 0:00 ping 8.8.8.8
```
Затем:
nsenter --target 1379043 --pid --mount
```
ps aux
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
root 1 0.0 0.0 9936 860 pts/5 S+ 20:07 0:00 ping 8.8.8.8
root 2 0.6 0.0 11052 5096 pts/7 S 20:10 0:00 -bash
root 14 0.0 0.0 11704 3416 pts/7 R+ 20:10 0:00 ps aux
```

7. :(){ :|:& };: это форк-бомба, фукция bash которая вызывает себя же рекурсивно. Которая, похоже, создаёт их пока не дойдет до ограничения максимального числа открытых дескрипторов для пользователя.
А порядок навёл модуль ядра fork rejected by pids controller

## Задание "3.5. Файловые системы"

1. dd if=/dev/zero of=file-sparse bs=1 count=0 seek=2G
du -h --apparent-size file-sparse
```
2,0G	file-sparse 
```
2. Поскольку inode у жестких ссылок одинаковый, поэтому у всех объектов будут одинаковые права.

touch test

ln test test_link

ls -ilh | grep test
```
 3711434 -rw-rw-r--  2 pomortsev pomortsev    0 мая 11 15:06 test
 3711434 -rw-rw-r--  2 pomortsev pomortsev    0 мая 11 15:06 test_link
```
ln test test_link2
chmod 766 test_link2
```
 3711434 -rwxrw-rw-  3 pomortsev pomortsev    0 мая 11 15:06 test
 3711434 -rwxrw-rw-  3 pomortsev pomortsev    0 мая 11 15:06 test_link
 3711434 -rwxrw-rw-  3 pomortsev pomortsev    0 мая 11 15:06 test_link2
```
chmod 664 test_link
```
 3711434 -rw-rw-r--  3 pomortsev pomortsev    0 мая 11 15:06 test
 3711434 -rw-rw-r--  3 pomortsev pomortsev    0 мая 11 15:06 test_link
 3711434 -rw-rw-r--  3 pomortsev pomortsev    0 мая 11 15:06 test_link2
```

3. Сделал на своей виртуалке (не Vagrant) 
```
sda                   8:0    0   200G  0 disk 
├─sda1                8:1    0     1M  0 part 
├─sda2                8:2    0   513M  0 part /boot/efi
└─sda3                8:3    0 199,5G  0 part 
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm  /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm  [SWAP]
sdb                   8:16   0     2G  0 disk 
sdc                   8:32   0     2G  0 disk 
sr0                  11:0    1  1024M  0 rom  
```
4. sudo fdisk /dev/sdb
```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): 

Using default response p.
Partition number (1-4, default 1): 
First sector (2048-4194303, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-4194303, default 4194303): +1536M

Created a new partition 1 of type 'Linux' and of size 1,5 GiB.
```
```
Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): 

Using default response p.
Partition number (2-4, default 2): 
First sector (3147776-4194303, default 3147776): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (3147776-4194303, default 4194303): 

Created a new partition 2 of type 'Linux' and of size 511 MiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
```
sda                   8:0    0   200G  0 disk 
├─sda1                8:1    0     1M  0 part 
├─sda2                8:2    0   513M  0 part /boot/efi
└─sda3                8:3    0 199,5G  0 part 
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm  /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm  [SWAP]
sdb                   8:16   0     2G  0 disk 
├─sdb1                8:17   0   1,5G  0 part 
└─sdb2                8:18   0   511M  0 part 
sdc                   8:32   0     2G  0 disk 
sr0                  11:0    1  1024M  0 rom  
```
5. sudo sgdisk -R /dev/sdc /dev/sdb
```
sda                   8:0    0   200G  0 disk 
├─sda1                8:1    0     1M  0 part 
├─sda2                8:2    0   513M  0 part /boot/efi
└─sda3                8:3    0 199,5G  0 part 
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm  /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm  [SWAP]
sdb                   8:16   0     2G  0 disk 
├─sdb1                8:17   0   1,5G  0 part 
└─sdb2                8:18   0   512M  0 part 
sdc                   8:32   0     2G  0 disk 
├─sdc1                8:33   0   1,5G  0 part 
└─sdc2                8:34   0   512M  0 part 
```
6. sudo mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b1,c1}
```
sda                   8:0    0   200G  0 disk  
├─sda1                8:1    0     1M  0 part  
├─sda2                8:2    0   513M  0 part  /boot/efi
└─sda3                8:3    0 199,5G  0 part  
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm   /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm   [SWAP]
sdb                   8:16   0     2G  0 disk  
├─sdb1                8:17   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdb2                8:18   0   512M  0 part  
sdc                   8:32   0     2G  0 disk  
├─sdc1                8:33   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdc2                8:34   0   512M  0 part  
sr0                  11:0    1  1024M  0 rom  
```
7. sudo mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b2,c2}
```
sda                   8:0    0   200G  0 disk  
├─sda1                8:1    0     1M  0 part  
├─sda2                8:2    0   513M  0 part  /boot/efi
└─sda3                8:3    0 199,5G  0 part  
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm   /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm   [SWAP]
sdb                   8:16   0     2G  0 disk  
├─sdb1                8:17   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdb2                8:18   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
sdc                   8:32   0     2G  0 disk  
├─sdc1                8:33   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdc2                8:34   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
sr0                  11:0    1  1024M  0 rom 
```
8. pvcreate /dev/sdb /dev/sdc
```
  Physical volume "/dev/md0" successfully created.
  Physical volume "/dev/md1" successfully created.
```
9. sudo vgcreate vg01 /dev/md0 /dev/md1
```
  --- Volume group ---
  VG Name               vg01
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <2,49 GiB
  PE Size               4,00 MiB
  Total PE              637
  Alloc PE / Size       0 / 0   
  Free  PE / Size       637 / <2,49 GiB
  VG UUID               P7gYkn-EVns-cdQE-Mlz8-2yE6-3W3q-sgoAUP
```
10. sudo lvcreate -L 100M vg01 /dev/md0
```
  --- Logical volume ---
  LV Path                /dev/vg01/lvol0
  LV Name                lvol0
  VG Name                vg01
  LV UUID                zo6keg-Goq6-A5Yc-1l0G-cArR-DRUX-e2uY7p
  LV Write Access        read/write
  LV Creation host, time pomor-test-ubu, 2022-05-11 16:22:52 +0700
  LV Status              available
  # open                 0
  LV Size                100,00 MiB
  Current LE             25
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:2
```
11. sudo mkfs.ext4 /dev/vg01/lvol0 
12. sudo mount /dev/vg01/lvol0 /tmp/new/
```
/dev/mapper/vg01-lvol0      90M   24K   83M   1% /tmp/new
```
13. ls -lh /tmp/new/
total 23M
drwx------ 2 root root 16K мая 11 16:45 lost+found
-rw-r--r-- 1 root root 22M мая 11 15:58 test.gz
14. lsblk
```
sda                   8:0    0   200G  0 disk  
├─sda1                8:1    0     1M  0 part  
├─sda2                8:2    0   513M  0 part  /boot/efi
└─sda3                8:3    0 199,5G  0 part  
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm   /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm   [SWAP]
sdb                   8:16   0     2G  0 disk  
├─sdb1                8:17   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
│   └─vg01-lvol0    253:2    0   100M  0 lvm   /tmp/new
└─sdb2                8:18   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
sdc                   8:32   0     2G  0 disk  
├─sdc1                8:33   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
│   └─vg01-lvol0    253:2    0   100M  0 lvm   /tmp/new
└─sdc2                8:34   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
sr0                  11:0    1  1024M  0 rom  
```
15. gzip -t ./test.gz && echo $?

0
16. sudo pvmove /dev/md0
```
sda                   8:0    0   200G  0 disk  
├─sda1                8:1    0     1M  0 part  
├─sda2                8:2    0   513M  0 part  /boot/efi
└─sda3                8:3    0 199,5G  0 part  
  ├─vgubuntu-root   253:0    0 198,5G  0 lvm   /
  └─vgubuntu-swap_1 253:1    0   976M  0 lvm   [SWAP]
sdb                   8:16   0     2G  0 disk  
├─sdb1                8:17   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdb2                8:18   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
    └─vg01-lvol0    253:2    0   100M  0 lvm   /tmp/new
sdc                   8:32   0     2G  0 disk  
├─sdc1                8:33   0   1,5G  0 part  
│ └─md0               9:0    0   1,5G  0 raid1 
└─sdc2                8:34   0   512M  0 part  
  └─md1               9:1    0  1019M  0 raid0 
    └─vg01-lvol0    253:2    0   100M  0 lvm   /tmp/new
sr0                  11:0    1  1024M  0 rom 
```                                   
17. Запутался в физических томах. Но бысто понял свою ошибку. Переместил PV обратно на RAID1, чтобы данные не потерялись
sudo mdadm /dev/md0 --fail /dev/sdb1

sudo mdadm -D /dev/md0
```
/dev/md0:
           Version : 1.2
     Creation Time : Wed May 11 16:06:38 2022
        Raid Level : raid1
        Array Size : 1569792 (1533.00 MiB 1607.47 MB)
     Used Dev Size : 1569792 (1533.00 MiB 1607.47 MB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Wed May 11 17:47:12 2022
             State : clean, degraded 
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : pomor-test-ubu:0  (local to host pomor-test-ubu)
              UUID : 9cc66808:27d419ab:473aff5e:4a4c33e7
            Events : 19

    Number   Major   Minor   RaidDevice State
       -       0        0        0      removed
       1       8       33        1      active sync   /dev/sdc1

       0       8       17        -      faulty   /dev/sdb1
```

18. md/raid1:md0: Disk failure on sdb1, disabling device.
md/raid1:md0: Operation continuing on 1 devices.

                                    
19. gzip -t /tmp/new/test.gz && echo $?
0

## Задание "3.6. Компьютерные сети, лекция 1"
              
1. 
```
HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: 940be52c-c7e9-4746-8542-5d68cfcc6e18
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Sat, 14 May 2022 08:38:51 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-ams21063-AMS
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1652517532.803121,VS0,VE75
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=480af295-e98c-e443-efc8-1bfa197bc958; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly
```
301 - сервер перенаправил запрос на HTTPS (Защищённый)

2. 
```
Request URL: http://stackoverflow.com/
Request Method: GET
Status Code: 301 Moved Permanently
Remote Address: 151.101.193.69:80
Referrer Policy: strict-origin-when-cross-origin
```
Дольше всего грузилась - https://stackoverflow.com/
https://disk.yandex.ru/i/W8T7XmyTXVJ8ng
                
3. узнал через сервис https://2ip.ru/
4. 
```
route:          90.189.220.0/24
descr:          Rostelecom networks
origin:         AS12389
```

5.
```
 4  ws.90.189.224.1.nsk.sibirtelecom.ru (90.189.224.1)  8.497 ms  8.471 ms  7.961 ms
 5  213.228.109.140 (213.228.109.140)  8.348 ms
    213.228.109.148 (213.228.109.148)  8.048 ms
    213.228.109.140 (213.228.109.140)  10.364 ms
 6  213.228.109.70 (213.228.109.70)  9.605 ms
    213.228.109.74 (213.228.109.74)  26.550 ms  9.709 ms
 7  185.140.148.155 (185.140.148.155)  48.764 ms
    185.140.148.153 (185.140.148.153)  59.976 ms
    185.140.148.155 (185.140.148.155)  51.164 ms
 8  72.14.205.132 (72.14.205.132)  51.808 ms  50.510 ms  52.293 ms
 9  * * *
10  108.170.250.33 (108.170.250.33)  60.164 ms
    66.249.95.40 (66.249.95.40)  48.853 ms  49.375 ms
11  108.170.250.34 (108.170.250.34)  49.180 ms
    108.170.250.66 (108.170.250.66)  51.100 ms
    108.170.250.113 (108.170.250.113)  69.204 ms
12  142.250.238.214 (142.250.238.214)  67.753 ms
    72.14.234.20 (72.14.234.20)  63.066 ms *
13  216.239.48.224 (216.239.48.224)  74.168 ms
    72.14.235.69 (72.14.235.69)  84.477 ms
    142.250.233.0 (142.250.233.0)  68.654 ms
14  209.85.251.63 (209.85.251.63)  63.996 ms
    142.250.56.125 (142.250.56.125)  68.468 ms
    172.253.51.241 (172.253.51.241)  67.681 ms
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  dns.google (8.8.8.8)  62.399 ms  64.559 ms  60.035 ms
```                                   
6.
```
2. AS31200  37.194.226.254                                     0.0%    86    1.4   5.7   0.7  29.7   6.4
 3. AS???    10.245.138.241                                     0.0%    86    0.9   1.1   0.8  10.4   1.1
 4. AS???    10.245.138.242                                     0.0%    86    1.3   1.8   1.0  12.8   1.9
 5. AS31200  178.49.128.2                                       0.0%    86    5.8   2.8   1.2  21.0   4.0
 6. AS9049   188.234.131.158                                    0.0%    86   43.0  43.5  42.7  63.0   2.5
 7. AS15169  72.14.214.138                                      0.0%    86   42.8  42.8  42.6  43.9   0.2
 8. AS15169  74.125.244.129                                     0.0%    86   43.7  43.7  43.5  44.4   0.2
 9. AS15169  74.125.244.133                                     0.0%    86   42.7  43.3  42.5  67.4   2.8
10. AS15169  72.14.232.84                                       0.0%    86   43.0  46.4  42.8 210.3  19.0
11. AS15169  142.251.61.219                                     0.0%    86   48.0  47.0  46.4  50.7   0.6
12. AS15169  216.239.56.101                                     0.0%    86   46.1  46.2  46.0  51.2   0.6
13. (waiting for reply)
14. (waiting for reply)
15. (waiting for reply)
16. (waiting for reply)
17. (waiting for reply)
18. (waiting for reply)
19. (waiting for reply)
20. (waiting for reply)
21. (waiting for reply)
22. AS15169  8.8.8.8                                            0.0%    85   46.0  45.9  45.9  46.4   0.1
```
7.
```
; <<>> DiG 9.16.1-Ubuntu <<>> dns.google
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 24325
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;dns.google.			IN	A

;; ANSWER SECTION:
dns.google.		22	IN	A	8.8.8.8
dns.google.		22	IN	A	8.8.4.4

;; Query time: 51 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Сб мая 14 16:37:57 +07 2022
;; MSG SIZE  rcvd: 71                                   
```     
8. dig -x 8.8.8.8
```
;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.	1615	IN	PTR	dns.google.
```
 dig -x 8.8.4.4
 ```
;; ANSWER SECTION:
4.4.8.8.in-addr.arpa.	17888	IN	PTR	dns.google.
```
   
## Задание 3.7. Компьютерные сети, лекция 2
1. Linux - ifconfig, ip a. Windows - ipconfig
2. Протокол lldp. 
lldpcli 
```
[lldpcli] $ show neighbors
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------

Interface:    eno1, via: LLDP, RID: 2, Time: 0 day, 00:00:50
  Chassis:     
    ChassisID:    mac 50:c7:bf:60:7f:3a
    SysName:      T2600G-52TS
    SysDescr:     JetStream 48-Port Gigabit L2 Managed Switch with 4 SFP Slots
    MgmtIP:       192.168.
    Capability:   Bridge, on
    Capability:   Router, on
  Port:        
    PortID:       ifname GigabitEthernet1/0/39
    PortDescr:    GigabitEthernet1/0/39 Interface
    TTL:          120
-------------------------------------------------------------------------------
```    
3. Технология VLAN используется для разделения L2 коммутатора на несколько виртуальных сетей. Стандарт IEEE 802.1q
    
modprobe 8021q - укстановка модуля ядра, для vlan
  
vim /etc/netplan/01-network-manager.yaml
```
    1 network:
    2   version: 2
    3   renderer: NetworkManager
    4   ethernets:
    5      eno1:
    6          dhcp4: no
    7          addresses:
    8          - 192.168.39.105/24
    9          gateway4: 192.168.39.1
   10          nameservers:
   11              addresses: [192.168.39.3, 192.168.39.7]
   12   vlans:
   13      vlan40:
   14         id: 3
   15         link: eno1
   16         addresses:
   17          - 192.168.40.205/24
```
```
   ping 192.168.40.205
  SEQ HOST                                     SIZE TTL TIME  STATUS             
    0 192.168.40.205                             56  64 0ms  
    1 192.168.40.205                             56  64 0ms  
    2 192.168.40.205                             56  64 0ms  
    3 192.168.40.205                             56  64 0ms  
    4 192.168.40.205                             56  64 0ms  
    5 192.168.40.205                             56  64 0ms  
    6 192.168.40.205                             56  64 0ms  
    7 192.168.40.205                             56  64 0ms  
    8 192.168.40.205                             56  64 0ms  
    9 192.168.40.205                             56  64 0ms  
```
4.  sudo apt install ifenslave
    
sudo modprobe bonding
  
```
        1 network:
    2   renderer: NetworkManager
    3   ethernets:
    4      eno1:
    5          dhcp4: false
    6          dhcp6: false
    7      wlp3s0:
    8          dhcp4: false
    9          dhcp6: false
   10   bonds:
   11      bond0:
   12         interfaces: [eno1, wlp3s0]
   13         addresses: [192.168.39.105/24]
   14         gateway4: 192.168.39.1
   15         parameters:
   16           mode: 802.3ad
                mii-monitor-interval: 1
   19         nameservers:
   20           addresses: [192.168.39.3, 192.168.39.7]
```
5. в сети с маской /29, 6 ip адресов
    
   Из сети с маской /24 получится 32 подсети с маской /29

```
Netmask:   255.255.255.248 = 29  11111111.11111111.11111111.11111 000
Wildcard:  0.0.0.7               00000000.00000000.00000000.00000 111

Network:   10.10.10.0/29         00001010.00001010.00001010.00000 000 (Class A)
Broadcast: 10.10.10.7            00001010.00001010.00001010.00000 111
HostMin:   10.10.10.1            00001010.00001010.00001010.00000 001
HostMax:   10.10.10.6            00001010.00001010.00001010.00000 110
Hosts/Net: 6                     (Private Internet)

Network:   10.10.10.8/29         00001010.00001010.00001010.00001 000 (Class A)
Broadcast: 10.10.10.15           00001010.00001010.00001010.00001 111
HostMin:   10.10.10.9            00001010.00001010.00001010.00001 001
HostMax:   10.10.10.14           00001010.00001010.00001010.00001 110
Hosts/Net: 6                     (Private Internet)

Network:   10.10.10.16/29        00001010.00001010.00001010.00010 000 (Class A)
Broadcast: 10.10.10.23           00001010.00001010.00001010.00010 111
HostMin:   10.10.10.17           00001010.00001010.00001010.00010 001
HostMax:   10.10.10.22           00001010.00001010.00001010.00010 110
Hosts/Net: 6                     (Private Internet)
```
6. можно взять подсеть  100.64.0.0/26
    
```
ipcalc 100.64.0.0/26
Address:   100.64.0.0           01100100.01000000.00000000.00 000000
Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
Wildcard:  0.0.0.63             00000000.00000000.00000000.00 111111
=>
Network:   100.64.0.0/26        01100100.01000000.00000000.00 000000
HostMin:   100.64.0.1           01100100.01000000.00000000.00 000001
HostMax:   100.64.0.62          01100100.01000000.00000000.00 111110
Broadcast: 100.64.0.63          01100100.01000000.00000000.00 111111
Hosts/Net: 62                    Class A
```    
7. Linux: Просмотр - sudo arp-scan --interface=eno1, отчистка - ip -s -s neigh flush all, удаление определенного адреса -  arp -d <ip>
   
Windows: Просмотр - arp -a, отчистка - netsh interface ip delete arpcache, удаление определенного адреса -  arp -d <ip>
    

## Задание Компьютерные сети (лекция 3

1. Routing entry for 185.158.155.0/24
  Known via "bgp 6447", distance 20, metric 10
  Tag 3257, type external
  Last update from 89.149.178.10 7w0d ago
  Routing Descriptor Blocks:
  * 89.149.178.10, from 89.149.178.10, 7w0d ago
      Route metric is 10, traffic share count is 1
      AS Hops 3
      Route tag 3257
      MPLS label: none
2. ip link add name dummy0 type dummy
```
3: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether ae:a3:d1:3d:6a:ca brd ff:ff:ff:ff:ff:ff
```
touch /etc/systemd/network/dummy0.netdev
```
[NetDev]
Name=dummy0
Kind=dummy
```    
touch /etc/systemd/network/dummy0.network
```
[Match]
Name=dummy0
[Network]
Address=192.168.0.100
Mask=255.255.255.0
```
systemctl restart systemd-networkd
```
3. dummy0: <BROADCAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether ae:a3:d1:3d:6a:ca brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.100/24 brd 192.168.0.255 scope global dummy0
       valid_lft forever preferred_lft forever
    inet6 fe80::aca3:d1ff:fe3d:6aca/64 scope link 
       valid_lft forever preferred_lft forever
```
ip route add 10.11.0.0/16 via 10.11.2.245
```
10.11.0.0/16 via 10.11.2.245 dev tun1
```
4. sudo ss -tlpn
```
State    Recv-Q    Send-Q    Local Address:Port     Peer Address:Port    Process                                                                                                                           
LISTEN     0       4096      0.0.0.0:10050            0.0.0.0:*          users:(("zabbix_agentd",pid=1145,fd=4),("zabbix_agentd",pid=1144,fd=4))
LISTEN     0       5         0.0.0.0:902              0.0.0.0:*          users:(("vmware-authdlau",pid=551317,fd=11))
LISTEN     0       4096      127.0.0.1:9101           0.0.0.0:*          users:(("node_exporter",pid=1316,fd=3))                                           
LISTEN     0       4096      127.0.0.53%lo:53         0.0.0.0:*          users:(("systemd-resolve",pid=942,fd=13))                           
LISTEN     0       128       0.0.0.0:22               0.0.0.0:*          users:(("sshd",pid=1140,fd=3))                                                     LISTEN     0       4096      [::]:10050               [::]:*             users:(("zabbix_agentd",pid=1145,fd=5))
LISTEN     0       5         [::]:902                 [::]:*             users:(("vmware-authdlau",pid=551317,fd=10)) 
LISTEN     0       128       [::]:22                  [::]:*             users:(("sshd",pid=1140,fd=4))                
```
5. sudo ss -ulpn
```
UNCONN     0       0         0.0.0.0:39631            0.0.0.0:*          users:(("openvpn",pid=1320,fd=4))                                   
UNCONN     0       0         172.19.0.1:56331         0.0.0.0:*          users:(("chrome",pid=660133,fd=229))                                
UNCONN     0       0         10.11.2.246:48225        0.0.0.0:*          users:(("systemd-resolve",pid=942,fd=12))                               
UNCONN     0       0         0.0.0.0:58742            0.0.0.0:*          users:(("openvpn",pid=1318,fd=5))                                   
UNCONN     0       0         0.0.0.0:1701             0.0.0.0:*          users:(("xl2tpd",pid=1426,fd=3))                                    
```
6. https://disk.yandex.ru/i/WonIdOFopDR8bw

## Задание 3.9. Элементы безопасности информационных систем
 
1. https://disk.yandex.ru/i/3_PPjrDObxrtfw
 
2. https://disk.yandex.ru/i/xUZ_tQe3uW3pJA
 
3. apt install apache2

ufw allow 'Apache'
    
mkdir /var/www/test
 
vim /var/www/test/index.html
```
 <html>
    <head>
        <title>Its done for homework</title>
    </head>
    <body>
        <h1>Success! The  virtual host is working!</h1>
    </body>
</html>
```
vim /etc/apache2/sites-available/test.conf    
```
<VirtualHost *:80>
        ServerName www.test.its-sib.ru
        Redirect "/" "https://test.its-sib.ru/"
</VirtualHost>
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
        ServerName test.its-sib.ru
        ServerAdmin ppe@its-sib.ru
        DocumentRoot /var/www/test/
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        SSLEngine on
        SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
        SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
    <FilesMatch "\.(cgi|shtml|phtml|php)$">
        SSLOptions +StdEnvVars
    </FilesMatch>
    <Directory /usr/lib/cgi-bin>
        SSLOptions +StdEnvVars
    </Directory>
        BrowserMatch "MSIE [2-6]" \
        nokeepalive ssl-unclean-shutdown \
        downgrade-1.0 force-response-1.0
    </VirtualHost>
</IfModule>
```
a2ensite test.conf

apache2ctl configtest
    
systemctl restart apache2
    
https://disk.yandex.ru/i/F6lBgTeZ0I88bQ
    
https://disk.yandex.ru/i/qbWVNI1s4wa2kA
    
4. https://disk.yandex.ru/i/8UqIwN9atrbRXA
 
5. ssh-keygen 
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/pomortsev/.ssh/id_rsa): ubuntutest         
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in ubuntutest
Your public key has been saved in ubuntutest.pub
The key fingerprint is:
SHA256:rH1+qUmwBJsb+mFYB8lBqvUL0fg1wo8WUZnc77u3A9E pomortsev@IT-POMOR-UBU
The key's randomart image is:
+---[RSA 3072]----+
|     .+o.+       |
|     * ++ .      |
|    = X o  . .   |
|   o + % .  o E  |
|  . . X S  . .   |
|     * O o  o    |
|    o * o o  +   |
|     o . + .+ o  |
|      .   +o.o.o |
+----[SHA256]-----+
```
cat ~/.ssh/ubuntutest.pub | ssh -i ~/.ssh/pomortsev root@192.168.39.213 "mkdir -p ~/.ssh && cat >> /root/.ssh/authorized_keys"
 
ssh -i ~/.ssh/ubuntutest root@192.168.39.213
```
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-33-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 updates can be applied immediately.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Tue May 24 14:10:24 2022 from 192.168.39.105
```

6. vim ~/.ssh/config
```
Host ubutest
    HostName 192.168.39.213
    User root
    IdentityFile ~/.ssh/ubuntutest
```
ssh ubutest 
```
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-33-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 updates can be applied immediately.

Last login: Wed May 25 10:11:15 2022 from 192.168.39.105
```
tcpdump -i eno1 -c 100 -w dump.pcap

https://disk.yandex.ru/i/_4Q_pDbf4nHKNw
