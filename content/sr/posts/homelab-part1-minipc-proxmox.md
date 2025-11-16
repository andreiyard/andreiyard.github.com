---
title: "Homelab: Deo 1 – Mini PC i Proxmox instalacija"
date: '2025-11-16T01:37:30+00:00'
summary: "Kupovina mini računara u Srbiji i instalacija Proxmox VE sa testiranjem Intel AMT/vPro"
tags: ["homelab", "proxmox", "virtualization", "serbia", "mini-pc", "intel-amt", "vpro"]
series: ["homelab-setup"]
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
disableHLJS: true
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
    image: "/posts/homelab-part1-minipc-proxmox/images/cover.jpg"
    alt: "Lenovo ThinkCentre M920q Mini PC"
    caption: "Moj \"novi\" mini računar za home lab"
    hidden: false
    hiddenInList: false
---

## Uvod

Dugo sam želeo da napravim svoju domaću laboratoriju, ali sam to stalno odlagao. Vremenom se nakupilo dovoljno razloga da konačno počnem:
- Nemogućnost pokretanja x86_64 aplikacija na mom Mac-u (ARM arhitektura) - trebalo mi je okruženje za testiranje i razvoj
- Želja da eksperimentišem sa self-hosted servisima (Paperless-NGX, Monica, n8n) i virtualizovanim mrežnim laboratorijama (GNS3, EVE-NG)
- Kontrola nad sopstvenim podacima i bekap-ima na sopstvenoj opremi umesto cloud pretplata
- DevOps praksa: postavljanje i upravljanje infrastrukturom (GitLab, K8s, monitoring)

**Ciljevi projekta:**
- Izgraditi infrastrukturu ličnih servisa sa pristupom preko VPN/interneta
- Steći praktično iskustvo u planiranju infrastrukture, automatizaciji i bezbednosti
- Uraditi to budžetski, dobijajući maksimum za potrošeni novac

Pošto je ovo hobi projekat, a ne produkciono okruženje, izabrao sam najjednostavniji start - **mini PC sa Proxmox-om**. Kompaktan, moćan i ekonomičan izbor za pokretanje više virtualnih mašina.

## Potraga za hardverom u Srbiji

Najpogodniji hardver za moje potrebe su mini računari dizajnirani za korporativne radne stanice. Kompaktni su, pouzdani, i često imaju dovoljno moćan procesor i dosta RAM memorije. A ako se kupuje na polovanom tržištu, na kome u Srbiji ima dosta takvih uređaja, ispada čak i budžetski.

Tražio sam na najpopularnijem sajtu za oglase u Srbiji - https://kupujemprodajem.com. Nisam razmatrao zvanične lance prodavnica elektronike, jer nude veoma mali izbor i samo najnovije modele mini računara. Zbog toga cena ispada neopravdano visoka. eBay i druge aukcije koje rade u Evropi nema smisla razmatrati zbog uvoznih carina (platio bih najmanje 20% dodatno).

Evo linija modela koje su me zanimale:
- Lenovo ThinkCentre Tiny
- Dell OptiPlex Micro
- HP Elitedesk/Prodesk Mini

Budžet za ovaj računar sam stavio na 250 evra. Najveći izbor odgovarajućih opcija bio je među Lenovo modelima. Evo šta sam našao:

| Model | CPU | RAM | Cena | Komentar |
|--------|-----|-----|------|-------------|
| M720q | i5-8500T | 8GB | €180 | Dobra cena, ali malo RAM-a za moje potrebe |
| **M920q** | **i7-8700T** | **16GB** | **€250** | **Izabrao sam ovaj** - najbolji odnos |
| M910q | i7-7700T | 16GB | €240 | Starija generacija CPU, malo slabiji |
| HP EliteDesk 800 G4 | i5-8500 | 16GB | €280 | Skuplje bez prednosti |

Na kraju sam uzeo **Lenovo ThinkCentre M920q** sa i7-8700T, 16GB RAM i 256GB NVMe SSD za 250 evra. Prodavac se ispostavilo da je servis koji prodaje desetine takvih polovnih računara. Tamo sam takođe kupio još jednu pločicu memorije od 16GB za 30 evra.

**Ukupno: 280 evra za hardver.**

---

## Tehničke specifikacije

Evo šta sam dobio za svoj novac:

{{< collapse summary="**Potpuni opis sistema** (spojler)" >}}

### Osnovne informacije
- **Model name:** Lenovo ThinkCentre M920q
- **Model number:** S35M00
- **Machine Type:** 10RR
- **Form factor:** Tiny (1L)

### Procesor
- **CPU:** Intel Core i7-8700T
- **Jezgra/Niti:** 6 jezgara / 12 niti
- **Osnovna frekvencija:** 2.4 GHz
- **Turbo frekvencija:** 4.0 GHz
- **TDP:** 35W
- **Generacija:** 8th Gen Intel (Coffee Lake)

### Memorija
- **RAM:** 2x16GB DDR4

### Skladište
- **Tip:** NVMe SSD
- **Kapacitet:** 256GB
- **Interfejs:** M.2 PCIe
- **Dodatni slot:** Sata 2.5" SSD

### Posebne funkcije
- **Intel AMT (vPro):** Mogućnost daljinskog upravljanja (upravljanje napajanjem, daljinski KVM pristup)

{{< /collapse >}}

### Fotografije

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/cover.jpg" alt="Lenovo M920q u radu" caption="Moj trenutni setup" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/m920q-front.jpg" alt="Prednji izgled" caption="Dugme za napajanje, priključci za slušalice i mikrofon, Thunderbolt 3, USB. Iza prednje ploče se takođe nalazi mali ugrađeni zvučnik" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/m920q-back.jpg" alt="Zadnji izgled" caption="Svi potrebni portovi: DisplayPort, USB, HDMI, Ethernet" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/m920q-internals-main.jpg" alt="Unutrašnjost" caption="Unutra: pristup SATA fioci za povezivanje dodatnog diska" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/m920q-internals-ram-nvme.jpg" alt="Odeljak sa NVMe i RAM memorijom" caption="Dva SODIMM slota, M.2 slot" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/m920q-size.jpg" alt="Poređenje veličine sa MX Master 3S mišem" caption="Poređenje veličine sa MX Master 3S mišem" >}}

### Potrošnja energije i buka

Za server koji radi 24/7, potrošnja energije i nivo buke su važni. i7-8700T ima TDP od samo 35W, što ga čini veoma ekonomičnim:

- **U mirovanju:** ~10-15W - pri minimalnom opterećenju sistem skoro ne troši energiju
- **Prosečno opterećenje:** ~20-25W
- **Maksimalno opterećenje:** do 35W - kada su sva jezgra potpuno opterećena

**Nivo buke:** Praktično nečujan. U idle režimu ventilator se jedva čuje (radi na minimalnim obrtajima). Pod opterećenjem se čuje tiho zujanje, ali ne smeta čak i ako je računar u stambenom prostoru. Mnogo tiši od običnog desktop računara ili gaming laptopa.

**Temperature:** U mirovanju CPU drži 40-50°C, pod opterećenjem 70-80°C. Sistem hlađenja radi odlično, nije primećeno throttling.

---

## Instalacija Proxmox VE

### Šta je Proxmox i zašto baš on?

Pošto želim da pokrećem virtualne mašine, potreban mi je hipervizor. Izabrao sam **Proxmox VE (Virtual Environment)**, otvorenu platformu za virtualizaciju baziranu na Debian Linux-u, koja omogućava upravljanje virtualnim mašinama i kontejnerima preko pogodnog veb interfejsa.

Za domaću laboratoriju, Proxmox je, po mom mišljenju, idealan izbor. Besplatan je i sa otvorenim izvornim kodom. Za razliku od VMware ESXi, koji je ili plaćen ili jako ograničen u besplatnoj verziji, Proxmox daje sve mogućnosti odmah. Plaćena pretplata je potrebna samo ako želite korporativnu podršku, ali za domaću upotrebu nije neophodna.

U Proxmox-u imate sve što vam treba: bekap-e, snapshot-e, praćenje resursa. Možete početi sa jednim serverom, a kasnije dodati još servera i povezati ih u klaster.

### VM vs LXC: U čemu je razlika i šta izabrati?

U Proxmox-u postoje dva načina da pokrenete nešto:

**Virtualne mašine (VM)** — Klasična opcija. To je u suštini potpuna emulacija računara. Izolacija ovde je maksimalna - svaki VM je poseban izolovani svet. Ali to se plaća: VM troši više RAM-a i procesora, duže se pokreće, i generalno ima dodatne troškove od virtualizacije hardvera.

**LXC kontejneri** rade potpuno drugačije. To je kontejner koji koristi jezgro operativnog sistema domaćina, odnosno Proxmoxa, ali pri tom izoluje procese, mrežu i fajl sistem. U suštini to je kao poseban Linux koji se pokreće za sekunde i troši megabajte memorije umesto gigabajta. Performanse su skoro kao kod nativnog sistema. Nedostatak je što možete pokrenuti samo Linux i izolacija od domaćina nije potpuna.

### Proces instalacije

Instalacija Proxmox-a je prilično jednostavna, ali postoji nekoliko važnih trenutaka. Potpuno zvanično uputstvo: https://www.proxmox.com/en/products/proxmox-virtual-environment/get-started

**Osnovni koraci:**
1. Preuzimamo ISO sliku sa zvaničnog sajta
2. Zapisujemo na USB fleš (Balena Etcher za macOS, Rufus za Windows)
3. Pokrećemo sa fleša (F12 za Boot meni na Lenovo)
4. Biramo grafičku instalaciju

**Važni momenti pri instalaciji:**
- **Disk:** Proxmox će se instalirati na ceo izabrani disk, brišući sve podatke
- **Mreža:** Bolje je odmah podesiti statičku IP adresu umesto DHCP - tako će biti lakše povezati se na veb interfejs
- **Hostname:** Zadajte razumljivo ime (na primer, `homelab-01.local`)
- **Root lozinka:** Koristite jaku lozinku - to će biti glavni pristup celom sistemu

Nakon instalacije, Proxmox je dostupan preko veb interfejsa na `https://<proxmox_ip>:8006/`. Login: `root`, lozinka ona koju ste zadali pri instalaciji.

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/proxmox.png" alt="Proxmox veb interfejs" caption="Proxmox veb interfejs nakon instalacije" >}}

---

## Testiranje Intel AMT / vPro

Sada najinteresantniji deo - testiranje Intel vPro. Pre kupovine ovog mini računara nisam čak ni čuo za ovu tehnologiju, ali kada sam video nalepnicu na kućištu sa natpisom Intel vPro odlučio sam da pretražim i na kraju sam se udubio u ovu prilično interesantnu temu.
Interesantno je što se nigde u specifikacijama za ovaj model ili druge slične modele ne pominje vPro i AMT. Nisam siguran da čak ni u kompanijama gde su ovi računari prvobitno radili neko iz IT zna za ovu funkcionalnost.

### Šta je Intel AMT?

Intel AMT (Active Management Technology) je tehnologija kompanije Intel, ugrađena u neke procesore i matične ploče za računare. Ona omogućava IT stručnjacima da daljinski upravljaju računarima: prate stanje, ažuriraju softver, uključuju/isključuju računar, čak i kada je računar isključen ili operativni sistem neće da se pokrene. Ovo se može uporediti sa IPMI na ozbiljnim serverima, ali IPMI obično ima svoj poseban mrežni interfejs.

### Omogućavanje AMT u BIOS-u

**Napomena**: Tačne putanje u meniju mogu se razlikovati zavisno od verzije BIOS-a. Ispod su opisani opšti koraci za Lenovo ThinkCentre M920q.

1. Ulazimo u BIOS (pritiskamo **F1** pri pokretanju)
2. Idemo u sekciju **Advanced** → **Intel(R) Manageability**
{{< figure src="/posts/homelab-part1-minipc-proxmox/images/bios-amt.jpg" alt="BIOS AMT podešavanja" caption="Omogućavanje Intel AMT u BIOS-u" >}}
3. Uključujemo **Intel Manageability Control**: biramo "Enabled"
{{< figure src="/posts/homelab-part1-minipc-proxmox/images/bios-amt2.jpg" alt="BIOS AMT opcije" caption="Dodatne AMT opcije u BIOS-u" >}}
4. Opciono: možete resetovati AMT konfiguraciju preko **Intel Manageability Reset** (ako AMT već ima lozinku koju ne znate)
5. Čuvamo promene (**F10**) i restartujemo
6. Nakon restartovanja, da biste ušli u **MEBx** (Management Engine BIOS Extension), treba pritiskati **Ctrl+P** tokom pokretanja

**Disclaimer**: Kombinacije tastera (F1, Ctrl+P) mogu se razlikovati na drugim modelima računara. Za Dell se često koristi F2, za HP - F10.

### Podešavanje AMT

Nakon što smo uspešno ušli u MEBx pri pokretanju računara, možemo podesiti AMT.
{{< figure src="/posts/homelab-part1-minipc-proxmox/images/MEBx-main.jpg" alt="MEBx glavni meni" caption="Glavni meni MEBx pri prvom ulasku" >}}

1. **Login**: Koristite podrazumevane kredencijale za prvi ulaz `admin`/`admin`
2. **Promena lozinke**: Sistem će odmah zatražiti da postavite novu lozinku
   - Minimum 8 karaktera
   - Mora sadržati: brojeve, mala i velika slova, specijalne karaktere
   - Na primer: `MyAmtPass123!`

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/MEBx-amt-config.jpg" alt="MEBx AMT konfiguracija" caption="Podešavanje parametara Intel AMT u MEBx" >}}

3. **Network Setup**: Možete podesiti IP adresu na kojoj će biti dostupan upravljački interfejs
   - **DHCP**: Automatsko dobijanje IP
   - **Static IP**: Ako vam treba statička adresa, navedite IP, masku, gateway
4. **Activate Network Access**: Aktivacija mrežnog pristupa AMT-u
5. **User Consent** (nivo saglasnosti korisnika):
   - **None** - daljinski pristup bez potvrde (pogodno za domaću upotrebu, kada računar stoji bez monitora i periferija)
   - **KVM only** - saglasnost potrebna samo za KVM sesije
   - **All** - potvrda potrebna za sve operacije (bezbednije za korporativno okruženje)
6. **Čuvanje**: Izlazimo iz MEBx sa čuvanjem (ESC → Yes)

**Važno! - Bezbednost Intel AMT:**

Intel AMT je moćan, ali potencijalno opasan alat. Radi **ispod nivoa operativnog sistema** i dostupan je čak i kada je računar isključen (ako su povezani napajanje i mreža).

**Šta morate uraditi obavezno:**
- Koristiti jaku lozinku
- **Nikada** ne otvarati AMT port (16992) direktno na internet

**Zašto je ovo važno:** Ako neko dobije pristup AMT-u, moći će potpuno da kontroliše računar: da vidi ekran, da upravlja sistemom, da montira ISO slike, čak i kada je OS isključen. To je kao fizički pristup računaru, ali preko mreže.

Za domaću mrežu iza NAT-a ovo je obično bezbedno, ali budite oprezni pri podešavanju daljinskog pristupa.

### Testiranje daljinskog pristupa

Nakon omogućavanja AMT-a pronašao sam njegovu DHCP IP adresu pomoću `nmap` skeniranja mreže.
Veb interfejs možete otvoriti tako što ćete u pretraživaču otići na `http://<amt_ip>:16992`.
Logujemo se sa korisnikom `admin` i lozinkom koju smo postavili i završavamo u ovom interfejsu:

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/amt-web-main.png" alt="AMT veb interfejs" caption="Intel AMT veb interfejs - glavna strana sa informacijama o sistemu" >}}

Ovde se vide neki opšti podaci o sistemu. Možete pogledati detaljnije informacije o hardveru (procesor, RAM moduli, disk).
Najvažnije što ovde postoji nalazi se u tabu `Remote Control`:

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/amt-web-power-management.png" alt="AMT upravljanje napajanjem" caption="Upravljanje napajanjem preko AMT veb interfejsa" >}}

Možemo da uključimo/isključimo/restartujemo. Ali... kako se povezati sa računarom daljinski sa ekranom, tastaturom i mišem?

U ovom interfejsu - nikako, za to je potreban poseban softver. Izbor za ovo nije baš veliki. Na macOS jedino što mogu da koristim je MeshCommander. (Dok sam tražio opcije, našao sam vodič za povezivanje sa Linux-a, ali nisam testirao da li radi - https://www.cyberciti.biz/faq/remotely-access-intel-amt-kvm-linux-desktop/)

### MeshCommander

**MeshCommander** - Ovo je softver sa otvorenim izvornim kodom za upravljanje računarima sa Intel AMT. To je cross-platform rešenje, napisano potpuno u JavaScript-u, koje radi na Windows, Linux i macOS sistemima. Na Windows-u je najlakše instalirati jer postoji jednostavan instalacioni program. Ali za druge operativne sisteme potrebno je pokrenuti lokalni veb server preko Node.js.

Koristio sam sledeći način na macOS:
1. Napravio sam direktorijum i prešao u njega
   ```bash
   mkdir meshcommander && cd meshcommander
   ```
2. Preuzeo sam meshcommander paket iz NPM
   ```bash
   npm install meshcommander
   ```
3. Pokrenuo sam ga pomoću node:
   ```bash
   node node_modules/meshcommander
   ```
4. Nakon toga možete otići na `http://127.0.0.1:3000` u pretraživaču

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/meshcommander.png" alt="MeshCommander" caption="MeshCommander interfejs" >}}

{{< figure src="/posts/homelab-part1-minipc-proxmox/images/meshcommander-kvm.png" alt="MeshCommander KVM sesija" caption="Primer povezivanja sa hostom za daljinsko upravljanje" >}}

### Mogućnosti

Pomoću MeshCommander-a možete uraditi mnogo više nego samo sa veb interfejsom AMT na samom uređaju:
- **KVM (Keyboard, Video, Mouse)** - daljinski pristup ekranu, tastaturi i mišu preko pretraživača
- **Serial-over-LAN** - pristup konzoli preko mreže
- **IDER (IDE Redirection)** - montiranje ISO slika daljinski

**Važno**: KVM radi samo sa Intel integriranom grafikom. Ako se koristi diskretna grafička kartica ili je monitor isključen bez HDMI dummy plug-a, slika možda neće biti dostupna.

U budućnosti planiram da nabavim takav HDMI dummy plug, ali za sada mi je dovoljno daljinsko uključivanje/isključivanje.

## Zaključak

Veoma sam srećan što sam, za razliku od mojih prethodnih pokušaja da se udubim u podešavanje svoje domaće laboratorije, ovog puta uspeo da prođem dalje od faze traženja servera.
Veoma sam iznenađen što sam zapravo našao savršenu opciju za sebe - malu, tihu i pristupačnu.
Moji osećaji nakon korišćenja Intel AMT bili su prilično mešoviti. S jedne strane, to je čudo što obični računar ima takve mogućnosti za daljinsko upravljanje, ali s druge strane potpuni nedostatak dokumentacije i čudni vlasniči protokoli koji zahtevaju poseban softver.

### Šta dalje?

U sledećem delu ću vam reći kako sam podešavao Proxmox nakon početne instalacije, kako sam postavljao virtualne mašine i LXC kontejnere sa servisima. Pokazaću kako postaviti i podesiti Cloudflare Tunnel za pristup servisima sa interneta bez potrebe za javnom IP adresom ili probacivanjem portova, kao i jednostavno i fleksibilno VPN rešenje.

---

## Linkovi

- [Awesome-selfhosted github repo](https://github.com/awesome-selfhosted/awesome-selfhosted)
- [Homelab subreddit](https://www.reddit.com/r/homelab/)
- [Selfhosted subreddit](https://www.reddit.com/r/selfhosted/)
- [Proxmox VE getting started](https://www.proxmox.com/en/products/proxmox-virtual-environment/get-started)
- [Intel AMT KVM kako se povezati sa Linux-a](https://www.cyberciti.biz/faq/remotely-access-intel-amt-kvm-linux-desktop/)
- [MeshCommander GitHub](https://github.com/Ylianst/MeshCommander)
- [MeshCommander zvanični sajt](https://www.meshcommander.com/)
