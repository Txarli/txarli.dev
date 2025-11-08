---
title: Proiektutxoak zure etxean ostatzeko bi modu
slug: dyndns-kaxeroa
description: >-
  Nola lortu, IP finkorik ez duzunean, domeinu batek zure etxeko zerbitzaritxo batera apuntatzea?.
tags:
  - katxarreoa
added: "2025-11-8"
---

Jende guztiak ez du zerbitzari dedikatu bat kontratatuta, baina edonork izan dezake katxarreo bat egiteko grina. Batzuetan gertatzen da proiektutxo bat montatu duzula (webgunetxo bat, web aplikazio bat, zerbitzu bat...) eta etxeko saretik internet bidez lagunei, familiari, kolektibo bati, etab. erakutsi nahi diezula. Igual `.com` domeinutxo bat erosi duzu eta guzti, total, ez dira horren garestiak. Momentu horretan, arazo bat izango duzu: zure internet hornitzaileak ez dizu IP finko bat ematen eta ezinezkoa egingo zaizu munduari erakustea.

Egoera hau gertatzen denean, hainbat aukera daude. Lehenengoa, internet hornitzaileak IP finkoa emateko aukerak aztertzea da. Askotan, ez dute aukera hau ematen edo kobratu egingo dizute. Ez baduzu horretan sartu nahi zure jostailutxoa erabiltzeko, horratx nik kasu hauetan erabili izan ditudan beste bi aukera.

## Ngrok: tunel bat zure aplikaziora

[ngrok](https://ngrok.com/) tresnak tunel seguruak sortzen ditu zure makina eta webaren artean. Esate baterako, Raspberry Pi batean web aplikazio bat jarri baduzu, aukera ematen dizu web aplikazio horretara apuntatuko duen url publiko bat lortzeko. Gaur gaurkoz, kontua irekita, ezer ordaindu gabe url bat sortzeko aukera ematen du (hori bai, ezin da zuk nahi duzuna izan, berak ausaz sortuko du `xxx-xxx.ngrok-free.ap` url bat).

Lehenengo hilabeteetan, Fosil.eu azpiegitura autogestionatura migratu aurretik, [laba.eus](https://laba.eus) webgunearen backend-a horrela zerbitzatzen genuen nire etxean zegoen Raspberry Pi batetik. Oso ongi funtzionatzen du gauza basikoetarako, ea ez den azkar [kakazten](https://eu.wikipedia.org/wiki/Kakazte_(Internet)). Muga nagusia, makina batean proiektu bakarra izan dezakezula da, ez baitu portu ezberdinekin jolasteko aukerarik ematen.

## Behin-behineko DynDNS sistema moduko bat

Sistema hau xinplea da: garapenerako API bat duen erregistradore bat baliatu, zure zerbitzaritik `cron` bidez IP publikoa eguneratzen joateko. Nik _script_ bat sortu nuen eta orduro egiten dut eguneraketa hori. Modu honetan, nire hornitzaileak esleitu didan IP helbidea aldatuko balu, orria erorita egongo den eperik luzeena ordu betekoa izango da.

![Konexioaren bloke diagrama. Ezkerrean zerbitzaria azaltzen da eta gezi batzuek https://api.apify.org URLra apuntatzen dute, "IP publikoa lortu" mezuarekin. Azpian, zerbitzaritik gezi batzuek IONOSen API-a koadrora apuntatzen dute, "Berritu bada, IONOS eguneratu" mezuarekin.](../assets/dyndns-kaxeroa/dyndns-kaxeroa.svg)

Erregistradoreari dagokionez, denetarik aurkitu dut: APIrik eskaintzen ez duten erregistradoreak, API ziztrin bat erabiltzeko extra bat kobratzen dutenak... Zin dagit ez dudala komisiorik jasoko honekin, baina nik [IONOS](https://www.ionos.es/) erabili dut domeinu izenak erregistratzeko. Arrazoia: [eskaintzen duten APIa](https://developer.hosting.ionos.es/docs/dns) doakoa dela eta ederki dokumentatuta dagoela.

Automatizatu aurretik, zure domeinuari `A` DNS erregistro bat sortu beharko diozu. Zure IP helbide publikoa sar dezakezu, edo `1.1.1.1` IP helbidea, total, aurrerago modu automatikoan jarriko diozu dagokion helbidea eta oso polita izango da funtzionatzen duela ikustea :).

### IONOS-en IP helbidea modu automatikoan eguneratzeko

Lehen esan bezala, _script_ bat sortu dut IONOS-ekin nire IP helbide publikoa eguneratzeko. Horretarako, lehen pausoa [zure _API key_-ak eskuratzea izango da](https://developer.hosting.ionos.es/). Honek bi kode emango dizkizu: publikoa eta sekretua (sekretua ongi gorde beharko duzu).

Behin _API key_ hauek lortuta, bigarren pausua eguneratuta mantendu nahi duzun erregistroaren identifikadoreak aurkitzea izango da. Horretarako, [IONOSek eskaintzen duen Swagger dokumentazioa](https://developer.hosting.ionos.es/docs/dns) baliatu dezakezu. Honen bidez, bi datu lortu beharko dituzu:

1. Zure domeinuaren zonalde identifikadorea: `/v1/zones` _endpointean_. Aurrerago ikusiko duzun kodean `ZONALDE_IDENTIFIKADOREA` balio honekin aldatu.
2. Zonalde horretan, zure erregistroak hartzen duen identifikadorea: `/v1/zones` _endpointean_. Aurrerago ikusiko duzun kodean, `ERREGISTRO_IDENTIFIKADOREA_ balio honekin aldatu.

Lortutako datu hauekin, hurrengo _script_ hau sortu beharko duzu zure ekipoan.

```bash
#!/bin/env bash

ip=$(curl -s https://api.ipify.org)
current_file="./current_ip"

if [ -f "$current_file" ]; then
  read -r old_ip < "$current_file"
else
  old_ip=""
fi

if [ "$old_ip" = "$ip" ]; then
  echo "${ip} IP helbidea konfiguratuta dago"
  exit 0
fi

printf '%s\n' "$ip" > "$current_file"

curl -X 'PUT' \
  'https://api.hosting.ionos.com/dns/v1/zones/ZONALDE-IDENTIFIKADOREA/records/ERREGISTRO_IDENTIFIKADOREA' \
  -H 'accept: application/json' \
  -H 'X-API-Key: KEY_PUBLIKOA.KEY_SEKRETUA' \
  -H 'Content-Type: application/json' \
  -d @- <<EOF
{
  "disabled": false,
  "content": "$ip",
  "ttl": 3600,
  "prio": 0
}
EOF
```

_Script_ honek, hurrengo ekintzak egiten ditu:

1. Zure IP helbide publikoa lortzen du.
2. Iada helbide hori kargatu duen aztertzen du, `current_ip` fitxategia erabiliz.
3. Fitxategian IP helbide hori ikusi badu, exekuzioa itxiko du. Bestela, IP helbidea eguneratuko du.

### Segurtasunaren inguruko hainbat ohar

Estrategia hau erabilitzeak, zure router-eko portuak irekitzera behartuko zaitu. Egiten ari zarena badakizu, ongi dago behin-behineko aukera bezala eta pixkatxo bat jolasteko, baina proiektua "serioagoa" bada edo epe luzerako martxan izan nahi baduzu, gomendagarriena ostatzeko zerbitzari dedikatu bat lortzea dela uste dut. Edonola ere, behin-behineko aukera honetan segurtasuna pixkatxo bat hobetzeko, Linux makina batean bazaude, `ufw` (firewall xinple bat) eta `fail2ban` (eraso posibleak blokeatzeko) erabiltzea gomendatuko nuke.
