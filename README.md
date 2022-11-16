# ST-Link-v2-clone-to-v2.1

Magic: QWK2tn+fM.EdjX6z

# Upgrade to J-Link?
https://mhhauto.com/Thread-Flash-ST-Link-Clone-to-J-link-STlink-Jlink

# Other resources:

* Translated to Eng: https://sudonull.com/post/32259-Making-ST-Link-V21-from-Chinese-ST-Link-V2

* Original article: https://webcache.googleusercontent.com/search?q=cache:d1aophrRn6cJ:https://habr.com/ru/post/442290/&cd=2&hl=en&ct=clnk&gl=ba&client=firefox-b-d

Делаем ST-Link V2.1 из китайского ST-Link V2
Программирование микроконтроллеров *DIY или Сделай сам Лайфхаки для гиков Электроника для начинающих
Из песочницы
Tutorial
Привет, Хабр!

В данной статье расскажу как модифицировать ST-Link V2 до ST-Link V2.1.

Возможно для кого-то это не будет новостью, но особой инфы по данной теме в инете не нашел.

Кому интересно — прошу под кат.
Предисловие

Так уж случилось, что мне надоели лишние провода.

Немного подумав я вспомнил что на платах Nucleo и Discovery — ST-Link совмещает в себе SWD и VCP (Virtual Com Port).

Первое что пришло в голову — купить самую дешевую из подобных плат, попытаться сдампить прошивку в обход защиты и залить в программатор из китая, либо же развести новую плату.
Однако мне подсказали ссылку на GitHub с уже вытянутым загрузчиком, в итоге получилось то что получилось.

Приступаем к работе

Модификацию можно произвести только на версии софта под Windows, кроссплатформенная версия софта отказывается обновлять девайс!

Есть несколько вариантов модификации, и часть из них нельзя сделать если чип не подходящий (не хватит памяти).

Например, модификацию STM32+MSD+VCP можно сделать только если чип STM32F1xxCBxx, однако у нее есть аналог STM32+Audio, который даст STM32+VCP (в принципе что нам и требуется).

Понадобится:

— Паяльник;
— Мультиметр с прозвонкой;
— ПК с ОС Windows (может получится через Wine, не пробовал);
— Архив с нужным софтом и бутлоадером (PASS: QWK2tn+fM.EdjX6z).
— Китайский клон ST-Link V2;
— USB-UART адаптер либо второй ST-Link.

Вскрываем...

Платы и чипы во всех разные




Прошивка

Есть два пути — USB-UART (немного сложнее) либо второй ST-Link.

USB-UART


1) Прозвонкой находим резистор который подключен к BOOT0.
Делаем перемычку от стороны этого резистора которая подключена к BOOT0 к 3.3v.

PA9(TX) может быть подключен к светодиоду или резистору рядом с ним, потому прозваниваем.

Подпаиваем UART на PA9(TX) и PA10(RX).

Я делал это так:



Так же подпаиваем питание.

Прошиваем загрузчик Protected-2-1-Bootloader.bin с помощью STM32 Flash loader demonstrator.

После прошивки отпаиваем перемычку, PA9 и PA10 (PA10 оставляем если хотим вывести SWO).

ST-Link

На платах есть по 4 контакта, в некоторых случаях они уже промаркированы, в противном же случае прозваниваем их относительно PA13(SWDIO) и PA14(SWCLK), подпаиваемся вторым ST-Link.



Так же подпаиваем питание.

Устанавливаем STM32 ST-LINK Utility V4.3 из архива, снимаем защиту от записи и прошиваем загрузчик Protected-2-1-Bootloader.bin.

Для снятия защиты в программе STM32 ST-LINK Utility жмем Target > Option Bytes, переключаем Read Out Protection в Disabled и жмем Apply.

Обновление до ST-Link V2.1

После прошивки подключаем прошитый ST-Link (уже почти V2.1) к ПК.

В программе STM32 ST-LINK Utility V4.3 жмем ST-LINK > Firmware update.

Жмем Device Connect — получаем список возможных модификаций:

Выбираем нужную вам модификацию, в моем случае STM32+MSD+VCP, жмем Yes >>>>.

Ждем пока завершится обновление…



Профит!

Завершающая часть

Так как SWIM и RST после такой модификации не работают — отрезаю их.

Так же отрезаю дублирующие 5V и 3.3V.

Получается 4 свободных пина.

На них подпаиваюсь проводками к чипу:

PA10 -> SWO
PB0 -> NRST
PA3 -> RX
PA2 -> TX

Вывожу все на основной разъем, на оставшиеся свободные пины.

Получилась такая распиновка:



Мой девайс после модификации




Накарябал скальпелем маркировку на корпусе:



Не забываем отмыть плату после пайки!

В итоге, в ПК девайс определяется так:





Я без понятия чему равен объем виртуальной флешки (в данном случае к ST-Link V2.1 был подключен F103C8).

Если на нее закинуть файл прошивки — программатор прошьет чип без программ.

Проверяем VCP:



Спасибо за внимание!
При копировании попрошу оставлять ссылочку на первоисточник.

С вопросами обращайтесь в комментарии, чем смогу — помогу.
