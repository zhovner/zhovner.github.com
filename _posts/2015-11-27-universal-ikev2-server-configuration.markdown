---
layout: post
category: geek
tags: 
excerpt: 
title: Universal IKEv2 Server Configuration
---

![IKEv2 vs Other VPN](/img/ikev2_vs_othervpn.png)

IKEv2 - modern protocol developed by Microsoft and Cisco wich used as default VPN type in OS X 10.11 (El Capitan) and  Windows since version 7.  
It supports strong encryption, auto reconnect when network changed (MOBIKE), easy connection configuration and more.

This manual describes minimal IKEv2 server configuration for most simple client setup based on username/password authentication.
No additional software needed on clients side, no certificates importing.


*IKEv2 supporting platforms*

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#ccc;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;border-top-width:1px;border-bottom-width:1px;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:0px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;border-top-width:1px;border-bottom-width:1px;}
.tg .tg-yw4l{text-align: left;vertical-align:center}
.tg .tg-yw4l-dh{text-align: right;vertical-align:center}
.spoiler-sum{font-size:20px; cursor:pointer;}
</style>
<table class="tg">
  <tr>
    <th class="tg-yw4l">Platform</th>
    <th class="tg-yw4l">Version</th>
    <th class="tg-yw4l">Ciphers</th>
    <th class="tg-yw4l">DH groups</th>
  </tr>
  <tr>
    <td class="tg-yw4l">Windows</td>
    <td class="tg-yw4l">≥ 7</td>
    <td class="tg-yw4l">3DES</td>
    <td class="tg-yw4l-dh">1024</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Windows Phone&#8194;&#8194;&#8194;</td>
    <td class="tg-yw4l">≥ 8.1</td>
    <td class="tg-yw4l">3DES</td>
    <td class="tg-yw4l-dh">1024</td>
  </tr>
  <tr>
    <td class="tg-yw4l">OS X</td>
    <td class="tg-yw4l">10.11 El Capitan&#8194;&#8194;&#8194;</td>
    <td class="tg-yw4l">DES<br>3DES (Default)<br>AES-128/256(GCM)&#8194;&#8194;&#8194;&#8194;</td>
    <td class="tg-yw4l-dh">768 - 8192<br>1024 (Default)</td>
  </tr>
  <tr>
    <td class="tg-yw4l">iOS</td>
    <td class="tg-yw4l">≥ 9<br>8 (no GUI)</td>
    <td class="tg-yw4l">same as OS X</td>
    <td class="tg-yw4l-dh"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Android</td>
    <td class="tg-yw4l"><a href="https://play.google.com/store/apps/details?id=org.strongswan.android">StrongSwan app</a></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l-dh"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Blackberry</td>
    <td class="tg-yw4l">8.1</td>
    <td class="tg-yw4l">DES<br>3DES<br>AES-128/256(GCM)</td>
    <td class="tg-yw4l-dh"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Linux / FreeBSD</td>
    <td class="tg-yw4l">strongSwan ≥ 5</td>
    <td class="tg-yw4l">any</td>
    <td class="tg-yw4l-dh">any</td>
  </tr>
</table>


<br>

## X.509 Certificates intro
In order to prevent man-in-the-middle attacks the IPsec/IKEv2 server always authenticates itself with an X.509 certificate using a strong RSA signature. 
After a secure communication channel has been set up clients authenticate themselves using the EAP-MSCHAPv2 protocol based on user name and  password (or another authentication protocol).  
This means that client needs to verify X.509 certificate authenticity using [CA](https://en.wikipedia.org/wiki/Certificate_authority) in system keychan.
The same way as for HTTPS connections in web browser.
Server certificate must be valid for successful client authentication.

There are two ways for getting server certificate: 

**1.** Use certificate issued by CA trusted by most operating system.  
**2.** Issue selfs-igned certificate and distribute your own CA to every client's system.  

First way make connection setup much easier on client side because it does not require importing any certificates in the system. This way is recommended.  
Self-signed certificates are more complicated. Follow this way  only if you know exactly what you need them for and how to manage your own PKI. 

### Issue certificate from one of Ceritifaces Authorities  
Fortunately X.509 certificates that we used to deploy as SSL certificates for HTTPS web servers are also suitable for IKEv2.  
You can get free certificate from [Wosign](https://buy.wosign.com/free/), [StartSSL](https://www.startssl.com/), or [LetsEncrypt](https://letsencrypt.org/), or any of your favorite CA.  
  
The only few requirements wich certificate must comply with:

* Have an Extended Key Usage (EKU) flag explicitly allowing the certificate to be used for authentication purposes.
  OID 1.3.6.1.5.5.7.3.1 (often called TLS server authentication)
  All certificated issued for web servers authenitcation have this flag.

* Subdomain  wich will be used as IKEv2 server adress must be in Subject Alternative Name.  
 Needed domain must be added as a additional domain, not as general one when issuing SSL certificate.
 I will use `tunnel.zhovner.com` as example.

* *(Optional)* If possible choose SHA-256 instead of SHA-1 signature algorithm, because SHA-1 is weak and deprecated.

![ipsec x509](/img/ipsec_x509.png)


###  Selfsigned certificates  
Selfsigned certificates requires to deploy complete [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure). Issue your own Root Certificate Authority (CA),
destribute this CA to all clients systems, issue server certificate, manage CLR (Certificate Revocation List) and OCSP.  
This subject is very complicated and goes out this manual, so I won't describe it here.  
In most cases you don't need selfsigned certificates. It is required only if you are planning to use client certificate authentication (without username/password).  
If you decided to use selfsigned certificates, take a look at [EasyRSA fork](https://github.com/ValdikSS/easy-rsa-ipsec) than allows to issue certificates suitable both for OpenVPN and IKEv2 and simplifies PKI management.

----------------

## Server configuration

[strongSwan](https://www.strongswan.org/) - powerfull and open source IPsec/IKEv2 server and client solution.  
You will need any Linux box with 2.6 or 3.x kernel to run strongSwan server.  
Check that your favorite distro have strongSwan ≥ 5.x package in repo.  
If you run Linux in virtual container, make sure that you have XEN or KVM virtualization but not OpenVZ, because OpenVZ not supporting kernel IPsec.

I recommend  <a href="https://www.linode.com/?r=f29232fc46afc280fceac66c08d46ebfec507fb9">Linode</a> as VPS hosting bacause they provide additional /64 IPv6 routable subnet that easely can be assigned to IPsec clients.

In this example I will use `Debian 8.2 jessie` as most common distro.  
Make sure that your stongSwan package not older than `5.2.1-6+deb8u2`

Installing strongSwan and extra plugins:

`apt-get install strongswan libcharon-extra-plugins`  

On this step you must have all necessary certificates and key files.

List of required files:  

**piratekey.pem** — RSA private key which was used for CSR when issuing certificate. Key must be non-encrypted.

**CA.crt** — Root Certificate of you Certificate Authority. It can be downloaded from web or exported from system keychain.  

**intermediate1.crt** — intermediate certificate of you Certificate Authority. If you get certificate from WoSign look at *for Other Server.zip* archive.  

**intermediate2.crt** — *(optional)* Number of intermediate certificates may be varied, depending on you CA. 

**my.crt** — your certificate. In the example this file will be named **tunnel.zhovner.com.crt**  

*How this chain looks from Google Chrome certificate information*
![cert chain](/img/x509_chain.png)

 Place each file in proper path:


<pre><code>/etc/ipsec.d/private/piratekey.pem
/etc/ipsec.d/cacerts/ca.crt
/etc/ipsec.d/cacerts/interm1.crt
/etc/ipsec.d/cacerts/interm2.crt
/etc/ipsec.d/certs/tunnel.zhovner.com.crt</code></pre>


Edit `/etc/ipsec.secrets` that contains users and private keys credentionals:


{% highlight bash %}
# This file holds shared secrets or RSA private keys for authentication.

# RSA private key for this host, authenticating it to any other host
# which knows the public part.

# this file is managed with debconf and will contain the automatically created private key
# this string can be removed if you not use debconf for strongswan like in our case
include /var/lib/strongswan/ipsec.secrets.inc 

# This is private key located at /etc/ipsec.d/private/piratekey.pem
: RSA privatekey.pem

# Create few VPN users for testing
obama : EAP "SuperPassword123"
putin : EAP "KremlinSecure666"
{% endhighlight %}

Edit `/etc/ipsec.conf`:

{% highlight bash %}
config setup

    #  Uncomment to allow few simultaneous connections with one user account.
    #  By default only one active connection per user allowed.
    # uniqueids=no

    # Increase debug level
    # charondebug = ike 3, cfg 3

conn %default

    # More advanced ciphers. Uncomment if you need it.
    # Default set of ciphers will works on most platforms.
    # ike=aes256gcm16-aes256gcm12-aes128gcm16-aes128gcm12-sha256-sha1-modp2048-modp4096-modp1024,aes256-aes128-sha256-sha1-modp2048-modp4096-modp1024,3des-sha1-modp1024!
    # esp=aes128gcm12-aes128gcm16-aes256gcm12-aes256gcm16-modp2048-modp4096-modp1024,aes128-aes256-sha1-sha256-modp2048-modp4096-modp1024,aes128-sha1-modp2048,aes128-sha1-modp1024,aes128-sha1,3des-sha1!

    # Dead peer detection will ping clients and terminate sessions after timeout
    dpdaction=clear
    dpddelay=35s
    dpdtimeout=2000s

    keyexchange=ikev2
    auto=add
    rekey=no
    reauth=no
    fragmentation=yes
    #compress=yes

    # left - local (server) side
    leftcert=tunnel.zhovner.com.crt # Filename of certificate located at /etc/ipsec.d/certs/
    leftsendcert=always
    # Routes pushed to clients. If you don't have ipv6 then remove ::/0
    leftsubnet=0.0.0.0/0,::/0

    # right - remote (client) side
    eap_identity=%identity
    # ipv4 and ipv6 subnets that assigns to clients. If you don't have ipv6 then remove it
    rightsourceip=10.1.1.0/24,2a00:1450:400c:c05::/112
    rightdns=8.8.8.8,2001:4860:4860::8888

# Windows and BlackBerry clients usually goes here
conn ikev2-mschapv2
    rightauth=eap-mschapv2

# Apple clients usually goes here
conn ikev2-mschapv2-apple
    rightauth=eap-mschapv2
    leftid=tunnel.zhovner.com

# If you need assign static IP to some clients for example for port forwarding 
# it can be matched by specific rightid
# conn static-ip-for-putin
#   also="ikev2-mschapv2-apple"
#   rightid=putin
#   rightsourceip=10.1.1.99,2a00:1450:400c:c05::1337
#   rightdns=8.8.8.8,2001:4860:4860::8888
{% endhighlight %}


Restart strongSwan to read new config files:  

`systemctl restart strongswan`

Verify that all cerifitaces configured correctly by executing `ipsec listall`  
Notice that output is very long and must be readed from the top.  
Full certificates chain must be presented and Entity Certificate must contain "has private key".

<pre><code>List of X.509 End Entity Certificates:

  altNames:  zhovner.com, www.zhovner.com, hub.zhovner.com, tunnel.zhovner.com, ....
  subject:  "CN=zhovner.com"
  issuer:   "C=CN, O=WoSign CA Limited, CN=CA CA 沃通免费SSL证书 G2"
  serial:    11:eb:85:14:5b:e2:56:96:0d:b4:fe:f2:4a:88:48:21
  validity:  not before Apr 22 17:19:27 2015, ok
             not after  Apr 22 18:19:27 2018, ok 
  pubkey:    RSA 2048 bits, <font color="red"><b>has private key</b></font>
  keyid:     0a:c6:c8:96:59:73:2c:6c:6f:7d:03:d0:35:da:e8:49:db:86:6e:fb
  subjkey:   f8:b0:bb:e9:9b:2c:8c:a2:90:b2:c0:77:b5:2b:c1:1f:d0:98:7e:d7
  authkey:   30:da:74:86:f3:28:90:56:9e:d7:31:31:c2:bd:59:cd:93:12:39:1d

List of X.509 CA Certificates:

  subject:  "C=CN, O=WoSign CA Limited, CN=CA 沃通免费SSL证书 G2"
  issuer:   "C=CN, O=WoSign CA Limited, CN=CA 沃通根证书"
  serial:    01:58:8c:3a:35:07:b3:f8:97:23:1c:76:b7:ef:85:dd
  validity:  not before Nov 08 03:58:58 2014, ok
             not after  Nov 08 03:58:58 2029, ok 
  pubkey:    RSA 2048 bits
  keyid:     7b:07:23:98:e4:9f:25:2f:19:3f:76:4d:cd:0f:70:f6:4b:fc:b0:e6
  subjkey:   30:da:74:86:f3:28:90:56:9e:d7:31:31:c2:bd:59:cd:93:12:39:1d
  authkey:   e0:4d:bf:dc:9b:41:5d:13:e8:64:f0:a7:e9:15:a4:e1:81:c1:ba:31
  pathlen:   0

  subject:  "C=CN, O=WoSign CA Limited, CN=CA 沃通根证书"
  issuer:   "C=IL, O=StartCom Ltd., OU=Secure Digital Certificate Signing, CN=StartCom Certification Authority"
  serial:    1f:ce:a7:f6:a9:7f:e9
  validity:  not before Sep 18 02:46:36 2006, ok
             not after  Jan 01 02:59:59 2020, ok 
  pubkey:    RSA 4096 bits
  keyid:     c2:06:fb:d5:3b:ba:0c:ee:f2:d2:d2:45:3d:07:52:26:3a:9f:e7:5f
  subjkey:   e0:4d:bf:dc:9b:41:5d:13:e8:64:f0:a7:e9:15:a4:e1:81:c1:ba:31
  authkey:   4e:0b:ef:1a:a4:40:5b:a5:17:69:87:30:ca:34:68:43:d0:41:ae:f2
  pathlen:   2

  subject:  "C=IL, O=StartCom Ltd., OU=Secure Digital Certificate Signing, CN=StartCom Certification Authority"
  issuer:   "C=IL, O=StartCom Ltd., OU=Secure Digital Certificate Signing, CN=StartCom Certification Authority"
  serial:    01
  validity:  not before Sep 17 23:46:36 2006, ok
             not after  Sep 17 22:46:36 2036, ok 
  pubkey:    RSA 4096 bits
  keyid:     23:4b:71:25:56:13:e1:30:dd:e3:42:69:c9:cc:30:d4:6f:08:41:e0
  subjkey:   4e:0b:ef:1a:a4:40:5b:a5:17:69:87:30:ca:34:68:43:d0:41:ae:f2</code></pre>


That's all. If everything looks right try connect to server.  
For debbuging connection problems run live logs stream `journalctl -f -u strongswan `  

#### NAT
For let out VPN clients into Internet you need configure NAT.  
This subject is not covered in this manual.  

For testing porpose  *(insecure)*:

`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`


