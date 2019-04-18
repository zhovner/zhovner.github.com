---
layout: post
category: geek
tags: gpg
excerpt: Очистка PGP карты в случае потери PIN кода 
title: OpenPGP Card V2.0 Factory Reset
---

Если от OpenPGP был утерян PIN код, или требуется полностью стереть все ключи и настройки,
можно вернуть карту к заводскому состоянию. Вероятно, эта поцедура может помочь починить брикнутую карту.  

Я использую OpenPGP карту на устройстве [Yubikey Neo](https://www.yubico.com/products/yubikey-hardware/yubikey-neo/)  
И GPG для OS X из пакета [GPGtools.org](https://gpgtools.org)  

Описанное ниже справедливо только для OpenPGP Card V2.0

Процедура представляет из себя посылку APDU команд карте с помощью `gpg-connect-agent`.  
В маке он он находится в `/usr/local/MacGPG2/bin/gpg-connect-agent`  

Перед началом нужно убедиться, что карта отвечает на APDU.  


Запрос версии прошивки:  
{% highlight text %}
# /usr/local/MacGPG2/bin/gpg-connect-agent --hex "scd apdu 00 f1 00 00" /bye
D[0000]  01 00 08 90 00                                     .....           
OK
{% endhighlight %}

Возврат к заводскому состоянию:  
```
# /usr/local/MacGPG2/bin/gpg-connect-agent -r ./openpgpcard_reset_apdu.scd
```

Содержимое файла `openpgpcard_reset_apdu.scd`:  
<pre><code>/hex
scd reset
scd serialno undefined
scd apdu 00 A4 04 00 06 D2 76 00 01 24 01
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 e6 00 00
scd reset
scd serialno undefined
scd apdu 00 A4 04 00 06 D2 76 00 01 24 01
scd apdu 00 44 00 00
/echo Card has been reset to factory defaults 
/bye
</code></pre>

Карта после очистки:
<pre><code>$ gpg --card-status
Application ID ...: D3740021040118000002040210340000
Version ..........: 2.0
Manufacturer .....: Yubico
Serial number ....: 02961802
Name of cardholder: [not set]
Language prefs ...: [not set]
Sex ..............: unspecified
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: forced
Key attributes ...: 2048R 2048R 2048R
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 3 3
Signature counter : 0
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
</code></pre>
