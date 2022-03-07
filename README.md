1. Определения

   Gearman предоставляет универсальную платформу приложений для передачи работы другим машинам или процессам.

   Worker -- отдельный контекст для выполнения фоновых задач, который не блокирует UI.

2. Сокращения (не требовались)

3. Объект исследования (описание)

   Объектом исследования, является механизм воркеров для запуска обработки скриптов в отдельных процессах или на удалённых машинах, по средствам `Gearman`.

4. Цель работы
   
   Освоить базовый функционал для работы с воркерами на примере запуска команды `wc` и скрипта для вывода аргументов в консоль (`script.sh`). 
   
   Листинг `script.sh`:
   ```bash
    #!/bin/sh
    echo "got args: $*"
   ```

5. Методология/порядок проведения работ

   1. Настроить и запустить gearmand.
   2. Запустить несколько воркеров с помощью команды `gearman -w -f <function_name> <command>` для `wc` и `script.sh`.
      При этом для `script.sh` необходимо воспользоваться командой `xargs` для корректной передачи аргументов. Пример: `gearman -w -f func xargs ./script.sh <worker index>` 
   3. В отдельном терменале запустить запрос на выполнение команды `wc` и `script.sh`. Пример: `gearman -f func <..args..>`.

6. Описание результата

   Запуск `script.sh` (`func -- <function_name>`)

   Формат вывода: `got args: <worker index> <comand arg>`

   ```console
   libuser@ds100:~ % gearman -f func "1 2 3" "2 3 4" "3 4 5" "4 5 6"  "5 6 7" "6 7 8" "7 8 9" "8 9 10" "1 2 3" "2 3 4" "3 4 5" "4 5 6" "5 6 7" "6 7 8" "7 8 9" "8 9 10" "1 2 3" "2 3 4" "3 4 5" "4 5 6" "5 6 7" "6 7 8" "7 8 9" "8 9 10"
   got args: 1 1 2 3
   got args: 1 2 3 4
   got args: 1 3 4 5
   got args: 1 4 5 6
   got args: 1 5 6 7
   got args: 1 6 7 8
   got args: 1 7 8 9
   got args: 1 8 9 10
   got args: 2 1 2 3
   got args: 2 2 3 4
   got args: 2 3 4 5
   got args: 2 4 5 6
   got args: 2 5 6 7
   got args: 2 6 7 8
   got args: 2 7 8 9
   got args: 2 8 9 10
   got args: 2 1 2 3
   got args: 1 2 3 4
   got args: 2 3 4 5
   got args: 1 4 5 6
   got args: 2 5 6 7
   got args: 1 6 7 8
   got args: 1 7 8 9
   got args: 2 8 9 10
   ```

   Видно, что до определённого момента воркера №1 достаточно для выполнения скрипта, однако позже подключается второй воркер и в итоге задача распределяется между ними. 
   
7. Ссылки

   - [Gearman (job queue manager) command line tool: routing the workload to the worker process arguments with xargs](https://www.stefaanlippens.net/gearman_setting_worker_process_arguments_through_xargs/)
