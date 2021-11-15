Добрый день, нужен совет по выполнению задания по работе в терминале.

Возможно, потому что никогда не работал в WSL, встретил много неожиданных вещей в ходе выполнения задания.
Никак не получается запустить vagrant в терминале через wsl. 

Я выполнил установку дистрибутивов для windows и linux, затем создал директорию для vagrant, выполнил в ней **vagrant init** и заменил содержимое файла, как описано в задании.
Также выполнил установку пакета linux-headers-generic (**sudo apt-get install linux-headers-generic**). 
Далее при запуске **vagrant up** все пошло не так, описываю последовательность действий, что я пробовал.

1. **vagrant up**
![img_1.png](img_1.png)
   
2. **vagrant up --provider=virtualbox**
![img_2.png](img_2.png)
   
3. **VBoxManage --version**
![img_3.png](img_3.png)
   
4. **sudo /sbin/vboxconfig**
![img_4.png](img_4.png)
   
5. **sudo apt-get install linux-headers-generic**
![img_5.png](img_5.png)
   
6. **uname -r**
**sudo apt-get install linux-headers-5.10.16.3-microsoft-standard-WSL2**
   ![img_6.png](img_6.png)
   
Может быть, у Вас есть идеи, с чем могут быть связаны эти проблемы, т.к. хочется разобраться, но интернет уже не дает явных подсказок. 
Заранее спасибо!



---
**Добавил скриншоты установки по полученным материалам.**

Убедился, что выполнен шаг с установкой VirtualBox для Windows (Установлен в папку C:\Soft):
![img_16.png](img_16.png)

Удалил имеющийся vagrant:
![img_7.png](img_7.png)

Установил его заново по инструкции:
![img_10.png](img_10.png)

Выполнил настройку и инициализацию vagrant:
![img_11.png](img_11.png)

Заменил содержимое Vagrantfile:

![img_12.png](img_12.png)

Запускаю vagrant и получаю прежние ошибки:
![img_13.png](img_13.png)

Найти решение по подсказкам в сообщениях ошибок также не получается:
![img_14.png](img_14.png)
![img_15.png](img_15.png)

Подозреваю, что проблема с VirtualBox, безуспешно пытался решить ее:
https://stackoverflow.com/questions/60350358/how-do-i-resolve-the-character-device-dev-vboxdrv-does-not-exist-error-in-ubu
![img_17.png](img_17.png)
![img_18.png](img_18.png)
![img_19.png](img_19.png)
![img_20.png](img_20.png)
![img_21.png](img_21.png)
------
После переустановки VirtualBox в директорию по умолчанию результат прежний:
![img_22.png](img_22.png)
Версия Linux:
![img_24.png](img_24.png)
---
Системная переменная VBOXINSTALLPATH:

У меня не было переменной VBOXINSTALLPATH, но была похожая VBOX_MSI_INSTALL_PATH со значением C:\Program Files\Oracle\VirtualBox\
![img_25.png](img_25.png)
Создал также VBOXINSTALLPATH с тем же значением:
![img_26.png](img_26.png)
Параметр secure boot в BIOS я отключил несколько дней назад в ходе своих попыток:
![img_27.png](img_27.png)
Результат запуска vagrant up не изменился:
![img_28.png](img_28.png)

---
Установил vagrant без WSL. Пришлось даунгрейдить VirtualBox, о версии 6.1.26, т.к. 6.1.28 видимо еще сырая, у меня в ней даже не запустилась виртуальная машина. После этого получилось:

5. Графический интерфейс VirtualBox:
![img_29.png](img_29.png)
   По умолчанию выделены следующие ресурсы:
   ОЗУ 1024 МБ,
   Процессоры: 2
   Память: 64 ГБ
   Видеопамять: 4МБ
   
6. Для того, чтобы изменить имя виртуальной машины и удвоить количество памяти и процессоров, Vagrantfile будет выглядеть так:
 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
 	config.vm.provider "virtualbox" do |v|
      v.name = "my_vm"
    end
    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 4
    end
 end
   
Результат:
![img_30.png](img_30.png)

7. Зашел в виртуальную машину через vagrant ssh. Дальнейшие действия делаем в ней. Например, man bash:
![img_31.png](img_31.png)
   
8. Заходим в man bash.
С помощью команды /history, переключаясь между результатами клавишей N, ищем команду для изменения длины журнала history:
   ![img_32.png](img_32.png)
   
   Нужная нам строка:  The text of the last  HISTSIZE  commands (default  500)  is  saved.
Нужная переменная HISTSIZE, номер нужной строки: 2602.
   Директива ignoreboth удаляет из истории повторяющиеся записи.
   
9. Скобки {} обозначают список команд, который будет исполнен в текущем терминале shell. Список должен заканчиваться точкой с запятой. Описано в разделе Compound Commands в строке 230.

10. Команда touch {0..100000} успешно выполняется.
Команда touch {0..300000} завершается ошибкой:
    -bash: /usr/bin/touch: Argument list too long
    
Опытным путем получаем, что максимальная длина списка аргументов для этой команды равна 147058.
11. Ищем по ключу в мануале bash и находим ответ в строке 240:

[[ expression ]]
              Return a status of 0 or 1 depending on the evaluation of the conditional expression expression.  Expressions are composed of the primaries
              described below under CONDITIONAL EXPRESSIONS.  Word splitting and pathname expansion are not performed on the words between  the  [[  and
              ]]; tilde expansion, parameter and variable expansion, arithmetic expansion, command substitution, process substitution, and quote removal
              are performed.  Conditional operators such as -f must be unquoted to be recognized as primaries.
              When used with [[, the < and > operators sort lexicographically using the current locale.

А также в строке 1587:

-d file
              True if file exists and is a directory.

Выражение [[ -d /tmp ]] проверяет существование директории /tmp и возвращает True, если существует.

12. Похожий результат получился по команде 
    alias bash=/tmp/new_path_directory/bash:
    
![img_33.png](img_33.png)

13. Команда at используется для назначения одноразового задания на заданное время, а команда batch — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше порогового уровня.

14. vagrant halt