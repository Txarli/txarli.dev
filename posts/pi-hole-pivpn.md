---
title: "Pi-Hole eta PiVPN: nabigazio segurua edonon"
slug: pi-hole-pivpn
description: >-
  Nola erabili Raspberry Pi bat nabigazioa babesteko etxean eta mugikorrean.
tags:
  - katxarreoa
added: "2025-11-5"
---

Etxean, beti daukagu [Raspberry Pi](https://www.raspberrypi.com/) bat sarera konektatuta. Honekin, hamaika erreminta jarri ditut martxan: produkzioan dauzkadan webguneen babes kopiak egiten ditut, prototipoak frogatu ditut, webguneak ostatu... Urrutira joan gabe, webgune hau Raspberry Pi batean ostatuta dago.

Aspaldi, [Pi-Hole](https://pi-hole.net/) proiektuari buruz hitz egin zidaten. Honen xedea, DNS zerbitzaritxo bat sortuta, sare osorako tracker eta iragarkiak blokeatzea da. Interesgarria iruditu zitzaidan eta, ikasleei, honi buruzko lanketatxo bat egiteko eskatu nien DNS sistema aztertzen ari ginela. Hortxe pikatu nintzen eta frogatzeko grina etorri zitzaidan.

Pi-Hole proiektuak nola funtzionatzen zuen ikusita, etxean bakarrik ez, eta mugikorrean ere trafikoa nik konfiguratutako Pi-Hole horretatik pasatzea erabaki nuen. Horretarako, [PiVPN](https://www.pivpn.io/) proiektuan oinarritu nintzen.

Sistema hau martxan daukat eta erabiltzen ari naiz. Gehiago jakiteko, segi irakurtzen.

## Pi-Hole

### Instalazioa

Pi-Hole instalazioa nahiko erraza da. Posible da Docker edukiontzi batean jartzea edo, nahiago baduzu, sistema eragilean zuzenean instalatzea. Nik, hasiera batean sistema eragilean instalatu nuen, baina azkar damutu nintzen Raspberry Pi hau gauza gehiagorako erabiltzen dudalako. Docker edukiontzi batean instalatuta, erabilera ezberdinen uztartze hori errazten dit.

Behin instalatuta, zure DHCP zerbitzarian DNS zerbitzari bezala Pi-Hole gailua konfiguratu beharko duzu. Hau egiteko, hoberena da Raspberry Pi-ak IP helbide estatikoa izatea. Guzti hau, Pi-Hole proiektuaren webguneak ongi azaltzen du, beraz, ez naiz asko luzatuko :).

### Konfigurazioa: blokeo zerrendak

Pi-Hole instalazioan, blokeo zerrenda bat lehenetsita dator. Zerrenda honek, 100.000 DNS baino gehiago blokeatuko ditu eta, nire kasuan behintzat, aski izan zen gehien erabiltzen ditudan webguneen iragarkiak blokeatzeko. Hale ere, pixka bat haratago joatea erabaki nuen eta [Githuben "hagezi" erabiltzaileak sortutako zerrenda hauek konfiguratu nituen](https://github.com/hagezi/dns-blocklists). Oso eguneratuta daude eta gaika sailkatzen dira. Nik, hurrengo zerrendak sartu nituen:

- [Multi PRO](https://github.com/hagezi/dns-blocklists#pro): oinarrizko DNS orokorrak.
- [Intelligence](https://github.com/hagezi/dns-blocklists#tif): segurtasuna hobetzeko.
- [Gambling](https://github.com/hagezi/dns-blocklists#gambling): online apostu etxeak ekiditeko.
- [NSFW](https://github.com/hagezi/dns-blocklists#nsfw): online pornografia ekiditeko.

Zerrenda hauekin, milaka DNS blokeatu ditut eta ez dut inoiz sentitu nire nabigazioa oztopatzen zenik.

## PiVPN

PiVPN instalatzeko, hoberena da [iturrira joatea](https://www.pivpn.io/). Erabiltzeko, hurrengoa beharko duzu:
- Raspberry Pi gailu bat.
- Zuk kudeatzen duzun domeinu izen bat.
- WAN aldeko IP fijoa, edo dyndns moduko bat. Nik, script bat sortu nuen hau lortzeko, baina hurrengo post batean azalduko dut.

Behin webgunean azaltzen den instalazio komandoa erabilita, aski duzu pausoak jarraitzearekin. Oso oso erraza da! Hau eginda, zure mugikorra, ordenagailu eramangarria edo beste edozein gailu VPN honetara konektatzen duzunean, zure etxeko sareko zerbitzuak erabiliko dituzu, zuk kudeatutako Pi-Hole hori barne.
