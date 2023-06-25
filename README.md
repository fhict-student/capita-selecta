## Inhoudsopgave
- [Definities](https://github.com/fhict-student/capita-selecta/tree/main#definities)
- [Benodigde Hardware](https://github.com/fhict-student/capita-selecta/tree/main#benodigde-hardware)
- [Benodigde Software](https://github.com/fhict-student/capita-selecta/tree/main#benodigde-software)
- [Voorbereiding](https://github.com/fhict-student/capita-selecta/tree/main#voorbereiding)
  - [Netwerkadapter Drivers](https://github.com/fhict-student/capita-selecta/tree/main#netwerkadapter-drivers)
- [Installatie en verbinding](https://github.com/fhict-student/capita-selecta/tree/main#installatie-en-verbinding)
  - [Besturingssysteem](https://github.com/fhict-student/capita-selecta/tree/main#besturingssysteem)
  - [Connectie met de Raspberry Pi](https://github.com/fhict-student/capita-selecta/tree/main#connectie-met-de-raspberry-pi)
- [Configureer OpenWrt](https://github.com/fhict-student/capita-selecta/tree/main#configureer-openwrt)
  - [Back-up](https://github.com/fhict-student/capita-selecta/tree/main#back-up)
  - [Aanpassingen](https://github.com/fhict-student/capita-selecta/tree/main#aanpassingen)
  - [Reboot en re-connect](https://github.com/fhict-student/capita-selecta/tree/main#reboot-en-re-connect)
- [Verbind met eduroam](https://github.com/fhict-student/capita-selecta/tree/main#verbind-met-eduroam)
- [Configureer de netwerkadapter](https://github.com/fhict-student/capita-selecta/tree/main#configureer-de-netwerkadapter)

## Definities
In deze handleiding wordt verstaan onder: 

1. **Router:** Een Raspberry Pi die fungeert als router en toegangspunt voor zowel server- als clientdevices, voorzien van Ethernet en een access point. De Raspberry Pi draait OpenWrt als besturingssysteem.
2. **Netwerk:** Het netwerk dat wordt geleverd door de Router in deze handleiding. Verbinding met het Netwerk kan worden gemaakt met behulp van een ethernetkabel en via een access point. Het Netwerk gebruikt DHCP om automatisch IP-adressen toe te wijzen in het bereik van 10.0.0.100 tot 10.0.0.254. Er is ruimte voor statische DHCP-leases op de overige adressen.

## Benodigde Hardware
Voor een succesvolle installatie van de Router is bepaalde hardware vereist:
| Benodigdheid                                                                        | Informatie                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Raspberry Pi, inclusief passende:<br>- Stroomadapter<br>- (Micro) SD-kaart (> 8 MB) | Voor dit project kan ieder type Raspberry Pi gebruikt worden. Hoe meer de Router moet doen, hoe krachtiger deze moet zijn. Het wordt aanbevolen om een Raspberry Pi 4 Model B te gebruiken, omdat deze altijd geschikt is.<br><br>**Let op:** de verschillende type Raspberry Pi's hebben verschillende SD-kaart poorten. [Onderzoek](https://elinux.org/RPi_SD_cards) eerst welk SD-kaart poort de gekozen Raspberry Pi heeft en welke SD-kaarten werken.                                                                                                                                                              |
| Een SD-kaartlezer                                                                   | Dit kan een computer of laptop zijn die is uitgerust met een ingebouwde SD-kaartlezer, maar het kan ook een externe USB SD-kaartlezer zijn die aan een computer / laptop wordt verbonden.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Draadloze netwerkadapter                                                            | Er zijn diverse netwerkadapters beschikbaar die kunnen werken met de Raspberry Pi, maar niet alle adapters zijn compatibel. Het is daarom van belang om vooraf onderzoek te doen naar de compatibiliteit tussen de gekozen netwerkadapter en de Raspberry Pi. In deze handleiding wordt de [Netgear WNA1100 N150 Wireless USB Adapter](https://www.netgear.com/support/product/wna1100) gebruikt. Deze adapter is getest en wordt aanbevolen voor de opstelling zoals beschreven in deze handleiding.                                                                                                                   |
| Toetsenbord en (Micro) HDMI-kabel met monitor<br>_of_<br>Computer / laptop met ethernetpoort    | Voor het configureren van de Router kan er gekozen worden tussen de voorheen genoemde mogelijkheden. Het verdient aanbeveling om gebruik te maken van een computer / laptop met een ethernetpoort, aangezien dit apparaat ook gebruikt kan worden voor het gebruik van de SD-kaartlezer. Het gebruik van een toetsenbord en een (Micro) HDMI-kabel met monitor is hierdoor overbodig.<br><br>**Let op:** de verschillende type Raspberry Pi's hebben verschillende HDMI-poorten. [Onderzoek](https://www.factoryforward.com/difference-between-raspberry-pi-hdmi-port-types/) eerst welke HDMI-poort de gekozen Raspberry Pi heeft. |
| Ethernetkabel (optioneel)                                                           | Voor een bedrade verbinding met het Netwerk is een ethernetkabel nodig. Het gebruik hiervan wordt sterk aanbevolen. Voor het gebruik van een ethernetkabel in de Router moet de gekozen Raspberry Pi wel beschikken over een ethernetpoort.                                                                                                                                                                                                                                                                                                                                                                             |

## Benodigde Software
Voor een succesvolle installatie van de Router is bepaalde software vereist:

| Benodigdheid            | Opmerking                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OpenWrt firmware        | OpenWrt is een op Linux gebaseerd besturingssysteem dat speciaal is ontworpen voor routers en andere embedded apparaten. Het biedt geavanceerde netwerkfuncties en configuratiemogelijkheden en heeft ook uitgebreide beveiligings- en prestatie-optimalisatiefuncties, wat het een geschikte keuze maakt voor de huidige opstelling.<br><br>Lees onder Operating System onder [Installatie](#installatie) meer over de downloadprocedure.                                                                                                                        |
| Image flashing software | Er zijn diverse soorten software beschikbaar voor het flashen van een Operating System op een (Micro) SD-kaart, zoals Balena Etcher, Rufus en ImageUSB. Desalniettemin wordt het aanbevolen om gebruik te maken van [Raspberry Pi Imager](https://www.raspberrypi.com/software/), aangezien deze software uiterst gebruiksvriendelijk is en goed van pas komt bij de installatie van Raspberry Pi OS. Dit besturingssysteem is immers benodigd voor andere handleidingen in deze repository. In deze handleiding wordt gebruikgemaakt van de Raspberry Pi Imager. | 

## Voorbereiding
### Netwerkadapter Drivers
Onderzoek welke driver nodig is voor de gekozen netwerkadapter. 
- Zoek op [deze](http://en.techinfodepot.shoutwiki.com/w/index.php?title=Special%3ASearch&search=&fulltext=Search) website naar de gekozen netwerkadapter. Typ bijvoorbeeld "Netgear WNA1100" of "TP-LINK TL-WN725N". 
- Klik op "Search" en klik in de zoekresultaten de passende pagina aan.
- Zoek in de informatietabel naar "Linux driver". Voor bijvoorbeeld de Netgear netwerkadapter is hier "ath9k_htc" als driver te vinden. Andere adapters hebben mogelijk andere drivers nodig, zoals bijvoorbeeld "8812au" of "rtl8192cu".
- Open [deze](https://openwrt.org/packages/table/start) pagina. Klik in het zoekveld onder "Name" in de tabel.
- Zoek naar de bijbehorende driver. Typ bijvoorbeeld "ath9k" of "rtl8192cu" en druk vervolgens op <kbd>⏎ Enter</kbd>.
- Controleer welke package nodig is voor de gekozen netwerkadapter. Bijvoorbeeld:
	- De Netgear netwerkadapter heeft als driver "ath9k_htc" nodig. Uit het zoekresultaat blijkt dat de package "kmod-ath9k-htc" nodig is.
	- De TP-LINK netwerkadapter heeft als driver "rtl8192cu" nodig. Uit het zoekresultaat blijkt dat de package "kmod-rtl8192cu" nodig is.
- Schrijf of sla de naam van de package ergens op. Deze is nodig in een volgende stap.

## Installatie en verbinding
### Besturingssysteem
- Download de OpenWrt firmware voor de Raspberry Pi. Dit kan gemakkelijk gedaan worden via de [firmware-selector](https://firmware-selector.openwrt.org/) op de officiële website van OpenWrt.
	- Typ in de zoekbalk de naam van de gekozen Raspberry Pi. Bijvoorbeeld, "Raspberry Pi 4B". Kies wanneer mogelijk voor de 64bit versie.
	- Vooraf kunnen benodigde packages geïnstalleerd worden op de image. Doe dit door op "Customize installed packages and/or first boot script" te klikken.
	- In de lijst van "Installed Packages" staan de packets die vooraf op de firmware worden geïnstalleerd, gescheiden door een spatie. Verwijder uit deze lijst de tekst "wpad-basic-wolfssl". Voeg aan het einde "wpad", "hostapd-common" en "luci" toe. Voeg ook de naam van de Netwerkadapter driver package uit [Voorbereiding](#voorbereiding) toe, bijvoorbeeld "kmod-ath9k-htc" of "kmod-rtl8192cu". Het wordt aanbevolen om ook de teksteditor "nano" toe te voegen.
	- Klik op "Request Build" en wacht tot de build succesvol is afgerond.
	- Klik onderaan op "Factory (EXT4)" en download de firmware.

- Haal de (Micro) SD-kaart uit de Raspberry Pi en plaats deze in de SD-kaartlezer. Verbind indien nodig nog de SD-kaartlezer met de computer / laptop.

- Open op de computer / laptop de Raspberry Pi Imager. Druk op de hieronder beschreven knoppen en voer uit:
	- **Choose OS:** Scroll helemaal naar beneden en klik op "Use Custom". In het venster dat opent, navigeer naar de OpenWrt firmware, selecteer deze en klik op "Open".
	- **Storage:** Selecteer de juiste (Micro) SD-kaart. **Let op:** controleer of de juiste kaart is aangeklikt! Alle inhoud van de SD-kaart wordt verwijderd.
<br><br>De instellingen onder het tandwiel icoontje hebben geen effect op de installatie van OpenWrt. De selectievelden kunnen dus allemaal worden uitgeschakeld. Klik op "Write", lees de prompt goed en klik dan op "Yes". Het duurt slechts een paar seconden om de installatie te voltooien.

- Haal de (Micro) SD-kaart uit de SD-kaartlezer en plaats deze terug in de Raspberry Pi.

- Plug de stroomkabel in de Raspberry Pi. OpenWrt wordt nu automatisch geïnstalleerd op de Raspberry Pi; dit kan best een aantal minuten duren, dus geef de Raspberry Pi voor de zekerheid 5-7 minuten om de installatie te voltooien.

### Connectie met de Raspberry Pi
Maak hier de keuze tussen het gebruik van een toetsenbord en (Micro) HDMI-kabel met monitor (1) of een computer / laptop met ethernetpoort (2).

**1. Toetsenbord en (Micro) HDMI-kabel met monitor**
- Sluit het toetsenbord en de (Micro) HDMI-kabel aan op de Raspberry Pi. Plug het andere uiteinde van de HDMI-kabel in een werkende monitor.

**2. Computer / laptop met ethernetpoort**
- Plug het ene uiteinde van de ethernetkabel in de Raspberry Pi en het andere uiteinde in de computer / laptop. Verbreek tijdelijk eventuele actieve Wi-Fi-verbinding van de computer / laptop.

- Open een terminal op de computer / laptop. Hieronder is te zien hoe dit gedaan kan worden op verschillende Operating Systems.

| Operating System | Methode                                                                                                                                                                            |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows          | - Op het toetsenbord van de computer / laptop, druk op <kbd><span>&#x229E;</span> Win</kbd> + <kbd>S</kbd>.<br>- Zoek naar "Terminal", "CMD" of "PowerShell" en open één van deze. | 
| Linux            | - Op het toetsenbord van de computer / laptop, druk op <kbd>❖ Super</kbd>.<br>- Zoek naar "Terminal" en open deze.                                                                 |
| MacOS            | - Op het toetsenbord van de computer / laptop, druk op <kbd>⌘ Command</kbd> + <kbd>Space</kbd>.<br>- Zoek naar "Terminal" en open deze.                                            |

- In de terminal, typ `ssh root@192.168.1.1` en druk op <kbd>⏎ Enter</kbd>. Typ "yes" en druk nogmaals op <kbd>⏎ Enter</kbd>. Er is nu een SSH-verbinding met de Raspberry Pi.

## Configureer OpenWrt
### Back-up
Voor het configureren van OpenWrt op de Raspberry Pi, moeten een aantal bestanden aangepast worden.

Het is raadzaam om een back-up te maken van bestanden voordat er instellingen geconfigureerd worden. Dit kan worden gedaan door de volgende commando's uit te voeren:
```
cp /etc/config/firewall /etc/config/firewall.back
```

```
cp /etc/config/network /etc/config/network.back
```

```
cp /etc/config/wireless /etc/config/wireless.back
```

```
cp /etc/config/dhcp /etc/config/dhcp.back
```

Als er iets fout gaat, kan het originele bestand worden hersteld met het volgende commando:
```
cp /etc/config/<bestand>.back /etc/config/<bestand>
```

Nu kunnen de bestanden zonder zorgen aangepast worden. Om de bestanden aan te passen kan gebruik gemaakt worden van de teksteditor "vi", of van "nano" als deze geïnstalleerd is bij de installatie van OpenWrt.
Het gebruik van de teksteditor "nano" wordt sterk aanbevolen voor ieder die niet bekend is met het gebruik van de teksteditor "vi".

Volg de volgende instructies om de bestanden op te slaan:
| Editor | Instructies                                                                                                                       |
| ------ | --------------------------------------------------------------------------------------------------------------------------------- |
| vi     | Druk op <kbd>⎋ Escape</kbd>. Druk daarna op <kbd>⇧ Shift</kbd> + <kbd>;</kbd>. Typ daarna "wq" en druk dan op <kbd>⏎ Enter</kbd>. |
| nano   | Druk op <kbd>⌃ Ctrl</kbd> + <kbd>S</kbd>. Druk daarna op <kbd>Ctrl</kbd> + <kbd>X</kbd>.                                            | 

### Aanpassingen
Pas de onderstaande bestanden aan met behulp van "vi" of "nano". De syntax is als volgt:
```
vi /pad/naar/bestand
```
of
```
nano /pad/naar/bestand
```

**1. Firewall configuratie**
```
<vi/nano> /etc/config/firewall
```

**Verander:**
1. onder `config zone` met `option name    wan`, van
   ```
   option input    REJECT
   ```
   naar
   ```
   option input    ACCEPT
   ```
   Dit zorgt ervoor dat inkomend verkeer op de WAN-interface wordt toegelaten.

<details>
  <summary>Het volledige "firewall" bestand.</summary>
  
  Hieronder is het "firewall" bestand met default configuratie inclusief bovenstaande aanpassingen te vinden.
  
  ```
config defaults
        option syn_flood '1'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'REJECT'

config zone
        option name 'lan'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'
        list network 'lan'

config zone
        option name 'wan'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'REJECT'
        option masq '1'
        option mtu_fix '1'
        list network 'wan'
        list network 'wan6'
        list network 'wwan'

config forwarding
        option src 'lan'
        option dest 'wan'

config rule
        option name 'Allow-DHCP-Renew'
        option src 'wan'
        option proto 'udp'
        option dest_port '68'
        option target 'ACCEPT'
        option family 'ipv4'

config rule
        option name 'Allow-Ping'
        option src 'wan'
        option proto 'icmp'
        option icmp_type 'echo-request'
        option family 'ipv4'
        option target 'ACCEPT'

config rule
        option name 'Allow-IGMP'
        option src 'wan'
        option proto 'igmp'
        option family 'ipv4'
        option target 'ACCEPT'

config rule
        option name 'Allow-DHCPv6'
        option src 'wan'
        option proto 'udp'
        option dest_port '546'
        option family 'ipv6'
        option target 'ACCEPT'

config rule
        option name 'Allow-MLD'
        option src 'wan'
        option proto 'icmp'
        option src_ip 'fe80::/10'
        list icmp_type '130/0'
        list icmp_type '131/0'
        list icmp_type '132/0'
        list icmp_type '143/0'
        option family 'ipv6'
        option target 'ACCEPT'

config rule
        option name 'Allow-ICMPv6-Input'
        option src 'wan'
        option proto 'icmp'
        list icmp_type 'echo-request'
        list icmp_type 'echo-reply'
        list icmp_type 'destination-unreachable'
        list icmp_type 'packet-too-big'
        list icmp_type 'time-exceeded'
        list icmp_type 'bad-header'
        list icmp_type 'unknown-header-type'
        list icmp_type 'router-solicitation'
        list icmp_type 'neighbour-solicitation'
        list icmp_type 'router-advertisement'
        list icmp_type 'neighbour-advertisement'
        option limit '1000/sec'
        option family 'ipv6'
        option target 'ACCEPT'

config rule
        option name 'Allow-ICMPv6-Forward'
        option src 'wan'
        option dest '*'
        option proto 'icmp'
        list icmp_type 'echo-request'
        list icmp_type 'echo-reply'
        list icmp_type 'destination-unreachable'
        list icmp_type 'packet-too-big'
        list icmp_type 'time-exceeded'
        list icmp_type 'bad-header'
        list icmp_type 'unknown-header-type'
        option limit '1000/sec'
        option family 'ipv6'
        option target 'ACCEPT'

config rule
        option name 'Allow-IPSec-ESP'
        option src 'wan'
        option dest 'lan'
        option proto 'esp'
        option target 'ACCEPT'

config rule
        option name 'Allow-ISAKMP'
        option src 'wan'
        option dest 'lan'
        option dest_port '500'
        option proto 'udp'
        option target 'ACCEPT'
  ```
</details>

**2. Network configuratie**
```
<vi/nano> /etc/config/network
```

**Verander:**
1. onder `config interface 'lan'`, van
   ```
   option ipaddr '192.168.1.1'
   ```
   naar
   ```
   option ipaddr '<private_ip>'
   ```
   Dit zet het IP-adres van de LAN. In deze handleiding wordt gebruik gemaakt van het IP-adres "10.0.0.1".

**Voeg Toe:**
1. onder `config interface 'lan'`
   ```
   option force_link '1'
   ```
   Dit zorgt ervoor dat, zelfs wanneer de link niet direct actief is, het IP-adres toch aan de LAN-interface wordt toegewezen.

2. nieuwe interface 'wwan'
   ```
   config interface 'wwan'
	   option proto 'dhcp'
	   option peerdns '0'
	   option dns '8.8.8.8 1.1.1.1'
   ```
   Met `option proto 'dhcp'` wordt netwerkinterface 'wwan' geconfigureerd om automatisch IP-adressen te krijgen. Met  `option dns '8.8.8.8 1.1.1.1'` worden DNS-adressen geconfigureerd voor de netwerkinterface 'wwan'. Met `option peerdns '0'` wordt aangegeven dat de voorkeur uitgaat naar de DNS-adressen van  `option dns`.

<details>
  <summary>Het volledige "network" bestand.</summary>
  
  Hieronder is het "network" bestand met default configuratie inclusief bovenstaande aanpassingen te vinden.
  
  ```
  config interface 'loopback'
        option device 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fde2:7bec:228d::/48'

config device
        option name 'br-lan'
        option type 'bridge'
        list ports 'eth0'

config interface 'lan'
        option device 'br-lan'
        option proto 'static'
        option ipaddr '10.0.0.1'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option force_link '1'

config interface 'wwan'
        option proto 'dhcp'
        option peerdns '0'
        option dns '8.8.8.8 1.1.1.1'
  ```
</details>

**3. Wireless configuratie**
```
<vi/nano> /etc/config/wireless
```

**Verander:**
1. onder `config wifi-device 'radio0'`, van
   ```
   option channel '36'
   option band '5g'
   option htmode 'VHT80'
   option disabled '1'
   ```
   naar
   ```
   option channel '7'
   option band '2g'
   option htmode 'HT20'
   option disabled '0'
   ```
   Met `band` wordt de band veranderd naar 2.4 GHz. Met `htmode` wordt de high throughput mode veranderd.

**Voeg Toe:**
1. onder `config wifi-device 'radio0'`
   ```
   option short_gi_40 '0'
   ```
   Dit zorgt ervoor dat de Short Guard Interval van 40 MHz wordt uitgeschakeld.

<details>
  <summary>Het volledige "wireless" bestand.</summary>
  
  Hieronder is het "wireless" bestand met default configuratie inclusief bovenstaande aanpassingen te vinden.
  
  ```
  config wifi-device 'radio0'
        option type 'mac80211'
        option path 'platform/soc/fe300000.mmcnr/mmc_host/mmc1/mmc1:0001/mmc1:0001:1'
        option channel '7'
        option band '2g'
        option htmode 'HT20'
        option disabled '0'
        option short_gi_40 '0'

config wifi-iface 'default_radio0'
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'OpenWrt'
        option encryption 'none'
  ```
</details>

### Reboot en re-connect
Nu er aanpassingen zijn gemaakt aan het IP-adres en de firewall van de router, moeten de aanpassingen toegepast worden. Dit kan gemakkelijk gedaan worden door opnieuw op te starten. 

- Noteer het aangepaste IP-adres uit `/etc/config/network`; dit IP-adres is nodig voor de verbinding met de Router.

- Herstart de router met het volgende commando:
```
reboot
```

- De verbinding met de router wordt verbroken en dus moet er opnieuw verbinding gemaakt worden. In de terminal, typ:
```
ssh root@<ip_adres>
```

Verander in het bovenstaande commando het `<ip_adres` met het aangepaste IP-adres, bijvoorbeeld "10.0.0.1". Het commando wordt dan `ssh root@10.0.0.1`.

- Druk op <kbd>⏎ Enter</kbd>. Typ "yes" en druk nogmaals op <kbd>⏎ Enter</kbd>. Er is nu een SSH-verbinding met de Raspberry Pi Router.

## Verbind met eduroam
- Open op de computer / laptop een nieuw tab in een browser.

- Navigeer in de browser naar het IP-adres van de Router. In deze handleiding is het IP-adres "10.0.0.1" aangehouden. Navigeer dus naar "http://10.0.0.1".

- In de navigatiebalk op de OpenWrt-pagina, navigeer naar `Network > Wireless`.

- Onder "Wireless Overview", klik achter "radio0" op de "Scan" knop.

- Nadat de scan is voltooid, zoek in de lijst naar "eduroam". Klik hier op "Join Network".

- In het scherm dat tevoorschijn komt, zet een vinkje achter "Replace wireless configuration". Vul het wachtwoord van een eduroam account in bij "WPA passphrase". Klik vervolgens op "Submit".

- Onder "Device Configuration":
	- "Operating frequency":
		- Zet "Mode" naar "N".
		- Zet "Band" naar "2.4 GHz".
		- Zet "Channel" naar "7 (2442 MHz)".
		- Zet "Width" naar "20 MHz".

- Onder "Interface Configuration":
	- "General Setup":
		- Zet "Mode" naar "Client".
		- Zet "ESSID" naar "eduroam".
		- Zet "Network" naar de "wwan"-interface.

	- "Wireless Security":
		- Zet "Encryption" naar "WPA2-EAP (strong security)".
		- Zet "Cipher" naar "Force TKIP and CCMP (AES)".
		- Zet "EAP-Method" naar "PEAP".
		- Zet "Authentication" naar "EAP-MSCHAPv2".
		- Vul achter "Identity" de gebruikersnaam van een eduroam-account in.
		- Vul achter "Password" het wachtwoord van een eduroam-account in.
		- Klik op "Save".

- Klik op "Save & Apply". De Router zou nu verbinding moeten maken met eduroam.

## Configureer de netwerkadapter
- Plug de netwerkadapter in een beschikbare USB-poort op de Raspberry Pi.

- Refresh het OpenWrt-tablad in de browser op de computer / laptop.

- Klik in de rij waar "SSID: OpenWrt" staat op "Edit".

- Onder "Device Configuration":
	- "Operating frequency":
		- Zet "Channel" naar "1 (2412 MHz)".
		- Zet "Width" naar "20 MHz".

- Onder "Interface Configuration":
	- "General Setup":
		- Zet "Mode" naar "Access Point".
		- Verander "ESSID"; dit is de naam van de access point. In deze handleiding zal "OpenWrt" als SSID worden aangehouden.
		- Zet "Network" naar de "lan"-interface.

	- "Wireless Security":
		- Zet "Encryption" naar "WPA2-PSK".
		- Zet "Cipher" naar "auto".
		- Zet "Key" naar een wachtwoord naar keuze; dit is het wachtwoord waarmee verbinding gemaakt kan worden met de access point.

- Klik op "Save".
