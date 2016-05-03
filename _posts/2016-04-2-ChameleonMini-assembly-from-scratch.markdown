---
layout: post
category: geek
---

ChameleonMini — standalone NFC Card Emulator. This guide explains all DIY steps: printing PCB, finding componetns, soldering and programming.
<a href="/img/ChameleonMini_full.jpg"><img alt="ChameleonMini NFC Card Emulator" src="/img/ChameleonMini.jpg" /></a>  

<font size="3" color="grey">Image is clickable (3577x1635 pixels)</font>  
  

When chameleon was released it was difficult to buy one. Only <a href="http://kasper-oswald.de/gb/chameleonmini/">Kasper-oswald.de</a> offers preordes but not telling the price.
You can buy assembled and tested device from <a href="https://store.ryscc.com/collections/proxmark-3/products/chameleonmini">ryscc.com</a> but $149 is probably too much.  
So I decide to build one from scratch.  

## Printing Board

As ChameleonMini is opensource hardware the board schematic is published [github.com/emsec/ChameleonMini/tree/master/EAGLE](https://github.com/emsec/ChameleonMini/tree/master/EAGLE).
RevE little shorter then RevE-2, so I choose second version. PCB has two layers.
Files .brd and .sch can be opened by free version of [EAGLE](http://www.cadsoft.de/download-eagle/eagle-freeware/) software. There is two version of board: RevE and RevE-2.  

<center>![ChameleonMini Schematic EAGLE](/img/ChameleonMini_schematic_eagle.png)</center>

Using this files you can print the circuit board. There is many companies offers printing circuit boards, like
[Dirty PCBs](http://dirtypcbs.com/), [Seeed Studio](https://www.seeedstudio.com/service/index.php?r=pcb) and many other. You can find local company in your country but in most cases Chinese friends can make it cheaper.
Good price is 15-30$ for 10 pieces. You will need board schematic in Gerber format for PCB assembly [ChameleonMini_gerber.zip](/files/ChameleonMini_gerber.zip)  

I made my in university laboratory on 1mm textolit and without silk mask.

![ChameleonMini PCB](/img/ChameleonMini_PCB.jpg)









