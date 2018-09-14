## Как общаться в сети через Android-смартфон

Стандартные веб-интерфейсы вполне оптимизированы для мобильных устройств, но иногда хочется использовать клиенты. Итак, что мы можем предложить:

#### Клиент IDEC Mobile
* *Для ОС версии 4.4 и новее*

Самый лучший выбор. Новые сборки появляются практически каждую неделю. Клиент находится в состоянии публичной беты.

Из полезностей:

* Многосерверность
* Хранение сообщений оффлайн
* Полная поддержка расширений IDEC
* Удобная настройка сетевых параметров, например, timeout и http-прокси
* Интеграция с Tor через Orbot
* Правка сообщений внешним редактором на выбор
* Разные темы оформления
* Полнотекстовый поиск
* Поддержка чёрного списка
* Прозрачная работа с БД: чистка содержимого эх, экспорт и импорт сообщений

Ссылки:

* [Github](https://github.com/vit1-irk/idec-mobile)
* [APK текущей версии](https://ii-net.tk/ii/files/app-debug.apk)

#### Клиент Цезий
* *Для ОС версии 5.0 и новее*

Чтобы заставить работать Цезий на телефоне, придётся приложить немного усилий, но это того стоит. Если ни разу не пробовали этот клиент, то прочитайте его [README](https://github.com/spline1986/caesium/blob/master/README.md).

Скачиваем 2 приложения, устанавливаем:

* [Termux](https://f-droid.org/repository/browse/?fdid=com.termux)
* [Hacker's Keyboard](https://f-droid.org/repository/browse/?fdid=org.pocketworkstation.pckeyboard)

Ставим Hacker's Keyboard системной клавиатурой по-умолчанию.

Запускаем Termux, вводим:

```
# подготавливаем
apt update
apt upgrade
apt install python nano git patch
git clone https://github.com/spline1986/caesium
cd caesium
cp caesium.def.cfg caesium.cfg

# правим конфиг согласно README
nano caesium.cfg

# патчим файл клавиш
patch caesium.py < keys_android.patch

# либо правим caesium.py и меняем
# from keys на from keys_android в первых строках

# запускаем клиент
python caesium.py
```

После того, как убедились, что клиент работает, можете установить [Termux:Widget](https://f-droid.org/repo/com.termux.widget_3.apk), чтобы можно было запускать его прямо с домашнего экрана.

###### Скриншоты клиента:

![Окно выбора эх](http://ii-net.tk/ii/files/t7GeiEBgGQuidT1l9TZ5.png)
![Просмотр сообщений](http://ii-net.tk/ii/files/799HWLjT0v6E7bwkPbHe.png)

##### ServerListener - приложение для отслеживания новых сообщений
* *Рекомендуется Android 4.2 или новее*

* APK: <https://ii-net.tk/ii/files/serverlistener-29.07.2016.apk>
* [Github-репозиторий](https://github.com/vit1-irk/ServerListener)

Открыв приложение, надо выбрать в выпадающем меню протокол /x/c, а в адрес сервера написать строку в подобном формате:

`https://ii-net.tk/ii/ii-point.php?q=/x/c/pipe.2032/ii.14/mlp.15/tmp.red.eyes`

и нажать кнопку для применения параметров. Если на телефоне настроен Termux и Termux.Widget вместе с Цезием, то можно интегрировать их в уведомления, выбрав нужный ярлык запуска через те же быстрые настройки.

После этого ServerListener начнёт заходить на сервер каждые N минут, проверять поступление сообщений в указанных эхоконференциях и бросать уведомления.

#### Другие клиенты

Любители экзотики могут не отчаиваться: можно завести другие клиенты с помощью Termux.

##### iitxt

Устанавливается похожим способом, как и Цезий. [README](https://github.com/spline1986/iitxt/blob/master/README)

```
apt update
apt upgrade
apt install python nano git
git clone https://github.com/spline1986/iitxt

# дальше смотрим в README и читаем, как пользоваться
```

##### Текстовый клиент на Си
* *Любая версия ОС*

Для этого клиента даже не понадобится Termux: подойдёт любой эмулятор терминала. Не позабудьте выставить в терминале нужный `LD_PRELOAD_PATH` для библиотек.

Есть готовая сборка, но также можно собрать его самому через кросс-компилятор (Android NDK) или с помощью GCC из Termux.

* Архив с готовой сборкой: <http://ii-net.tk/files/iitxt-c.tar.gz>
* [Репозиторий](https://github.com/vit1-irk/iitxt-c) с кодом и инструкцией по использованию
