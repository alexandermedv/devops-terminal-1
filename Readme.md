Добрый день, нужна помощь в выполнении задания по работе в терминале.

Возможно, потому что никогда не работал в WSL, встретил много неожиданных вещей в ходе выполнения задания.
Никак не получается запустить vagrant в терминале через wsl. 
Я выполнил установку дистрибутивов для windows и linux, затем создал директорию для vagrant, выполнил в ней vagrant init и заменил содержимое файла, как описано в задании.
Также выполнил установку пакета linux-headers-generic (sudo apt-get install linux-headers-generic). 
Далее при запуске vagrant up все пошло не так, описываю последовательность действий, что я пробовал.

1. vagrant up
![img_1.png](img_1.png)
   
2. vagrant up --provider=virtualbox
![img_2.png](img_2.png)
   
3. VBoxManage --version
![img_3.png](img_3.png)
   
4. sudo /sbin/vboxconfig
![img_4.png](img_4.png)
   
5. sudo apt-get install linux-headers-generic
![img_5.png](img_5.png)
   
6. uname -r
sudo apt-get install linux-headers-5.10.16.3-microsoft-standard-WSL2
   ![img_6.png](img_6.png)