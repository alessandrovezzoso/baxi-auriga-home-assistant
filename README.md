# Indice
- [Indice](#indice)
  - [Baxi Auriga heat pump](#baxi-auriga-heat-pump)
    - [Descrizione dei termini](#descrizione-dei-termini)
- [Gateway USR-TCP232-410S-H7](#gateway-usr-tcp232-410s-h7)
    - [Consiglio d'acquisto](#consiglio-dacquisto)
  - [Collegamento fisico](#collegamento-fisico)
  - [Accesso alla web UI](#accesso-alla-web-ui)
  - [1. Configurazione di rete (Network → IP Config)](#1-configurazione-di-rete-network--ip-config)
    - [La questione dei DNS](#la-questione-dei-dns)
  - [2. Parametri seriali (Port → RS485 → Port)](#2-parametri-seriali-port--rs485--port)
  - [3. Modalità Modbus TCP (Port → RS485 → Socket)](#3-modalità-modbus-tcp-port--rs485--socket)
  - [4. Impostazioni di sistema (System → System Setting)](#4-impostazioni-di-sistema-system--system-setting)
  - [5. Isolamento dal cloud (Cloud Service + regola router)](#5-isolamento-dal-cloud-cloud-service--regola-router)
    - [Livello 1 — Disattivare le funzioni cloud nella web UI](#livello-1--disattivare-le-funzioni-cloud-nella-web-ui)
    - [Livello 2 — Regola sul router (blocco WAN)](#livello-2--regola-sul-router-blocco-wan)
    - [Test di verifica](#test-di-verifica)
  - [6. Configurazione lato Home Assistant](#6-configurazione-lato-home-assistant)
  - [Note sul bus condiviso e più dispositivi](#note-sul-bus-condiviso-e-più-dispositivi)
---

## Baxi Auriga heat pump 

### Descrizione dei termini
| Sigla               | Elemento                         | Significato pratico (Per l'utente)                                                                                                                               |
| :---                | :---                             | :---                                                                                                                                                             |
| **T4**              | Temperatura esterna              | Temperatura dell'aria esterna                                                                                                                                    |
| **T5**              | Temperatura Acqua Calda Sanitaria| Temperatura dell'acqua presente nel boiler                                                                                                                       |
| **T1S**             | Temperatura Obiettivo (Setpoint) | Temperatura a cui la macchina sta cercando di portare l'acqua dell'impianto                                                                                      |
| **TW_out** / **T1** | Temperatura acqua in uscita      | Temperatura reale dell'acqua che esce dalla pompa di calore e viaggia verso i termosifoni o il pavimento radiante                                                |
| **TW_in**           | Temperatura acqua di ritorno     | Acqua che "torna indietro" dalla casa alla pompa di calore, dopo aver rilasciato il suo caldo (o freddo) nelle stanze                                            |
| **T1B**             | Temperatura acqua Zona 2         | Se si ha l'impianto diviso in due, questa è la temperatura dell'acqua mandata alla seconda zona                                                                  |
| **AHS**             | Caldaia di supporto              | Se si ha un impianto ibrido, indica la caldaia tradizionale (es. a gas). Si accende solo per dare una mano quando fuori fa un freddo estremo                     |
| **IBH1 / IBH2**     | Resistenze elettriche (Casa)     | Riscaldatori elettrici di emergenza. Si accendono in automatico per aiutare a scaldare la casa se la pompa di calore non ce la fa da sola                        |
| **TBH**             | Resistenza elettrica (Doccia)    | Riscaldatore elettrico immerso nel boiler. Si usa per scaldare l'acqua sanitaria molto in fretta o per il ciclo mensile di disinfezione termica (antilegionella) |
| **T2 / T2B**        | Sensori Gas Refrigerante         | *(Dato tecnico)*: Misurano lo stato del gas freon dentro i tubi                                                                                                  |
| **T3**              | Sensore Batteria Esterna         | *(Dato tecnico)*: Serve alla macchina per capire se si è formato del ghiaccio sull'unità esterna in inverno (per avviare lo sbrinamento)                         |
| **Th / Tp**         | Sensori del Compressore          | *(Dato tecnico)*: Indicano le temperature del motore                                                                                                             |
| **Pe**              | Pressione Gas                    | *(Dato tecnico)*: Indica a che pressione sta lavorando l'impianto. Utile per capire se c'è una perdita di gas                                                    |

# Gateway USR-TCP232-410S-H7 
La serie USR-TCP232-410s esiste in **quattro varianti hardware**, rilasciate in sequenza: **M4**, **HB**, **RT** e **H7**. Sono esteticamente identiche e i rivenditori le vendono quasi sempre come "410s" generico, senza specificare il suffisso. Le differenze però sono sostanziali.
 
| Caratteristica             | M4        | HB        | RT        | H7        |
|---                         |:---:      |:---:      |:---:      |:---:      |
| Processore                 | Cortex-M4 | Cortex-M7 | Cortex-M7 | Cortex-M7 |
| Frequenza                  | 120 MHz   | 400 MHz   | 400 MHz   | 400 MHz   |
| Conversione Modbus TCP/RTU | SI        | SI        | SI        | SI        |
| Edge Computing             | NO        | NO        | NO        | SI        |
| MQTT                       | NO        | NO        | NO        | SI        |
| JSON                       | NO        | NO        | NO        | SI        |
| SSL/TLS                    | NO        | NO        | NO        | SI        |
| 485 Anti-Collision         | NO        | NO        | NO        | SI        |
| NTP                        | NO        | NO        | NO        | SI        |
| Firmware                   | 30xx      | 71xx      | 80xx      | 72xx      |
 
Ho scelto la variante **H7** per tre motivi concreti legati al mio impianto — non per il generico "ha più funzioni":
1. **485 Anti-Collision**: è la funzione decisiva. Il bus RS485 della pompa di calore è **condiviso con il comando cablato** della macchina. Questa funzione impedisce al gateway di trasmettere mentre il bus è occupato dal comando cablato, prevenendo le collisioni che causano errori di comunicazione intermittenti. È l'unica variante che ce l'ha, ed è mirata esattamente a questo scenario.
2. **MQTT con scrittura registri**: se in futuro vorrò disaccoppiare il polling da Home Assistant, la H7 può fare da gateway MQTT completo (legge i registri, pubblica in JSON, riceve comandi di scrittura).
3. **Edge computing**: permette al gateway di interrogare autonomamente la pompa e restituire i dati già impacchettati, alleggerendo Home Assistant.
 
Sul web la variante non è quasi mai specificata, e la variante fisica non è sempre stampata in modo leggibile sul dispositivo. Il metodo sicuro è leggere la versione firmware dalla web UI seguendo il percorso **Status → Overview → Type**.
La corrispondenza è inequivocabile:
![Variante gateway](images/01-usr-version-type.png)

 
### Consiglio d'acquisto
 
Comprare da un venditore con **reso facile** (store ufficiale PUSR o distributori come DigiKey) e **specificare per iscritto** nell'ordine che si vuole la versione H7 (firmware 72xx). Alla consegna, verificare il firmware **prima** di installare: se è una variante diversa, restituirla.
 
---
 
## Collegamento fisico
 
- **Alimentazione**: 5–36 V DC (alimentatore in dotazione). Il modulo **non** è PoE.
- **Ethernet**: dal modulo al router.
- **RS485 verso la pompa** (collegare **dopo** aver configurato il modulo):
  - **A+** → **H2** sul PCB del comando cablato
  - **B−** → **H1**
  - **GND/E** → connessione earth (opzionale; con cavo schermato collegare la schermatura da **un solo lato** per evitare loop di massa)
> Il comando cablato deve **restare collegato** al modulo idraulico, altrimenti i registri non rispondono.
 
**➡️ [SCREENSHOT QUI (opzionale): foto del cablaggio A+/B− ai morsetti H2/H1]**
 
---
 
## Accesso alla web UI
 
Il modulo esce con IP statico di fabbrica **192.168.0.7** (utente/password: **admin / admin**).
 
- Se la propria rete è già `192.168.0.x` → aprire `http://192.168.0.7`.
- Altrimenti → usare il tool ufficiale **EthernetTool** (scaricabile dal sito PUSR), che trova il modulo via broadcast anche su sottorete diversa, oppure cercare il nuovo dispositivo nella lista client del router.
---
 
## 1. Configurazione di rete (Network → IP Config)
 
**➡️ [SCREENSHOT QUI: pagina Network → IP Config]**
 
Il modulo di default può essere in **DHCP/AutoIP** e prendere un IP dal router. Il problema: in DHCP l'IP **potrebbe cambiare** a un riavvio del router, e Home Assistant perderebbe il contatto (nel file YAML l'IP è fisso).
 
**Cosa fare** — una delle due:
- Impostare **Method of IP Obtaining → Static IP** e fissare un IP libero della propria rete, **oppure**
- Lasciare DHCP e fare una **reservation** sul router (IP legato al MAC del modulo). *Soluzione consigliata*: si gestisce tutto da un punto solo.
Annotare l'IP: servirà nel campo `host` del file YAML di Home Assistant.
 
### La questione dei DNS
 
Nella stessa pagina compaiono due DNS di default: **114.114.114.114** e **223.5.5.5**. Sono **DNS pubblici cinesi** (114DNS e AliDNS), preimpostati dal produttore perché il dispositivo è di origine cinese.
 
**Nel setup Modbus TCP locale questi DNS sono irrilevanti**: il modulo comunica con Home Assistant tramite **indirizzo IP diretto**, non tramite nomi di dominio, quindi non deve risolvere nulla. Il DNS servirebbe solo per raggiungere server cloud tramite nome — esattamente ciò che **non** si vuole.
 
Si possono lasciare così senza problemi. Per pulizia, passando a IP statico si possono sostituire con il DNS del proprio router (es. `192.168.x.1`) o un DNS neutro (es. `1.1.1.1`). È una scelta puramente estetica, non cambia il funzionamento.
 
---
 
## 2. Parametri seriali (Port → RS485 → Port)
 
**➡️ [SCREENSHOT QUI: pagina Port → RS485 → tab "Port"]**
 
Impostare i parametri seriali **identici a quelli del dispositivo Modbus** (per la Baxi Auriga: 9600-8-N-1).
 
| Campo | Valore | Note |
|---|---|---|
| **Baud rate** | **9600** | Il default è 115200 → cambiare |
| Data bits | 8 | |
| Parity | None | |
| Stop bits | 1 | |
| Flow ctrl | NONE | |
| UART Packet Length | 0 | default |
| UART Packet Time | 0 | default |
| Restarting Without Uartdata | 0 | disattivato |
| **Sync Baudrate (RFC2217)** | **OFF** | Il default è ON → **disattivare** |
| Uart Heartbeat Type | NONE | |
 
**Perché disattivare Sync Baudrate (RFC2217)**: questa funzione permette a un software di rete di cambiare al volo i parametri seriali inviando un pacchetto speciale. Con Modbus non serve e potrebbe alterare il baud rate in modo indesiderato. Meglio bloccarlo su OFF così il baud rate resta fisso.
 
Cliccare **Save&Apply**.
 
---
 
## 3. Modalità Modbus TCP (Port → RS485 → Socket)
 
**➡️ [SCREENSHOT QUI: pagina Port → RS485 → tab "Socket"]**
 
Questa è la parte **più importante**: attiva la conversione da Modbus TCP (lato rete) a Modbus RTU (lato seriale), che permette di usare `type: tcp` pulito in Home Assistant.
 
| Campo | Valore | Note |
|---|---|---|
| Working Mode (1° menu) | **TCP Server** | |
| Working Mode (2° menu) | **Modbus TCP** | Il default è "None" → **selezionare Modbus TCP**. *È la conversione cruciale* |
| Local Port Number | **502** | Il default è 26 → cambiare in 502 (porta standard Modbus TCP) |
| Maximum Sockets supported | 8 | default |
| Exceeding Maximum | KICK | default |
| PRINT | OFF | |
| **Modbus Poll** | **✅ spuntato** | Attiva il polling Modbus |
| → Response Timeout | 200 ms | ok |
| → Interval | **50–100 ms** | consigliato, per dare respiro al bus condiviso |
| Modbus Cache | ❌ deselezionato | Non memorizzare valori: si vogliono letture fresche |
| Modbus TCP Exception | ❌ deselezionato | Vedi nota sotto |
| Net Heartbeat Type | NONE | |
| SOCKET B → Operating Mode | None | non serve |
 
> **Modbus TCP Exception** — in fase di **debug iniziale** può essere utile attivarlo temporaneamente: fa sì che Home Assistant riceva i codici di errore Modbus (es. "illegal address") invece di un timeout generico, aiutando a capire perché un registro non risponde. A regime si può lasciare OFF.
 
Cliccare **Save&Apply**.
 
---
 
## 4. Impostazioni di sistema (System → System Setting)
 
**➡️ [SCREENSHOT QUI: pagina System → System Setting]**
 
| Campo | Valore | Note |
|---|---|---|
| **485 Anti-Collision** | **ON** | Il default è OFF → **attivare** (vedi sotto) |
| → 485 Idle Time | 10 ms | valore di default, ok |
| NTP | OFF | non serve per il Modbus TCP diretto |
| Mask BCAST | OFF | |
| Uart Cache | OFF | come Modbus Cache: no valori memorizzati |
| Restarting Without Data | 0 | disattivato |
| Web Switch | ON | lasciare ON per accedere alla web UI |
| Webserver Port | 80 | |
| Pass Word | *(consigliato cambiarla dal default `admin`)* | igiene di base |
 
**Perché attivare 485 Anti-Collision**: è la funzione esclusiva della H7 e il motivo principale della scelta. Con il bus RS485 condiviso col comando cablato della pompa, controlla lo stato del bus e non manda il pin EN in trasmissione mentre il bus è in ricezione, prevenendo le collisioni che causano anomalie di comunicazione.
 
Cliccare **Save&Apply**.
 
---
 
## 5. Isolamento dal cloud (Cloud Service + regola router)
 
Per garantire che il modulo resti **completamente locale**, servono **due livelli** di protezione complementari.
 
### Livello 1 — Disattivare le funzioni cloud nella web UI
 
**➡️ [SCREENSHOT QUI: pagina Cloud Service]**
 
Nel menu **Cloud Service** (e nelle relative sottosezioni) disattivare:
- **DM Cloud / PUSR Cloud** → OFF
- **MQTT** (se non usato) → OFF
- **HTTPD Client** → OFF
- ogni altra connessione a server remoti
Questo dice al modulo di **non tentare** connessioni verso l'esterno.
 
### Livello 2 — Regola sul router (blocco WAN)
 
Sul router, creare una regola che **nega l'accesso a internet (WAN)** all'IP del modulo, lasciando intatta la comunicazione **LAN**. Consigliato agganciare la regola al **MAC address** del modulo (o prima fare una reservation DHCP e poi bloccare l'IP ormai stabile), così resta valida anche se l'IP cambiasse.
 
Questo **impedisce fisicamente** al modulo di raggiungere internet, a prescindere dalle sue impostazioni interne.
 
### Test di verifica
 
Dopo aver applicato entrambi i livelli, verificare che **tutto continui a funzionare con la WAN bloccata**:
- la web UI del modulo resta raggiungibile in LAN ✅
- Home Assistant continua a leggere i registri ✅
Se entrambe funzionano a internet tagliato, è la **conferma pratica** che il modulo non ha alcuna dipendenza dal cloud.
 
---
 
## 6. Configurazione lato Home Assistant
 
Riepilogo dei parametri da usare nel file YAML (integrazione `modbus` nativa):
 
```yaml
modbus:
  - name: nome_dispositivo
    type: tcp
    host: 192.168.x.x        # IP del gateway USR
    port: 502
    delay: 5
    message_wait_milliseconds: 100   # respiro sul bus condiviso
    # ... sensori, con slave: N (Slave ID del dispositivo Modbus)
```
 
> Con la conversione **Modbus TCP** attiva sul gateway (passo 3), si usa `type: tcp`. Se invece si usasse solo il *transparent transmission* (senza conversione), servirebbe `type: rtuovertcp`.
 
**Primo test consigliato**: partire in **sola lettura**, verificare che un paio di sensori (es. temperatura esterna, temperatura acqua uscita) riportino valori coerenti col display della pompa. Se sì, la catena funziona end-to-end. Solo dopo abilitare le scritture.
 
> Se al primo tentativo si ottengono solo timeout con parametri seriali corretti, il primo sospettato è lo **Slave ID** (indirizzo Modbus del dispositivo, spesso impostato da un rotary switch sul PCB).
 
---
 
## Note sul bus condiviso e più dispositivi
 
- Il bus RS485 verso la pompa è **condiviso con il comando cablato**: non essere aggressivi col polling (intervalli di 10–15 s lato Home Assistant sono più che sufficienti per le temperature di una pompa di calore).
- **Terminazione**: su bus lunghi servono resistenze da 120 Ω alle estremità; su tratte brevi (pochi metri) spesso non sono necessarie.
- **Più dispositivi sullo stesso gateway?** Modbus RTU è multi-drop (fino a 16 slave su questo USR, con Slave ID diversi), ma **conviene solo** se i dispositivi sono fisicamente vicini, condividono gli stessi parametri seriali e il cablaggio a catena è comodo. Per dispositivi distanti o con parametri diversi (es. un inverter fotovoltaico vicino al quadro) è **più pulito e affidabile un secondo gateway dedicato**, con il suo bus separato e un secondo hub `modbus:` in Home Assistant.
---
 
*Documentazione a scopo personale/informativo. Verificare sempre la mappa registri sulla documentazione ufficiale del proprio dispositivo.*