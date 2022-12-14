# MAC-адреса сетевых устройств
## Топология  
![](Topology_mac.png)  
## Таблица адресации  
Таблица адресации приведена ниже:  
|Устройство|Интерфейс|IP-адрес|Маска подсети|
|:--------:|:-------:|:------:|:-----------:|
|S1|VLAN 1|192.168.1.11|255.255.255.0|
|S2|VLAN 1|192.168.1.12|255.255.255.0|
|PC-A|NIC|192.168.1.1|255.255.255.0|
|PC-B|NIC|192.168.1.2|255.255.255.0|

## Цели  
### Часть 1. Создание и настройка сети  
#### Подключите сеть в соответствии с топологией.  
#### Настройте узлы ПК.  
#### Выполните инициализацию и перезагрузку коммутаторов.  
#### Настройте базовые параметры каждого коммутатора.  
**Настройте имена устройств в соответствии с топологией.**  
**Настройте IP-адреса, как указано в таблице адресации.**  
**Назначьте cisco в качестве паролей консоли и VTY.**  
**Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.**  
Ниже приведен пример настройки коммутатора S1:  
![](configure_S1.png)  
Настройки всех сетевых устройств приведены [здесь](Configs/).  
### Часть 2. Изучение таблицы МАС-адресов коммутатора  
#### Запишите МАС-адреса сетевых устройств.  
**Откройте командную строку на PC-A и PC-B и введите команду *ipconfig /all.***  
![](ipconfig_all_PC-A_and_PC-B.png)  
Назовите физические адреса адаптера Ethernet.  
**MAC-адрес компьютера PC-A:**  
0001.4288.1316  
**MAC-адрес компьютера PC-B:**  
0002.4A6D.905D  
**Подключитесь к коммутаторам S1 и S2 через консоль и введите команду *show interface F0/1* на каждом коммутаторе.**  
Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).  
**МАС-адрес коммутатора S1 Fast Ethernet 0/1:**  
0009.7c90.b301  
**МАС-адрес коммутатора S2 Fast Ethernet 0/1:**  
0001.4200.0201  
#### Просмотрите таблицу МАС-адресов коммутатора.  
Подключитесь к коммутатору S2 через консоль и просмотрите таблицу МАС-адресов до и после тестирования сетевой связи с помощью эхо-запросов.  
**Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.**  
Откройте окно конфигурации  
**В привилегированном режиме EXEC введите команду show mac address-table и нажмите клавишу ввода.**  
S2# show mac address-table  
![](show_mac_sddress-table_S1_and_S2.png)  
_Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?_  
Да, записаны.  
_Какие МАС-адреса записаны в таблице? С какими портами коммутатора они сопоставлены и каким устройствам принадлежат?_  
Mac-адрес интерфейса VLAN 1 на порту коммутатора f0/1  
Игнорируйте МАС-адреса, сопоставленные с центральным процессором.  
_Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды *show mac address-table*?_  
Можно посмотреть с какого номера интерфейса виден МАС-адрес, указанный в таблице и посмотреть к какому устройству подключен данный интерфейс.  
_Работает ли это решение в любой ситуации?_  
Нет, данное устройство не в любой ситуации работает. Например, если за устройством, которое подключено к нашему интерфейсу расположено другое устройство. На нашей схеме можно привести следующий пример: к порту F0/1 коммутатора S2 подключен S1, а за ним еще подключен PC-A. В нашей таблице может появиться 2 МАС-адреса, которые видны через порт F0/1. 
#### Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.  
**В привилегированном режиме EXEC введите команду *clear mac address-table dynamic* и нажмите клавишу Enter.**  
S2# clear mac address-table dynamic  
![](clear_mac_sddress-table_S1_and_S2.png)  
**Снова быстро введите команду *show mac address-table*.**  
_Указаны ли в таблице МАС-адресов адреса для VLAN 1?_  
Нет, МАС-адреса для VLAN1 нет. Указан МАС-адрес интерфейса F0/1 коммутатора.  
МАС-адреса интерфейсов VLAN 1 выглядят так:  
![](show_interfaces_vlan1.png)  
_Указаны ли другие МАС-адреса?_  
Нет, других MAС-адресов нет.  
Через 10 секунд введите команду *show mac address-table* и нажмите клавишу ввода.  
_Появились ли в таблице МАС-адресов новые адреса?_  
Нет, новые МАС-адреса не появились даже спустя несколько минут.  
#### С компьютера PC-B отправьте эхо-запросы устройствам в сети и просмотрите таблицу МАС-адресов коммутатора.  
**На компьютере PC-B откройте командную строку и еще раз введите команду *arp -a*.**  
![](arp-a_PC-B.png)  
На другом компьютере вывод получился такой:  
![](arp-a_work.png)  
_Не считая адресов многоадресной и широковещательной рассылки, сколько пар IP- и МАС-адресов устройств было получено через протокол ARP?_    
Появился только 1 IP- и МАС-адрес.   
**Из командной строки PC-B отправьте эхо-запросы на компьютер PC-A, а также коммутаторы S1 и S2.**  
![](ping_from_PC-B_to_PC-A_and_S1_and_S2.png)  
_От всех ли устройств получены ответы?_  
Да, ответы получены от всех устройств.  
**Если нет, проверьте кабели и IP-конфигурации.**  
**Закройте командную строку.**  
**Подключившись через консоль к коммутатору S2, введите команду *show mac address-table*. Откройте окно конфигурации.**  
![](show_mac_sddress-table_S2_after_ping_all_device.png)  
_Добавил ли коммутатор в таблицу МАС-адресов дополнительные МАС-адреса?_  
Да, коммутатор добавил в таблицу новые МАС-адреса.  
_Если да, то какие адреса и устройства?_  
 В таблицу добавились МАС-адреса Vlan1 коммутатора S1, МАС-адрес РС-А, МАС-адрес интерфейса F0/1 коммутатора S1 и МАС-адрес РС-В.  
**На компьютере PC-B откройте командную строку и еще раз введите команду *arp -a*.**  
![](arp-a_PC-B_after_ping_all_device.png)  
**Появились ли в ARP-кэше компьютера PC-B дополнительные записи для всех сетевых устройств, которым были отправлены эхо-запросы?**  
Да, появились дополнительныые записи (если сравнивать с первонвчальным [arp -a](arp-a_PC-B.png)).  
### Вопрос для повторения  
В сетях Ethernet данные передаются на устройства по соответствующим МАС-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы МАС-адресов. Если компьютеров в сети немного, эта процедура выглядит достаточно простой.  
_Какие сложности могут возникнуть в крупных сетях?_  
Если в сети много устройств, то во внутренней памяти может не хватить места для записи таблицы МАС-адресов всех устройств. Из-за этого может дольше происходить передача данных по сети, т.к. потребуется дополнительное время для построения пути по которому будет передаваться пакет данных.  
