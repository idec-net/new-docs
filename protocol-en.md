# IDEC protocol

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [IDEC protocol](#idec-protocol)
    - [Names convention](#names-convention)
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
        - [Location /x/](#location-x)
            - [GET /x/c/echo.1/echo.2](#get-xcecho1echo2)
                - [Example](#example)
            - [GET /x/features](#get-xfeatures)
                - [Example](#example)

<!-- markdown-toc end -->


## Names convention

  * echoarea name must contain a dot <.>: ```music.14```
  * msgID is a unique 20-symbol piece of base64-encoded sha256 hash. Special base64 symbols like `+` and `/` must be replaced by readable letters (like A and Z for example).

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

The bundle. The primary way to receive messages on the client and on the server. Format:

```
msgid:<base64-encoded message>
msgid:<base64-encoded message>
msgid:<base64-encoded message>
msgid:<base64-encoded message>
... and so on ...
```

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/u/m/k37ndQLS4e8P9GsZmOAz
k37ndQLS4e8P9GsZmOAz:aWkvb2sKbXVzaWMuMTQKMTQwNTQ4MTI3NApzcGxpbmUKc3RhdGlvbjEzLCAxCkFsbAptdXNpYy4xNAoKCtCt0YXQvtC60L7QvdGE0LXRgNC10L3RhtC40Y8g0L/QvtGB0LLRj9GJ0LXQvdCwINC+0LHRgdGD0LbQtNC10L3QuNGOINC80YPQt9GL0LrQuCwg0LXRkSDRgdC+0LfQtNCw0L3QuNGPLCDQvNGD0LfRi9C60LDQu9GM0L3Ri9GFINC40L3RgdGC0YDRg9C80LXQvdGC0L7QsiDQuCDQv9GA0L7Qs9GA0LDQvNC80L3QvtCz0L4g0L7QsdC10YHQv9C10YfQtdC90LjRjy4=
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

Return list of msgids from *echo.1*

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
### Location /x/
#### GET /x/c/echo.1/echo.2

Return list of *echoname:messages count*

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/x/c/python.15/music.14
python.15:42
music.14:43
```

#### GET /x/features

Return list of supported features

##### Example

```
curl -XGET https://dynamic.lessmore.pw/idec/x/features
list.txt
u/e
u/m
x/c
```
