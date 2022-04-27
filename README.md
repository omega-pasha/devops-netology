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

4. Зомби не занимают памяти (как процессы-сироты), но блокируют записи в таблице процессов, размер которой ограничен для каждого пользователя и системы в целом.

5. 






