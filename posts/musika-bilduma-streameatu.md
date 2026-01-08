---
title: Gure musika bilduma gure sarean streameatzeko proposamen bat
slug: musika-bilduma-streameatu
description: >-
  Sareetan azaltzen den #despotify mugimendua jarraitzeko modu bat, Navidrome eta Metadata Remote erabilita.
tags:
  - katxarreoa
added: "2026-1-8"
---

Etxean, denbora daramagu musika [Qobuz](https://www.qobuz.com/es-es/discover) plataformaren bidez entzuten. Kalitate ona eskaintzen du, artistak ongi tratatzen dituela dirudi, ez du adimen artifizial generatiboak sortutako musikarik sartzen eta, batez ere, [jabeek ez dute genozidioan inbertitzen](https://www.berria.eus/kultura/euskal-herriko-162-musika-taldek-utzi-dute-spotify-musikariak-palestinarekin-egitasmoarekin-bat-eginda_2150132_102.html). Edonola ere, [#despotify](https://redesnuestras.net/2024/12/31/una-propuesta-para-convertir-en-proposito-de-ano-nuevo-despotify/) ideiarekin kutsatuta, aurrekoan etxean alternatiba propio bat montatzeko aukerak aztertzen egon nintzen. Ikasteko asmoz edo, Qobuz [kakazten denean](https://teknopata.eus/2024/02/kakaztea-enshittification-1-zer-da), prest egoteko.

Hau egiteko, Raspberry Pi gailu bat erabili dut eta, zerbitzari aldean erabilitako software guztiak, Docker edukiontzietan sartu ditut. Hurrengo lerroetan nik frogatutako *stack*-ean sakonduko dut, baina, laburbilduz, hauek dira erabilitako aplikazioak (guztiak *Open Source* proiektuak dira):

- [Navidrone](https://www.navidrome.org/): Raspberry Pi-an dagoen musika streaming bidez zerbitzatzeko.
- [Metadata Remote](https://github.com/wow-signal-dev/metadata-remote): Musika bilduma ordenatzeko eta disken karatulak sartzeko.
- [Chora](https://github.com/CraftWorksMC/Chora): Musika bilduma mugikorretik gozatzeko.

## Navidrome: musika zerbitzaria

Navidromeren funtzio nagusia, zerbitzari batean daukagun musika bilduma web nabigatzaile baten bidez entzutea da. Honetaz gain, [Subsonic API](https://www.subsonic.org/pages/api.jsp)-a baliatuta, gure musika bilduma edozein gailutan entzuteko aukera ematen du.

Instalatzeko, lehen esan bezala, [Docker edukiontziaren](https://www.navidrome.org/docs/installation/docker/) estrategia jarraitu dut. Hau horrela egin dut, nire Raspberry Pi honetan hainbat zerbitzu dauzkadalako eta, modu honetan, guztia enkapsulatuta mantendu dezakedalako. Docker bidezko instalazioa, sistema hau pixkatxo bat manejatzen baduzu, oso erraza da. Kontutan izan beharreko gauza nagusia, zure musika liburutegia non dagoen apuntatzea da. Beste guztia, Docker irudiak egingo du.

## Metadata Remote: bildumaren kudeaketa

Tontakeri bat badirudi ere, hau izan da buruhauste gehien ekarri didana. Navidromek, [fitxategien metadatuak erabiltzen ditu kanta eta diska guztien informazioa erakusteko](https://www.navidrome.org/docs/usage/library/). Hau da, Navidromek ez du zure bilduma ordenatuta mantentzeko aukerarik ematen. Honek, fitxategien metadatuak kudeatzeko sistema bat bilatzera behartzen gaitu.

Helburu hau betetzeko hainbat aplikazio interesgarri daude, baina, nik aurkitutako guztiek behintzat, lokalean dauden fitxategiekin besterik ez dute funtzionatzen. Sareetan ikusitako hainbat aukera frogatu ondoren, [Metadata Remote](https://github.com/wow-signal-dev/metadata-remote) topatu nuen. Aplikazio honek, Github errepositorioaren README-ak ongi azaltzen duen bezala, zerbitzari bateko fitxategien metadatuak web nabigatzaile baten bidez kudeatzeko aukera ematen du.

## Gure musika bilduma edonora eramateko

Gaur egun, gure musika etxean eskuragarri izatea, ez da nahikoa. Lanera biderako autobusean, kotxean, kalean, etab. mugikorrean entzun ahal izatea ia ezinbestekoa da pertsona askorentzat. Horretarako, bi gauza lortu beharko ditugu: mugikorretik etxeko sarera konektatzea eta mugikorrean gure liburutegia entzuteko aplikazio bat izatea.

### Sarerako konexioa

Beste sarrera batean azaltzen nuen bezala, [nire Raspberry Pi zerbitzarian VPN bat konfiguratu nuen, PiVPN baliatuta](/post/pi-hole-pivpn). Honi esker, nire mugikorrean VPN hau aktibatzen dudanean, nire sareko zerbitzu guztiak erabili ditzaket, etxean banego bezala.

### Mugikorrerako aplikazioa

Posible da mugikorreko web nabigatzailetik, Navidrome zerbitzarira jotzen dugunean, gure musika mugikorretik entzutea. Hala ere, erosoagoa da beti musika entzuteko aplikazio natibo bat erabiltzea, kontrolak eskura izateko, edota mugikorra blokeatzerakoan entzuten jarraitzeko. 

Navidromek, bere webgunean, [bezero zerrenda bat eskaintzen du](https://www.navidrome.org/apps/). Hauetatik, Android mugikorrentzako ospetsuena [Symfonium](https://symfonium.app/) da. Onartu beharra dago, nire ustez behintzat, Symfonium dela aplikazio garbiena eta aukera gehien ematen duena, baina, bere "arazo" nagusia, ordaintzera behartzen duela da. Oker ez banago, 5-6 € ordaintzearekin bizitza osorako lizentzia eskuratu dezakegu, beraz, ez da oso garestia. Nire musika entzuteko modu bakarra sarrera honetan azaldutakoa balitz, dudarik gabe ordainduko nuke.

Edonola ere, akaso gauza kulturala da, baina asko kostatzen zait aplikazioengatik ordaintzea. Horregatik, alternatiba batzuk aztertu nituen. Doako Open Source aplikazio interesgarri asko aurki daitezke, baina, niri gehien konbentzitu didana, [Chora](https://github.com/CraftWorksMC/Chora) da. Xinplea da, interfaze garbi batekin eta, batez ere, Play Store-an dago. Honek, asko errazten du nire bilduma gertuko edonork entzutea.
