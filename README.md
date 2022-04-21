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
