---
title: "3D štampa: Deo 1 - Creality Hi i prvi printovi"
date: '2025-11-30T00:00:00+00:00'
summary: "Izbor prvog 3D printera, raspakivanje Creality Hi i prva nedelja štampanja"
tags: ["3d-printing", "creality", "orcaslicer", "klipper"]
series: ["3d-printing"]
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Izbor Creality Hi printera, prvi printovi i utisci"
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "/posts/print3d-part1-creality-hi/images/cover.jpg"
    alt: "Creality Hi 3D printer"
    caption: ""
    hidden: false
    hiddenInList: false
---

## Uvod

Video sam mini regale u kojima ljudi prave svoje kućne labove i veoma sam poželeo isto. Ispada da gotovo svi koriste fabričke desetinčne regale od DeskPi ( https://deskpi.com/ ). Posle traženja alternativa i načina da kupim nešto od tih regala, postalo je jasno da će me to koštati minimum 250 dolara za 10 unita (i to još bez polica). Zatim sam naišao na čoveka koji je potpuno dizajnirao i odštampao takav regal na 3D printeru — izgledalo je nerealno kul. Pred mnom je stao izbor: naručiti 3D štampu i potrošiti oko 100 evra na sve, ili kupiti 3D printer i sve uraditi sam, usput učeći nešto potpuno novo i potrošivši 4 puta više vremena i novca. Pravi izbor izgleda očigledno. :)

Evo projekta na koji sam naišao: [Lab Rax](https://makerworld.com/en/models/1464819-lab-rax-10-server-rack-bolted-version-5u) — potpuno 3D-štampani modularni 10-inčni serverski regal. Imajući u vidu da već imam mini PC sa Proxmox-om (vidi [moju seriju postova o kućnom labu](/sr/series/homelab-setup/)), ideja da odštampam regal za njega izgledala je kao logičan sledeći korak.

Ranije nisam imao iskustva sa 3D štampom. Ovo je prvi članak iz serije gde ću opisivati proces učenja — od kupovine printera do štampanja složenih projekata.

**Ciljevi:**
- Razumeti tehnologiju 3D štampe
- Odštampati regal za svoj kućni lab
- Štampati korisne stvari za kuću (držači, organizeri, nosači)
- Zamenjivati pokvarene delove umesto kupovine novih

## Izbor printera

### Kriterijumi

Lista zahteva:

1. **Buka** - želeo bih da mogu da postojim pored printera dok štampa i da ne umirem od krvi iz ušiju. Možda čak i da spavam dok printer radi u drugoj sobi
2. **Veličina** - nemam veliki sto gde bih mogao da stavim printer, potrebno je nešto što stane na komodu
3. **Budžet ~30,000 RSD** (~€250-300) — pošto tek počinjem ne treba mi najnapredniji printer sa milion funkcija
4. **Jednostavnost za početnike** — autokalibracija, minimum ručnog podešavanja
5. **Mogućnost prilagođavanja** — želeo bih da mogu uređivati konfige i koristiti potpuni veb interfejs (da mogu podešavati stvari po sebi kada se osposobim)

### Opcije

Posle traženja printera u zvaničnim srpskim prodavnicama i na sajtovima za oglase, sveo sam na četiri opcije:

| Model | Cena | Komentari |
|--------|------|-------------|
| **Creality Hi** | 29,600 RSD | Klipper, velika brzina, ali teže održavanje zbog dizajna glave za štampu |
| Elegoo Neptune 4 Plus | 33,000 RSD | Ogroman, složena procedura ažuriranja iz nekoliko koraka, poluručna kalibracija |
| Elegoo Centauri Carbon | 40,000 RSD | Veoma glasan |
| Anycubic Kobra 3 | 25,500 RSD | Jeftiniji, manje funkcija, poteškoće sa prilagođavanjem |

Najpopularniji printer za početnike je **Bambulab A1** (savršen kvalitet iz kutije). Nisam ga razmatrao zbog potpuno zatvorenog ekosistema i nemogućnosti menjanja firmvera ili uređivanja konfiga. Pa i košta ipak malo više.

### Zašto Creality Hi

Izabrao sam **Creality Hi**:

- **Nije zaključan od proizvođača** — Fluidd veb interfejs, SSH pristup bez sumnjivog koda za otključavanje, mogućnost izmene konfiga
- **Buka** — od svih razmatranih modela ovo je bio najtiši
- **Cena u okviru budžeta** — 29,600 RSD

## Porudžbina i sadržaj paketa

- Printer Creality Hi — 29,600 RSD
- PLA Soleyin Matt White (1 kg) — 1,680 RSD
- PLA Soleyin Matt Black (1 kg) — 1,680 RSD
- PETG Creality Hyper White (1 kg) — 2,600 RSD
- PETG Devil Design "boja-iznenađenje" (1 kg) — 1,680 RSD

Ukupno: **37,240 RSD** (~€320)

Dva kalema PLA za obično štampanje i prve eksperimente. Dva kalema PETG kupio sam sa namerom za budući Lab Rax projekat.

### Raspakivanje

Sadržaj paketa:

- Osnova printera
- Vertikalni okvir sa šinama i glavom za štampu
- Držač kalema filamenta
- Kabl za napajanje
- Set alata (imbus ključevi, kliješta)
- Mazivo
- Iglica za čišćenje ekstrudera
- Testni filament (mali kalem PLA)
- Uputstvo za montažu

{{< figure src="/posts/print3d-part1-creality-hi/images/things.jpg" alt="Sadržaj paketa" caption="Sve što je bilo osim samog printera" >}}

## Montaža i prvi start

### Montaža

Montaža je trajala manje od 30 minuta:

1. Stavio sam okvir sa šinama na osnovu i privrnuo zavrtnje
2. Povezao kablove
3. Postavio držač kalema sa filamentom

Nakon uključivanja printer je sam pokrenuo autokalibraciju.

{{< figure src="/posts/print3d-part1-creality-hi/images/printer-assembled.jpg" alt="Sklopljeni Creality Hi" caption="Printer sklopljen" >}}

### Interfejs

Kontrola preko ekrana osetljivog na dodir 3.2":

- Status štampanja
- Izbor fajla
- Podešavanja filamenta
- Parametri printera

{{< figure src="/posts/print3d-part1-creality-hi/images/display.jpg" alt="Ekran Creality Hi" caption="Ekran osetljiv na dodir" >}}

## Prvi printovi

Tokom prve nedelje odštampao sam nekoliko stvari — od testnih modela do korisnih predmeta za kuću. Sve printove radio sam sa PLA filamentom uz podrazumevana podešavanja slajsera.

### Block PLA — prvi print

Na printeru je bio učitan fajl `block_PLA_17m` (17 minuta štampanja). Ispada — ovo je blok za otpadni filament. Na početku štampe printer istiskuje plastiku da očisti mlaznicu, i ovaj blok sprečava da odleti u stranu.

{{< figure src="/posts/print3d-part1-creality-hi/images/block-pla.jpg" alt="Block PLA i kutijica za prikupljanje otpada" caption="Sistem za prikupljanje otpadnog filamenta u akciji" >}}

### Instalacija OrcaSlicer

Za pripremu modela potreban je slajser — program koji pretvara 3D model u G-code (instrukcije za printer).

Izabrao sam **OrcaSlicer** — besplatan, sa gotovim profilima za Creality Hi. Kasnije sam probao i zvanični **Creality Print**, kojim sam se češće služio zbog ugrađene podrške za kameru (više o tome u odeljku "Opcije kontrole printera").

Instalacija:
1. Instalirao sam pomoću `homebrew`, ali možete jednostavno preuzeti sa [GitHub](https://github.com/SoftFever/OrcaSlicer)
2. Izabrao Creality Hi printer sa liste
3. Profili podešavanja učitani automatski

### Benchy

Standardni test — mali brodić koji pokazuje mogućnosti printera (sitni detalji, prepusti, mostovi).

{{< figure src="/posts/print3d-part1-creality-hi/images/benchy.jpg" alt="Benchy" caption="Benchy sa podrazumevanim podešavanjima" >}}

Sa podrazumevanim podešavanjima ispalo je normalno. Sitni detalji se vide, prepusti su čisti.

### Markeri za pletenje

Žena me je zamolila da odštampam markere za pletenje kojih joj je ponestalo. Našao sam model na [MakerWorld](https://makerworld.com/en/models/555121-stitch-markers), odštampao 10 komada.

### Promena filamenta

Posle markera poželeo sam da odštampam kutijicu za otpadni filament (o kojoj ću pričati niže), i za nju je bio potreban crni filament. Proces promene:

1. Na ekranu: **Podešavanja filamenta** → **Retract**
2. Printer je automatski odzeo i istisnuo ostatak
3. Promenio sam kalem i provukao filament do glave za štampu
4. Pritisnuo: **Extract**
5. Printer je uvukao filament do mlaznice

Trajalo je par minuta. Valjda je sve jednostavno, ali prvi put uvek strahujete da nešto ne pokvarite.

### Dugmad — prvi problem

Dugmad za pleteni predmet — dva velika (4 cm) i tri mala (2 cm).

Površina je ispala grubo. Najverovatnije problem u brzini — sitni detalji, možda printer ne stigne da istisne plastiku (pod-ekstrudiranje). Eksperimente sa podešavanjima brzine ostavio sam za kasnije — dugmad ionako rade svoj posao.

## Korisni printovi za kuću

### Držač kablova sa štipaljkom za sto

Prvi model sa prepustima. Da bi se odštampao takav deo, potrebni su saporti (supports) — privremeni oslonci koji se kasnije uklanjaju.

U OrcaSlicer sam probao **Tree Supports** (saporti u obliku drveta) — rastu od stola granama ka delovima modela koji prepuštaju. Zauzimaju manje mesta i lakše se uklanjaju nego obični pravi saporti.

{{< figure src="/posts/print3d-part1-creality-hi/images/cable-holder-supports.jpg" alt="Držač kablova sa tree supports" caption="Držač za kablove odštampan sa tree supports" >}}

Posle štampe saporti su se odlomili bez problema, površina ispod njih ispala je blago hrapava ali prihvatljiva. Držač radi odlično — pričvrstio sam USB kabl na ivicu stola.

{{< figure src="/posts/print3d-part1-creality-hi/images/cable-holder.jpg" alt="Gotov držač kablova" caption="Gotov držač posle uklanjanja saporta" >}}

### Kutijica za otpadni filament

Pošto se block PLA pokazao korisnim, odlučio sam da odštampam kutijicu za njega gde će padati otpadni filament. Koristio sam crni PLA.

Odštampao sam uspešno, ali je OrcaSlicer automatski dodao suknju (skirt) printu — dodatni perimetar oko modela. Ispada da nije tako lako odvojiti od glavnog printa. Sledeći put ću proveravati podešavanja pre pokretanja. Siguran sam da bi ovaj jednostavni print i tako normalno bio odštampan.

Kutijica se savršeno uklopila u sistem — sada se otpadni filament skuplja uredno umesto da odleti na pod.

### Pokušaj režima vaze

U OrcaSlicer postoji režim vaze (Spiral Vase) — štampa objekat kao jednu neprekidnu spiralu bez gornjih i donjih slojeva. Izgleda bolje nego štampa po slojevima jer ne ostavlja šav na svakom sloju.

Probao sam odštampati malu vazu. Štampa je prošla uspešno, izgleda kul, ali vaza je ispala sa malim pukotinama. Negde spirala nije legla čvrsto. Za dekor odgovara, ali ne drži vodu.

{{< figure src="/posts/print3d-part1-creality-hi/images/vase.jpg" alt="Vaza" caption="Vaza" >}}

### Kutijica za stvari u hodniku

Prvi ozbiljniji funkcionalni print — tacna za sitnice u hodniku (ključevi, novčići, razne gluposti).

Za ovaj print odlučio sam da probam specijalne funkcije OrcaSlicer:

**Variable layer height (promenljiva visina sloja)** — automatski smanjuje visinu slojeva na zaobljenjima i ivicama za kvalitetniju površinu. Štampa je trajala znatno duže, ali detalji su ispali glatki.

**Ironing (peglanje)** — glava za štampu prolazi preko gornjih slojeva sa isključenom ekstrudacijom, zaglađujući površinu. Dodalo je još više vremena, ali površina dna ispala je veoma ravna i glatka.

{{< figure src="/posts/print3d-part1-creality-hi/images/catchall-tray.jpg" alt="Catchall tacna za hodnik" caption="Tacna sa variable layer height i ironing" >}}

Rezultat je vredeo utrošenog vremena — predmet izgleda kvalitetno. Bilo je sitnih defekata (tanke niti između rebara) koje sam lako uklonio kliještima i komadićem šmirgle.

## Opcije kontrole printera

### Ekran
Osnovna kontrola: pokretanje štampe iz memorije printera ili fleš diska, promena filamenta, ručno upravljanje, podešavanja.

### Fluidd (veb interfejs)
Printer radi na Klipper sa Fluidd veb interfejsom. Ulazite na IP printera sa portom 4408 u pregledaču (npr. `http://192.168.1.100:4408`):
- Kontrola štampe
- Praćenje temperatura
- Pristup Klipper konfiguraciji
- Konzola za komande

{{< figure src="/posts/print3d-part1-creality-hi/images/fluidd.png" alt="Fluidd veb interfejs" caption="Fluidd" >}}

Ugrađena kamera u printer bi trebalo da se takođe prikazuje u Fluidd interfejsu, ali ne. Možete je naći na odvojenom portu 8000:

{{< figure src="/posts/print3d-part1-creality-hi/images/camera.jpeg" alt="Kamera printera" caption="Interfejs kamere na portu 8000" >}}

### OrcaSlicer
Pretvaranje 3D modela u instrukcije za štampu i slanje na printer preko mreže.

{{< figure src="/posts/print3d-part1-creality-hi/images/orcaslicer.png" alt="OrcaSlicer" caption="OrcaSlicer, presek modela Benchy" >}}

### Creality Print
Zvanični slajser od Creality. Koliko razumem zasnovan je na već pomenutom open-source OrcaSlicer, ali sa ograničenim funkcijama. Izgleda veoma slično, ali je interfejs neugodan za oči, teško se čita tekst na poluprovidnoj pozadini.

Uprkos svim ovim problemima, najčešće sam koristio upravo Creality Print. Glavni razlog — u njemu "iz kutije" radi ugrađena kamera, što omogućava lako praćenje štampe na daljinu. U OrcaSlicer nema takve integracije nažalost, a radi pogodnog praćenja i mogućnosti isključivanja duge kalibracije pre štampe bio sam spreman trpeti nedostatke interfejsa.

{{< figure src="/posts/print3d-part1-creality-hi/images/creality_print.png" alt="Creality Print" caption="Creality Print" >}}

### SSH
Potpuni pristup sistemu. Još ne znam čemu bi to moglo koristiti, ali po meni je dobro što proizvođač dozvoljava naprednim korisnicima proširene mogućnosti kontrole nad svojim uređajem.

{{< figure src="/posts/print3d-part1-creality-hi/images/ssh.png" alt="SSH konekcija" caption="Povezivanje na printer preko SSH" >}}

### USB
Klasičan način — fajl na fleš disk, ubacio u printer, pokrenuo štampu. Nisam ga probao jer je prenos preko Wi-Fi mnogo pogodniji, ali mislim da stari dobri način treba da radi bez problema.

## Utisci

### Šta mi se dopalo

- Jednostavno je početi koristiti. Sklopio, uključio, štampam.
- Kvalitet štampe sa podrazumevanim podešavanjima je pristojan. Očekivao sam da će slojevi biti vidljiviji, ali zapravo ako se ne pogleda pažljivo gotov predmet, nije odmah očigledno da je odštampan na 3D printeru.
- Printer nije zaključan u ekosistemu proizvođača. Ako bude potrebno da promenim neki parametar ili koristim nestandardni slajser, moći ću to učiniti.

### Šta mi se nije dopalo

- Pri korišćenju OrcaSlicer printer pokreće potpunu kalibraciju stola pre svake štampe. U principu dovoljno je kalibrisati jednom posle uključivanja. Još nisam razumeo da li je ovo bag i kako to popraviti, ali tih prvih 10 minuta kalibracije je najmučnije što je bilo u mom životu.
- Kamera je prilično loša i nikakvi detalji se ne razaznaju, niska rezolucija i mali dinamički opseg. LED osvetljenje za kameru pravi uzak snop svetla i u potpunom mraku to uopšte ne odgovara. Mada treba imati u vidu da samo postojanje bilo kakve kamere je već plus.

## Šta dalje

Dok sam štampao ove jednostavne stvari od PLA pojavio se prvi pravi zadatak — napraviti ručku za ormar umesto pokvarene. Ovo je odličan povod da probam projektovati i odštampati svoj model. Trenutno sam u procesu projektovanja i čim završim, pričaću o tome u sledećem članku.

A dalje ću početi eksperimente sa **PETG** — čvršćim materijalom za štampanje baš tog Lab Rax serverskog regala. PETG je zahtevniji materijal od PLA i možda će trebati rešiti neke probleme vezane za to.

---

## Linkovi

- [Creality Hi — opis i specifikacije](https://www.creality.com/products/creality-hi-combo)
- [Lab Rax — 3D-štampani serverski regal](https://makerworld.com/en/models/1464819-lab-rax-10-server-rack-bolted-version-5u)
- [OrcaSlicer na GitHub](https://github.com/SoftFever/OrcaSlicer)
- [Markeri za pletenje na MakerWorld](https://makerworld.com/en/models/555121-stitch-markers)
- [Držač za kablove na MakerWorld](https://makerworld.com/en/models/878180-table-side-data-cable-clamp#profileId-844230)
- [Kutijica za hodnik na MakerWorld](https://makerworld.com/en/models/1074841-tray-bowl-catch-all-tray?from=search#profileId-1065398)
