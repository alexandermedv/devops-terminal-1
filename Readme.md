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