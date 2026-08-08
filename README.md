# Baxi Auriga heat pump 

## Descrizione dei termini
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

## Mappa dei sensori e registri modbus
Di seguito viene riportato l'elenco di tutti i sensori acquisiti via modbus.
 
La colonna **Accesso** indica se un'entità è di sola lettura (**R**) oppure se può anche essere comandata (**R-W**). Per leggere o comandare una funzione, usare direttamente l'entità indicata in `template`.
 
### Comandi e impostazioni
 
| Nome sensore | Accesso | Descrizione |
| :--- | :---: | :--- |
| `sensor.system_power_state_raw` | R-W | Registro 0 - stato accensioni (bitfield grezzo) |
| `sensor.setting_mode` | R-W | Registro 1 - modalità impostata (1 Auto, 2 Cool, 3 Heat) |
| `sensor.setting_water_temperature_t1s_raw` | R-W | Registro 2 - setpoint acqua T1s grezzo (byte basso risc., byte alto raffr.) |
| `sensor.setting_air_temperature_raw` | R-W | Registro 3 - setpoint aria Ts grezzo (valore in mezzi gradi, x2) |
| `sensor.setting_water_tank_temperature` | R-W | Registro 4 - setpoint accumulo ACS T5s (°C) |
| `sensor.function_setting_raw` | R-W | Registro 5 - funzioni speciali (bitfield grezzo) |
| `sensor.curve_selection_raw` | R-W | Registro 6 - curva climatica grezza (byte basso risc., byte alto raffr.) |
| `sensor.forced_water_heating` | R-W | Registro 7 - forzatura produzione acqua calda |
| `sensor.forced_electric_water_tank_heater` | R-W | Registro 8 - forzatura resistenza accumulo TBH |
| `sensor.forced_electric_heater` | R-W | Registro 9 - forzatura resistenza integrativa IBH1 |
| `sensor.compressor_operating_frequency` | R | Registro 100 - frequenza compressore (Hz) |
| `sensor.operating_mode` | R | Registro 101 - modalità operativa reale (0 Off, 2 Cool, 3 Heat) |
| `sensor.fan_speed` | R | Registro 102 - velocità ventilatore (rpm) |
| `sensor.electronic_expansion_valve_openness` | R | Registro 103 - apertura valvola di espansione (multipli di 8) |
| `sensor.water_inlet_temperature` | R | Registro 104 - temperatura acqua ingresso TW_in (°C) |
| `sensor.water_outlet_temperature` | R | Registro 105 - temperatura acqua uscita TW_out (°C) |
| `sensor.condenser_temperature` | R | Registro 106 - temperatura condensatore T3 (°C) |
| `sensor.outdoor_ambient_temperature` | R | Registro 107 - temperatura aria esterna T4 (°C) |
| `sensor.discharge_temperature` | R | Registro 108 - temperatura mandata compressore Tp (°C) |
| `sensor.return_air_temperature` | R | Registro 109 - temperatura aspirazione compressore (°C) |
| `sensor.total_water_outlet_temperature` | R | Registro 110 - temperatura totale uscita acqua T1 (°C) |
| `sensor.total_water_outlet_temperature_behind_the_auxiliary_heater` | R | Registro 111 - temperatura acqua dopo riscaldatore ausiliario T1B (°C) |
| `sensor.refrigerant_liquid_side_temperature` | R | Registro 112 - temperatura refrigerante lato liquido T2 (°C) |
| `sensor.refrigerant_gas_side_temperature` | R | Registro 113 - temperatura refrigerante lato gas T2B (°C) |
| `sensor.room_temperature` | R | Registro 114 - temperatura ambiente Ta (°C) |
| `sensor.domestic_hot_water_tank_temperature` | R | Registro 115 - temperatura accumulo ACS T5 (°C) |
| `sensor.outdoor_unit_high_pressure` | R | Registro 116 - alta pressione unità esterna (kPa) |
| `sensor.outdoor_unit_low_pressure` | R | Registro 117 - bassa pressione unità esterna (kPa) |
| `sensor.outdoor_unit_operating_current` | R | Registro 118 - corrente assorbita unità esterna (A) |
| `sensor.outdoor_unit_voltage` | R | Registro 119 - tensione unità esterna (V) |
| `sensor.compressor_operating_time` | R | Registro 122 - ore funzionamento compressore (h) |
| `sensor.current_fault` | R | Registro 124 - codice errore attuale |
| `sensor.status_bit_1` | R | Registro 128 - stato macchina (bitfield grezzo) |
| `sensor.load_output` | R | Registro 129 - stato uscite/carichi (bitfield grezzo) |
| `sensor.hydraulic_module_version_number` | R | Registro 130 - versione modulo idronico |
| `sensor.wired_controller_version_number` | R | Registro 131 - versione comando a filo |
| `sensor.unit_target_frequency` | R | Registro 132 - frequenza target compressore (Hz) |
| `sensor.dc_bus_current` | R | Registro 133 - corrente bus DC (A) |
| `sensor.dc_bus_voltage` | R | Registro 134 - tensione bus DC (V, valore/10) |
| `sensor.inverter_module_temperature` | R | Registro 135 - temperatura modulo inverter TF (°C) |
| `sensor.hydraulic_module_curve_t1s_calculated_value_1` | R | Registro 136 - valore T1S calcolato zona 1 (°C) |
| `sensor.hydraulic_module_curve_t1s_calculated_value_2` | R | Registro 137 - valore T1S calcolato zona 2 (°C) |
| `sensor.water_flow` | R | Registro 138 - portata acqua (m³/h, valore x100) |
| `sensor.limit_outdoor_unit_current` | R | Registro 139 - schema limite corrente unità esterna |
| `sensor.ability_of_hydraulic_module` | R | Registro 140 - potenza resa modulo idronico (kW, valore x100) |
| `sensor.water_return_pump_running_time` | R | Registro 209 - tempo di funzionamento pompa ricircolo ACS (min) |
| `sensor.parameter_setting_1_raw` | R | Registro 210 - parametri configurazione 1 (bitfield grezzo) |
| `sensor.parameter_setting_2_raw` | R | Registro 211 - parametri configurazione 2 (bitfield grezzo) |

---

## Entità `template`
Entità elaborate a partire dai sensori Modbus grezzi (decodifica di bit, byte impacchettati, scale). La colonna **Accesso** indica se l'entità è di sola lettura (**R**) o scrivibile (**R-W**). Le entità scrivibili comandano la pompa scrivendo sui registri con logica read-modify-write.

### Comandi

| Nome entità | Accesso | Descrizione |
| :--- | :---: | :--- |
| `switch.system_power_floor_heating` | R-W | Registro 0 BIT0 - accensione riscaldamento a pavimento |
| `switch.system_power_air_conditioner_zone_1` | R-W | Registro 0 BIT1 - accensione climatizzazione zona 1 (riscaldamento su questo impianto) |
| `switch.system_power_domestic_hot_water` | R-W | Registro 0 BIT2 - accensione acqua calda sanitaria |
| `switch.system_power_air_conditioner_zone_2` | R-W | Registro 0 BIT3 - accensione climatizzazione zona 2 |
| `switch.disinfect` | R-W | Registro 5 BIT4 - disinfezione termica (antilegionella) |
| `switch.silent_mode` | R-W | Registro 5 BIT6 - modalità silenziosa |
| `switch.eco_mode` | R-W | Registro 5 BIT7 - modalità Eco |
| `switch.dhw_water_recycling` | R-W | Registro 5 BIT11 - ricircolo pompa ACS |
| `switch.climate_curve` | R-W | Registro 5 BIT12 - abilitazione curva climatica |
| `select.setting_operating_mode` | R-W | Registro 1 - modalità: Auto / Raffrescamento / Riscaldamento |
| `select.setting_silent_mode` | R-W | Registro 5 - silenzioso: Non attivo / Livello 1 / Livello 2 |
| `select.heating_curve_selection` | R-W | Registri 6+5 - curva riscaldamento: Non attiva / 1-8 |
| `select.cooling_curve_selection` | R-W | Registri 6+5 - curva raffrescamento: Non attiva / 1-8 |
| `select.forced_water_heating` | R-W | Registro 7 - forzatura ACS: Auto / Forzata on / Forzata off |
| `select.forced_electric_water_tank_heater` | R-W | Registro 8 - forzatura TBH: Auto / Forzata on / Forzata off |
| `select.forced_electric_heater` | R-W | Registro 9 - forzatura IBH1: Auto / Forzata on / Forzata off |
| `number.setting_heating_water_temperature` | R-W | Registro 2 - setpoint acqua riscaldamento (20-60 °C) |
| `number.setting_cooling_water_temperature` | R-W | Registro 2 - setpoint acqua raffrescamento (5-25 °C) |
| `number.setting_air_temperature` | R-W | Registro 3 - setpoint aria Ts (17-30 °C) |
| `number.setting_domestic_hot_water_temperature` | R-W | Registro 4 - setpoint accumulo ACS T5s (40-60 °C) |
| `number.setting_water_return_pump_running_time` | R-W | Registro 209 - tempo funzionamento pompa ricircolo ACS |

### Stato uscite / carichi

| Nome entità | Accesso | Descrizione |
| :--- | :---: | :--- |
| `binary_sensor.load_run` | R | Unità in funzione (RUN) |
| `binary_sensor.load_defrost` | R | Sbrinamento in corso |
| `binary_sensor.load_alarm` | R | Allarme attivo |
| `binary_sensor.load_ibh1_electric_heater` | R | Resistenza elettrica IBH1 |
| `binary_sensor.load_tbh_electric_heater` | R | Resistenza elettrica accumulo TBH |
| `binary_sensor.load_external_heater` | R | Resistenza esterna |
| `binary_sensor.load_internal_water_pump_p_i` | R | Pompa circolazione interna P_i |
| `binary_sensor.load_external_water_pump_p_o` | R | Pompa circolazione esterna P_o |
| `binary_sensor.load_water_return_pump_p_d` | R | Pompa ricircolo ACS P_d |
| `binary_sensor.load_mixed_water_pump_p_c` | R | Pompa acqua miscelata P_c |
| `binary_sensor.load_solar_water_pump_p_s` | R | Pompa circolazione solare P_s |
| `binary_sensor.load_sv1` | R | Valvola deviatrice SV1 (riscaldamento/ACS) |
| `binary_sensor.load_sv2` | R | Valvola SV2 |
| `binary_sensor.load_heat4` | R | Uscita ausiliaria HEAT4 |

### Stato macchina

| Nome entità | Accesso | Descrizione |
| :--- | :---: | :--- |
| `binary_sensor.status_defrost` | R | Sbrinamento |
| `binary_sensor.status_antifreeze` | R | Antigelo |
| `binary_sensor.status_oil_return` | R | Ritorno olio |
| `binary_sensor.status_remote_on_off` | R | Remote On/Off |
| `binary_sensor.status_outdoor_unit_test` | R | Segnale test unità esterna |
| `binary_sensor.status_thermostat_heating` | R | Termostato ambiente (riscaldamento) |
| `binary_sensor.status_thermostat_cooling` | R | Termostato ambiente (raffrescamento) |
| `binary_sensor.status_solar_input` | R | Ingresso segnale solare |
| `binary_sensor.status_sg_signal` | R | Segnale SG (on = tariffa normale, off = prezzo alto) |
| `binary_sensor.status_evu_signal` | R | Segnale EVU (on = elettricità gratis, off = valuta segnale SG) |

### Configurazione installatore

| Nome entità | Accesso | Descrizione |
| :--- | :---: | :--- |
| `binary_sensor.param_enable_heating` | R | Riscaldamento abilitato |
| `binary_sensor.param_enable_cooling` | R | Raffrescamento abilitato |
| `binary_sensor.param_enable_water_heating` | R | Produzione ACS abilitata |
| `binary_sensor.param_priority_dhw_first` | R | Priorità (on = ACS prima, off = riscaldamento prima) |
| `binary_sensor.param_room_thermostat` | R | Termostato ambiente |
| `binary_sensor.param_dual_room_thermostat` | R | Doppio termostato ambiente supportato |
| `binary_sensor.param_supports_room_thermostat` | R | Supporta termostato ambiente |
| `binary_sensor.param_supports_room_temperature_sensor` | R | Supporta sonda ambiente Ta |
| `binary_sensor.param_supports_sensor_t1` | R | Supporta sonda T1 |
| `binary_sensor.param_support_dhw_pump` | R | Pompa ACS supportata |
| `binary_sensor.param_dhw_pump_pipe_disinfect` | R | Pompa ACS supporta disinfezione tubazioni |
| `binary_sensor.param_supports_disinfection` | R | Supporta disinfezione |
| `binary_sensor.param_supports_tbh_heater` | R | Supporta resistenza accumulo TBH (read-only) |
| `binary_sensor.param_double_zone_valid` | R | Doppia zona valida |
| `binary_sensor.param_set_t1s_heating_high_low` | R | Impostazione high/low T1s riscaldamento |
| `binary_sensor.param_set_t1s_cooling_high_low` | R | Impostazione high/low T1s raffrescamento |
| `binary_sensor.param_enable_tw2` | R | Abilitazione Tw2 |
| `binary_sensor.param_smart_grid` | R | Smart grid |
| `binary_sensor.param_port_dhw_heater` | R | Porta configurabile (on = resistenza ACS, off = remote ON/OFF) |

---

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
 
Ho scelto la variante **H7** per tre motivi concreti legati al mio impianto — non per il generico "ha più funzioni":
1. **485 Anti-Collision**: è la funzione decisiva. Il bus RS485 della pompa di calore è **condiviso con il comando cablato** della macchina. Questa funzione impedisce al gateway di trasmettere mentre il bus è occupato dal comando cablato, prevenendo le collisioni che causano errori di comunicazione intermittenti. È l'unica variante che ce l'ha, ed è mirata esattamente a questo scenario.
2. **MQTT con scrittura registri**: se in futuro vorrò disaccoppiare il polling da Home Assistant, la H7 può fare da gateway MQTT completo (legge i registri, pubblica in JSON, riceve comandi di scrittura).
3. **Edge computing**: permette al gateway di interrogare autonomamente la pompa e restituire i dati già impacchettati, alleggerendo Home Assistant.
 
Sul web la variante non è quasi mai specificata, e la variante fisica non è sempre stampata in modo leggibile sul dispositivo. Il metodo sicuro è leggere la versione firmware dalla web UI seguendo il percorso: **Status → Overview → Type**.

La corrispondenza è inequivocabile:
![Variante gateway](images/01-usr-version-type.png)
 
Consiglio di acquistare da un venditore con reso facile e specificare per iscritto nell'ordine che si vuole la versione H7. Alla consegna, verificare il tipo prima di installarlo.


## Collegamento fisico
- **Alimentazione**: 5–36 V DC (alimentatore in dotazione). Il modulo non è PoE.
- **Ethernet**: dal modulo al router.
- **RS485 verso la pompa** (collegare dopo aver configurato il modulo):
  - **A +** → **H2** sul PCB del comando cablato
  - **B −** → **H1** sul PCB del comando cablato
  - **E** → connessione earth (opzionale, con cavo schermato collegare la schermatura da un solo lato per evitare loop di massa)
Il comando cablato deve restare collegato al modulo idraulico, altrimenti i registri non rispondono.
 
![Cablaggio HMI Baxi](images/01-baxi-hmi-wiring.png)


## Accesso alla web UI
Il modulo esce con IP statico di fabbrica **192.168.0.7** (utente/password: **admin/admin**).
- Se la propria rete è già `192.168.0.x` → aprire `http://192.168.0.7`.
- Altrimenti usare il tool ufficiale **EthernetTool** (scaricabile dal sito PUSR), che trova il modulo via broadcast anche su sottorete diversa, oppure cercare il nuovo dispositivo nella lista client del router.


## Configurazione di rete (Network → IP Config) 
Il modulo di default può essere in **DHCP/AutoIP** e prendere un IP dal router, ma in DHCP l'IP potrebbe cambiare a un riavvio del router, e Home Assistant perderebbe il contatto (nel file YAML l'IP è fisso).
 
**Cosa fare** — una delle due:
- Impostare **Method of IP Obtaining → Static IP** e fissare un IP libero della propria rete, **oppure**
- Lasciare DHCP e fare una **reservation** sul router (IP legato al MAC del modulo). 
  *Soluzione consigliata*: si gestisce tutto da un punto solo.
 
### DNS
Nella stessa pagina compaiono due DNS di default: **114.114.114.114** e **223.5.5.5**. Sono **DNS pubblici cinesi** (114DNS e AliDNS), preimpostati dal produttore perché il dispositivo è di origine cinese.
 
**Nel setup Modbus TCP locale questi DNS sono irrilevanti**: il modulo comunica con Home Assistant tramite **indirizzo IP diretto**, non tramite nomi di dominio, quindi non deve risolvere nulla. Il DNS servirebbe solo per raggiungere server cloud tramite nome — esattamente ciò che **non** voglio.
 
Si possono lasciare così senza problemi. Per pulizia, passando a IP statico si possono sostituire con il DNS del proprio router (es. `192.168.x.1`) o un DNS neutro (es. `1.1.1.1`). È una scelta puramente estetica, non cambia il funzionamento.

![Configurazione IP](images/02-usr-ip-config.png)

Cliccare **Save&Apply**.


## Parametri seriali (Port → RS485 → Port)
Impostare i parametri seriali identici a quelli del dispositivo Modbus 
(per la Baxi Auriga: **9600-8-N-1**):

| Campo                       | Valore | Note                |
|---                          |---     |---                  |
| Baud rate                   | 9600   | Il default è 115200 |
| Data bits                   | 8      |                     |
| Parity                      | None   |                     |
| Stop bits                   | 1      |                     |
| Flow ctrl                   | NONE   |                     |
| UART Packet Length          | 0      | default             |
| UART Packet Time            | 0      | default             |
| Restarting Without Uartdata | 0      | disattivato         |
| Sync Baudrate (RFC2217)     | OFF    | Il default è ON     |
| Uart Heartbeat Type         | NONE   |                     |

**Perché disattivare Sync Baudrate (RFC2217)?**: questa funzione permette a un software di rete di cambiare al volo i parametri seriali inviando un pacchetto speciale. Con Modbus non serve e potrebbe alterare il baud rate in modo indesiderato. Meglio bloccarlo su OFF così il baud rate resta fisso.
 
![Configurazione RS485 Port](images/03-usr-rs485-port-config.png)

Cliccare **Save&Apply**.


## Modalità Modbus TCP (Port → RS485 → Socket) 
Questa è la parte più importante: attiva la conversione da Modbus TCP (lato rete) a Modbus RTU (lato seriale), che permette di usare `type: tcp` pulito in Home Assistant.
 
| Campo                     | Valore     | Note                                           |
|---                        |---         |---                                             |
| Working Mode (1° menu)    | TCP Server |                                                |
| Working Mode (2° menu)    | Modbus TCP | Il default è "None"                            |
| Local Port Number         | 502        | Il default è 26                                |
| Maximum Sockets supported | 8          | Default                                        |
| Exceeding Maximum         | KICK       | Default                                        |
| PRINT                     | OFF        |                                                |
| Modbus Poll               | SI         | Attiva il polling Modbus                       |
| → Response Timeout        | 200 ms     |                                                |
| → Interval                | 50–100 ms  | Consigliato, per dare respiro al bus condiviso |
| Modbus Cache              | NO         | Non memorizzare valori                         |
| Modbus TCP Exception      | NO         | Vedi nota sotto                                |
| Net Heartbeat Type        | NONE       |                                                |
| SOCKET B → Operating Mode | None       | non serve                                      |
 
**Modbus TCP Exception**: in fase di **debug iniziale** può essere utile attivarlo temporaneamente: fa sì che Home Assistant riceva i codici di errore Modbus (es. "illegal address") invece di un timeout generico, aiutando a capire perché un registro non risponde. A regime si può lasciare OFF.

![Configurazione RS485 Socket](images/04-usr-rs485-socket-config.png)
 
Cliccare **Save&Apply**.

 
## Impostazioni di sistema (System → System Setting)
 
| Campo                   | Valore | Note                                     |
|---                      |---     |---                                       |
| 485 Anti-Collision      | ON     | il default è OFF                         |
| → 485 Idle Time         | 10 ms  |                                          |
| NTP                     | OFF    | non serve per il Modbus TCP diretto      |
| Mask BCAST              | OFF    |                                          |
| Uart Cache              | OFF    | come Modbus Cache: no valori memorizzati |
| Restarting Without Data | 0      | disattivato                              |
| Web Switch              | ON     | lasciare ON per accedere alla web UI     |
| Webserver Port          | 80     |                                          |
| Pass Word               | admin  | consigliato cambiarla                    |

![Configurazione sistema](images/05-usr-system-config.png)

Cliccare **Save&Apply**.

 
## Isolamento dal cloud (Cloud Service + regola router)
Per garantire che il modulo resti **completamente locale**, servono due livelli di protezione complementari.
 
### Livello 1 — Disattivare le funzioni cloud nella web UI
Nel menu **Cloud Service** (e nelle relative sottosezioni) disattivare:
- **DM Cloud / PUSR Cloud** → OFF
- **MQTT** (se non usato) → OFF
- **HTTPD Client** → OFF
- ogni altra connessione a server remoti

Questo dice al modulo di non tentare connessioni verso l'esterno.
 
### Livello 2 — Regola sul router (blocco WAN)
Sul router, creare una regola che **nega l'accesso a internet (WAN)** all'IP del modulo, lasciando intatta la comunicazione **LAN**. Consigliato agganciare la regola al **MAC address** del modulo (o prima fare una reservation DHCP e poi bloccare l'IP ormai stabile), così resta valida anche se l'IP cambiasse.
 
Questo impedisce fisicamente al modulo di raggiungere internet, a prescindere dalle sue impostazioni interne.

---
 
# Configurazione lato Home Assistant
L'integrazione è organizzata con il sistema dei `package` di Home Assistant, così tutto ciò che riguarda la pompa di calore Baxi resta raccolto in un unico punto, separato dal resto della configurazione.

```
/config/
├── configuration.yaml           # attiva i package (vedi sotto)
└── packages/
    └── auriga_modbus.yaml       # tutta la configurazione Baxi
```

Perché Home Assistant carichi la cartella `packages/`, in `configuration.yaml` deve essere presente la riga:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Note importanti:
- La chiave `homeassistant:` deve comparire **una sola volta** nel file. Se esiste già (di solito c'è di default), va aggiunta solo la riga `packages: ...` al suo interno con l'indentazione corretta (2 spazi), senza creare un secondo blocco `homeassistant:`.
- La cartella deve chiamarsi **esattamente** `packages` e trovarsi in `/config/packages/`, perché `!include_dir_named packages` la cerca lì.

Il file del package contiene l'hub Modbus con, al suo interno, l'intera lista dei sensori:

```yaml
modbus:
  - name: DEVICE_NAME
    type: tcp
    host: IP_ADDRESS
    port: 502
    delay: 5
    message_wait_milliseconds: 100
    sensors:
      - name: ...
        address: ...
        ...
      # ... tutti gli altri sensori ...
```
> Con la conversione **Modbus TCP** attiva sul gateway, si usa `type: tcp`. Se invece si usasse solo il *transparent transmission* (senza conversione), servirebbe `type: rtuovertcp`.

Verifica dopo le modifiche:
1. **Strumenti per sviluppatori → Controlla configurazione**: deve restituire "Configurazione valida".
2. **Riavviare** Home Assistant (le modifiche a `modbus:` richiedono un riavvio completo, non basta il semplice "ricarica").
3. **Strumenti per sviluppatori → Stati**: cercare i sensori e verificare che riportino valori sensati. 

Se dopo il riavvio di Home Assistant tutti i sensori Modbus restano su `unknown` o `unavailable`, significa che le richieste partono ma la pompa di calore non risponde. Nei log (Impostazioni → Sistema → Log, filtro `modbus`) si vede tipicamente:

```
No response received after 3 retries, continue with next request
```

Questo indica che la comunicazione fisica arriva fino al gateway, ma lo slave interrogato non replica. La causa più frequente è un **indirizzo slave non corrispondente**.

## Verifica dell'indirizzo sul modulo idraulico
L'indirizzo Modbus della pompa si imposta sulla **scheda dell'unità esterna**, tramite un **selettore rotativo (rotary switch) da 0 a F** — oppure, su alcune versioni, un blocco di dip-switch. Il valore è in esadecimale: 0–9 = 0–9, A=10, B=11, C=12, D=13, E=14, F=15.

**Di fabbrica il selettore è impostato su `0`.** Questo è un problema, perché in Modbus l'indirizzo `0` è riservato al broadcast: nessuno slave risponde a una richiesta indirizzata a 0. Con la rotella su `0` la pompa quindi non risponde, e tutti i sensori restano `unavailable`.

È necessario quindi portare il selettore su `1` e impostare `slave: 1` nella configurazione Home Assistant (già il valore usato in questi file).
In alternativa, qualsiasi valore da 1 a F va bene, purché il numero sul selettore coincida con il campo `slave:` di tutti i sensori.

![Configurazione DIP switch](images/02-baxi-dip-switch.png)
