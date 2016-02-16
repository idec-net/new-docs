## Как общаться в сети через Android-смартфон

Стандартные веб-интерфейсы хорошо оптимизированы для мобильных устройств, но иногда хочется использовать клиенты. Итак, что мы можем предложить:

#### Стандартный Android-клиент для ii
* *Для ОС версии 4.0.3 и новее*

Родом из 2014 года, содержит в себе кучу багов, давно не обновлялся. Возможно хранение сообщений в ОЗУ или sqlite. Требуется дополнительная настройка (спрашивайте у сисопа вашей станции).

* [Ссылка 1](https://yadi.sk/d/iCL2ob75cfykh)
* [Ссылка 2](https://yadi.sk/d/zF477StyZ8NWX)

#### Клиент Цезий
* *Для ОС версии 5.0 и новее*

Чтобы заставить работать Цезий на телефоне, придётся приложить немного усилий, но это того стоит. Если ни разу не пробовали этот клиент, то прочитайте его [README](https://github.com/spline1986/caesium/blob/master/README).

Скачиваем 2 приложения, устанавливаем:

* [Termux](https://f-droid.org/repo/com.termux_29.apk)
* [Hacker's Keyboard](https://f-droid.org/repo/org.pocketworkstation.pckeyboard_1038002.apk)

Ставим Hacker's Keyboard системной клавиатурой по-умолчанию.

Запускаем Termux, вводим:

```
# подготавливаем
apt update
apt upgrade
apt install python nano git
git clone https://github.com/spline1986/caesium
cd caesium
cp caesium.def.cfg caesium.cfg

# правим конфиг согласно README
nano caesium.cfg

# патчим файл клавиш
# пока патча нет =)

# запускаем клиент
python caesium.py
```

После того, как убедились, что клиент работает, можете установить [Termux:Widget](https://f-droid.org/repo/com.termux.widget_3.apk), чтобы можно было запускать его прямо с домашнего экрана.

###### Скриншоты клиента:

![Окно выбора эх](http://ii-net.tk/ii/files/t7GeiEBgGQuidT1l9TZ5.png)
![Просмотр сообщений](http://ii-net.tk/ii/files/799HWLjT0v6E7bwkPbHe.png)
