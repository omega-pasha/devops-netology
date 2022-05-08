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
