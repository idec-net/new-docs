# IDEC (ii-like Data Exchange Convention)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [IDEC(ii-like Data Exchange Convention)](#idecii-like-data-exchange-convention)
    - [Names convention](#names-convention)
    - [Bundle](#bundle)
        - [Bundle message format](#bundle-message-format)
            - [Example](#example)
    - [Point messages format](#point-messages-format)
        - [Example](#example)
- [API(HTTP)](#apihttp)
    - [Base methods](#base-methods)
        - [Location /u/](#location-u)
            - [GET /u/e/echoe.1/echoe.2/.../echoe.n](#get-ueechoe1echoe2echoen)
                - [Example](#example)
            - [GET /u/m/msgid/msgid/msgid/.../msgidN](#get-ummsgidmsgidmsgidmsgidn)
                - [Example](#example)
            - [GET /u/point/pauth/tmsg](#get-upointpauthtmsg)
                - [Example](#example)
            - [POST /u/point](#post-upoint)
                - [Example](#example)
        - [Location /e/](#location-e)
            - [GET /e/echo.1](#get-eecho1)
                - [Example](#example)
        - [Location /m/](#location-m)
            - [GET /m/msgid](#get-mmsgid)
                - [Example](#example)
    - [Extensions](#extensions)
        - [GET /list.txt](#get-listtxt)
            - [Example](#example)
        - [GET /x/features](#get-xfeatures)
            - [Example](#example)
        - [GET /x/c/echo.1/echo.2](#get-xcecho1echo2)
            - [Example](#example)
        - [POST /u/push](#post-upush)
            - [Answers](#answers)
            - [Errors](#errors)
            - [Example](#example)
        - [GET /blacklist.txt](#get-blacklisttxt)
            - [Example](#example)
        - [GET /u/e/echo.1/echo.2/offset:limit](#get-ueecho1echo2offsetlimit)
            - [Example](#example)
        - [Files](#files)
            - [GET /x/filelist/pauth](#get-xfilelistpauth)
                - [Example](#example)
            - [POST /x/filelist](#post-xfilelist)
                - [Example](#example)
            - [GET /x/file/filename](#get-xfilefilename)
                - [Example](#example)
            - [POST /x/file](#post-xfile)
                - [Example](#example)

<!-- markdown-toc end -->


## Names convention

  * echoarea name length is from 3 to 120 symbols; must contain a dot <.>: ```music.14```
  * msgID is a unique 20-symbol piece of base64-encoded sha256 hash. Special base64 symbols like `+` and `/` must be replaced by readable letters (like A and Z for example).

## Bundle

Bundle is full base64-encoded message with unique ID (or list of those messages).

### Bundle raw message format

  1. tags
  2. echoarea
  3. unixtime
  4. msgfrom
  5. address
  6. msgto
  7. subject
  8. empty line
  9. message text

#### Example

```
ii/ok/repto/IZXhLBKJx0rhx0lXYu3L
im.16
1455789357
Vasya
Lunar, 2
Pupkin
Re: Мое первое сообщение в эху

текст
сообщения
```

## Point messages format

  1. echoarea
  2. msgto
  3. subject
  4. empty string
  5. repto
  6. message text

If repto starts with ```@repto:```, the node places the tag *repto*.  Otherwise the string belongs to the message text.

**WARNING**

  * Empty fields not allowed
  * Only one message to one request
  * Maximum message size is 64k.
  
### Example

```
im.16
All
Тестируем

@repto:2hEUbMAxKSA83vcmgU4s
И вот я пишу своё первое письмо в нашу секту.
Меня видно?
```

# API(HTTP)
## Base methods
### Location /u/
#### GET /u/e/echoe.1/echoe.2/.../echoe.n

Return list of msgIDs related to specified echoareas.

##### Example
```
curl -XGET https://dynamic.lessmore.pw/idec/u/e/music.14/python.15
music.14
XA71p66PAoKAVgAzqM0I
W256C6NAHFTKHQMG7FTH
...SKIP...
python.15
l2dwnaJVhSHfu9btNMkz
WMJaWs1uKJZXkGTXL8Qp
...SKIP...
```

#### GET /u/m/msgid/msgid/msgid/.../msgidN

The bundle. The primary way to receive messages on the client and on the server.

Return format: *msgid(string):(delimiter)base64 encoded message(string)*

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/u/m/k37ndQLS4e8P9GsZmOAz
k37ndQLS4e8P9GsZmOAz:aWkvb2sKbXVzaWMuMTQKMTQwNTQ4MTI3NApzcGxpbmUKc3RhdGlvbjEzLCAxCkFsbAptdXNpYy4xNAoKCtCt0YXQvtC60L7QvdGE0LXRgNC10L3RhtC40Y8g0L/QvtGB0LLRj9GJ0LXQvdCwINC+0LHRgdGD0LbQtNC10L3QuNGOINC80YPQt9GL0LrQuCwg0LXRkSDRgdC+0LfQtNCw0L3QuNGPLCDQvNGD0LfRi9C60LDQu9GM0L3Ri9GFINC40L3RgdGC0YDRg9C80LXQvdGC0L7QsiDQuCDQv9GA0L7Qs9GA0LDQvNC80L3QvtCz0L4g0L7QsdC10YHQv9C10YfQtdC90LjRjy4=
... and so on ... each new message starts at the new line (LF).
```

#### GET /u/point/pauth/tmsg

Parameters:

  * pauth: user authstring
  * tmsg: base64 encoded urlsafe message
  
Maximum message size is 87382 bytes (because base64-encoded message would be 64k in size).
  
##### Example

```
curl -XGET http://dynamic.lessmore.pw/idec/u/point/myauthstring/SGVsbG8sIFdvcmxkIQo=
```

#### POST /u/point

Parameters:

  * pauth: user authstring
  * tmsg: base64 encoded urlsafe message

Maximum message size is 87382 bytes (reason explained above).

##### Example

```
curl -XPOST http://dynamic.lessmore.pw/idec/u/point -d '{"pauth": "myauthstring", "tmsg": "SGVsbG8sIFdvcmxkIQo="}'
```

### Location /e/
#### GET /e/echo.1

Return list of msgids(string) from *echo.1*

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/e/python.15
l2dwnaJVhSHfu9btNMkz
WMJaWs1uKJZXkGTXL8Qp
```

### Location /m/
#### GET /m/msgid

Return raw message without base64 encoding.

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/m/0XRz7HAPfC6vc1PdYmHZ
ii/ok/repto/pprfzJ5NlQSHvmIm7oUO
python.15
1458562549
vit01
mira, 1
Andrew Lobanov
Re: Код, возвращаемый приложением


====
p=subprocess.Popen(блаблабла)
p.wait()
print(p.returncode)
====

Но вот зачем...
```

## Extensions

### GET /list.txt

Return list of public available echoes (the hidden ones are just hidden, but available as well). Format: *test.14(echoname string):35(messages count int):Echo description(description string)*

#### Example

```
curl -XGET http://idec.spline-online.ml/list.txt
bash.rss:4993:RSS с сайта bash.im
creepy.14:321:Страшные истории
develop.16:206:Обсуждение вопросов программирования
game.rogue.14:196:Играем в rogue-like игры
habra.16:4537:RSS с сайта habrahabr.ru
ifhub.club:56:RSS-лента сайта ifhub.club
ifiction.15:149:Интерактивная литература
ii.14:2949:Обсуждение вопросов, связанных с ii
ii.stat:95:Статистика станции
ii.test.14:513:Тестовые сообщения
linux.14:604:Обсуждение OS GNU/Linux
lit.14:200:Литература
lor-opennet.17:1143:RSS с сайтов linux.org.ru и opennet.ru
mlp.15:1311:Уголок дружбомагии
music.14:39:Обсуждение музыки и её создания
pipe.2032:2232:Болталка
piratemedia.rss.15:385:RSS сайта piratemedia.net
python.15:42:Общение python-истов
ru.humor.14:603:Юмор на русском языке
std.club:1247:Клуб INSTEAD
```

### GET /x/features

Return list of supported features

#### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/x/features
list.txt
u/e
u/m
x/c
```

### GET /x/c/echo.1/echo.2

Return list of *echoname(string):messages count(int)*

#### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/x/c/python.15/music.14
python.15:42
music.14:43
```

### POST /u/push

Server method for receive messages from another trusted server.

Parameters:

  * **nauth** - authstring
  * **upush** - messages bundle
  * **echoarea** - echo name

#### Answers

All types of servers must print ```message saved: ok``` for each successfull saved message and `error: no auth` due to failed authentication. Any other server answers can differ from implementation to implementation. Error codes given below are relevant to PHP-node.

#### Errors

  * **error: <ERROR NAME>** - wrong message format
  * **error: no auth** - wrong authstring

#### Example

```
curl -XPOST http://idec.spline-online.ml/u/push -d '{
  "nauth": "authstring",
  "upush": "WMJaWs1uKJZXkGTXL8Qp:SGVsbG8sIFdvcmxkIQo=",
  "echoarea": "test.15"
}'
```

### GET /blacklist.txt

Bad messages list. Return list of msgids. These messages are not accepted on the node. Server will reject fetching attempts and treat the messages like they don't exist at all.

#### Example

```
curl -XGET http://idec.spline-online.ml/blacklist.txt
KNMUXRTMJA6XWHMB22CD
6AKO6DEWYF7EHXWMI2BY
qcYj9ceDYjoxt2z6qhzN
k1zaEUu1Tg0g97osDeS7
j8s7ZMdTzmHzsRJna3xb
vuMNfQWe8xonMFPZtOxP
bcp6izdkdCj9AXUOP2aT
```

### GET /u/e/echo.1/echo.2/offset:limit

  * **offset** - int
  * **limit** - int

The offset can be negative. If parameter is wrong, returns whole echoes.

#### Example

```
curl -XGET http://idec.spline-online.ml/u/e/ii.test.14/test.15/1:2
ii.test.14
7pHGprJwLFezD1JSHsd2
tL5FvcJGqtd4wg2PB6pD
```

### Files

Sysop puts files to own node and it's file can be downloaded using API.

#### GET /x/filelist/pauth

Return list of files. Format is *filename(string):size in bytes(int):description(string)*.

Parameters:

  * **pauth** - authstring

If client has passed, the server appends hidden files to filelist (if available).

##### Example

```
curl -XGET http://idec.spline-online.ml/x/filelist/authstring
haiku_caesium.png:22368:Цезий под Haiku OS
hotdoged0.png:83425:Редактор HotdogEd
hotdoged1.png:69114:Редактор HotdogEd
hotdoged2.png:18922:Редактор HotdogEd
```

#### POST /x/filelist

Return list of files. Format is *filename(string):size in bytes(int):description(string)*.

Parameters:

  * **pauth** - authstring

If client has passed authorization node appends hidden files to filelist.

##### Example

```
curl -XGET http://idec.spline-online.ml/x/filelist -d '{"pauth": "authstring"}'
haiku_caesium.png:22368:Цезий под Haiku OS
hotdoged0.png:83425:Редактор HotdogEd
hotdoged1.png:69114:Редактор HotdogEd
hotdoged2.png:18922:Редактор HotdogEd
```

#### GET /x/file/filename

Retrieve file from server.

Parameters:

  * **filename** - name of the file to download

If filename is empty returns error: ```error: specify file name```.

##### Example

```
curl -XGET http://idec.spline-online.ml/x/file/hotdoged0.png >/dev/null
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 83425  100 83425    0     0  31967      0  0:00:02  0:00:02 --:--:-- 31963
```

#### POST /x/file

Retrieve file from server.

Parameters:

  * **filename** - name of the file to download
  * **pauth** - authstring

If filename is empty returns error: ```error: specify file name```.

##### Example

```
curl -XPOST http://idec.spline-online.ml/x/file/hotdoged0.png -d '{"pauth": "authstring"}' >/dev/null
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 83425  100 83425    0     0  31967      0  0:00:02  0:00:02 --:--:-- 31963
```
