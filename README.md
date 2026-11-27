# Codereview Vesta MAIS – Actualisatie Startanalyse aardgasvrije buurten 2026 (ASA2026)

Onderzochte code: [pbl-nl/model-vesta-public](https://github.com/pbl-nl/model-vesta-public/tree/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d), branch `ActualisatieStartanalyse2026`, commit `0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d`.  
Datum: 24 november 2026 · Auteur: Jeroen Bussmann – Regionale Expertgroep Duurzaam Energiesysteem (REDE)

## Inleiding

Dit document bevat de bevindingen van een onafhankelijke codebeoordeling van Vesta MAIS, zoals gebruikt voor de *Actualisatie Startanalyse aardgasvrije buurten 2026* (ASA2026). De beoordeling bevat 4 grote issues (categorie A) en 7 kleinere issues (categorie B). Omdat het tabblad *Issues* op de GitHub-repository van het PBL is uitgeschakeld, zijn de bevindingen hier openbaar vastgelegd.

De review is uitgevoerd door de Regionale Expertgroep Duurzaam Energiesysteem (REDE), een onafhankelijke expertgroep van bewoners in de regio Holland Rijnland. Wij leveren gevraagd en ongevraagd een bijdrage aan de besluitvorming over de warmtetransitie, onder meer door second opinions te maken op beleidsbepalende rapporten. De aanleiding voor deze review was een afwijkende uitkomst voor buurten met een bestaand stadswarmtenet in Leiden en Leiderdorp. Daarna is de volledige broncode nagelopen.

## Samenvatting

Drie issues (`A1`, `A2` en `A3`) hebben landelijk en lokaal de grootste impact op de uitkomsten en de voorkeursstrategie per buurt, aangevuld met een zwaarwegende aanname over transportkosten bij bestaande warmtenetten (`A4`). De meeste issues zitten al sinds de eerste versie van de Startanalyse (2020) in het model. Gemeenten hebben die cijfers gebruikt voor hun plannen voor de warmtetransitie en vaak ook voor hun Transitievisie Warmte.

Samen zorgen deze issues ervoor dat het model voor naar schatting zeker 0,75 miljoen woningen een andere warmtetechniek aanwijst als optie met de laagste nationale kosten. Dat vraagt om een herbeoordeling van de keuzes die gemeenten en andere beleidsmakers op basis van deze gegevens hebben gemaakt, en om een heranalyse van rapporten die op de Startanalyse voortbouwen.

1. **Isolatiekosten worden niet gecorrigeerd voor woningen op een bestaand warmtenet (`A1`)**  
   Bij vrijwel alle kostenposten voor nieuwe maatregelen vermenigvuldigt het model met `R_AardG` (`1 - R_StadV`), het aandeel woningen dat nog op aardgas zit. Bij de isolatiekosten (`K08`, `K19`, `K21`) ontbreekt die factor. In buurten met een bestaand net worden daardoor isolatiekosten gerekend voor álle woningen, inclusief woningen die al op het warmtenet zitten. Het gaat om 937 buurten ($\ge 50$ woningen) met in totaal 1,17 miljoen woningen, waarvan 0,43 miljoen al aangesloten. Een eerste schatting op basis van de gepubliceerde resultaten laat zien dat na correctie in circa 190 buurten (220.000 woningen) de goedkoopste strategie verschuift, meestal van een warmtenet (`S2`) naar `S3` of `S1`. Dit zijn vaak buurten die nu onterecht als 'robuust' voor een warmtenet zijn aangemerkt.

2. **Meterkastkosten staan in het rapport, maar ontbreken in de uitkomsten (`A2`)**  
   `Ki_Meterkast` wordt berekend in `GebouwOptieT.dms`, maar nooit doorgegeven aan de allocatieresultaten en `K09`. Hierdoor vallen strategieën met een warmtepomp circa € 25–37 per woning per jaar te goedkoop uit ten opzichte van `S2`.

3. **Koudelevering bij WKO wordt als kostenpost opgeteld in plaats van als baat (`A3`)**  
   In kostenpost `K13` staat een plusteken voor `Om_ge_comfort`. In de Startanalyse 2019 stond hier nog een minteken; sinds een herindeling in februari 2020 staat er een plus. Het Functioneel Ontwerp boekt deze post als baat voor de gebouwgebruiker, terwijl het informatieblad *Koudevraag in de gebouwde omgeving* (maart 2025) stelt dat koudelevering buiten de nationale kosten valt. Hierdoor worden WKO-varianten € 110–225 per aangesloten woning per jaar te duur gerekend. In de gepubliceerde resultaten stijgt `K13` met € 38–61 per GJ koude (en € 0 bij varianten zonder koude). Na correctie verschuift naar schatting voor tot wel 1,7 miljoen woningen de goedkoopste strategie, meestal van een individuele warmtepomp (`S1`) naar WKO (`S3`). Hiernaast moet ook het electriciteitsverbruik voor koeling uit de WKO varianten worden verwijderd.

4. **Transportkosten op nul bij een bestaand warmtenet (`A4`)**  
   Zodra `R_StadV > 0` zet het model de transportkosten (`K06`) direct op nul (558 buurten; 0,83 miljoen woningen). In buurten met een klein bestaand warmtenet wordt een uitbreiding van het net daardoor mogelijk aanzienlijk te goedkoop gerekend.

Daarnaast zijn er kleinere issues vastgesteld (`B1`–`B7`), waaronder een onverklaarde opslag op alle elektriciteit per buurt via het landelijke warmtepompaandeel (`B2`), koudeverkoop in de selectie voor 70 °C-WKO-netten (`B3`), een dubbele correctie bij klimaatneutraal gas (`B4`), kopieerfouten in beheer en onderhoud van collectieve warmtepompen (`B5`), een verkeerde factor in gevoeligheidsanalyse `G05` (`B6`) en een eenhedenfout in de pompenergie van industriële restwarmte (`B7`).

De issues werken niet allemaal dezelfde kant op. `A1` benadeelt strategieën met een grote isolatiestap (`S1`, `S3`) in buurten met een bestaand warmtenet, terwijl `A4` warmtenetten in die buurten juist goedkoper maakt. `A3` en `B5` maken WKO/warmtenetten te duur (`B7` maakt industriële restwarmte iets te goedkoop). `A2` en `B2` maken warmtepompstrategieën respectievelijk te goedkoop en te duur.

## Wat is bekeken

* **Code en invoerbestanden:** Repository [pbl-nl/model-vesta-public](https://github.com/pbl-nl/model-vesta-public/tree/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d), branch `ActualisatieStartanalyse2026`, commit `0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d` (kentallen, installaties, energieprijzen en bronnen).
* **Gepubliceerde buurtresultaten:** Eerder gedownloade gemeentebestanden, waarin variant `S3g` nog als `s3g_D_WKO15_15` staat (van vóór de aanpassing van `S3g` in ASA2026).
* **Documentatie:**
  * *Concept verdiepend rapport Startanalyse 2025* (PBL, 27 februari 2025)
  * *Advies van CE Delft over de validatiesessies* (maart 2025)
  * *Informatieblad Koudevraag in de gebouwde omgeving* (PBL, maart 2025, publicatie 5840)
  * *Achtergrondrapport Startanalyse 2020* (PBL, 15 april 2022, publicatie 4917)
  * *Functioneel Ontwerp Vesta MAIS 3.0 / 5.0* (PBL)
  * *Definitief Achtergrondrapport ASA2026* (PBL, 7 oktober 2026)

De ruimtelijke brondata (BAG-koppeling, energielabels, CBS-verbruik, bestaande netten en de bronnenlijst) zitten niet in de openbare repository en zijn dus niet gecontroleerd.

---

## A. Belangrijke issues

### A1. Isolatiekosten niet gecorrigeerd voor woningen op een bestaand warmtenet

**Locatie:** `model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms` — [R_AardG (r. 15–16)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L15-L16), [K08 (r. 1107–1115)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1107-L1115), [K19 (r. 1238)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1238) en [K21 (r. 1264)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1264).

`R_AardG = 1 - R_StadV` geeft het aandeel van de buurt aan dat nog op aardgas zit. Het model vermenigvuldigt vrijwel alle kosten van nieuwe maatregelen met `R_AardG`, maar bij `K08` ontbreekt die factor:

```dms
K08_Gebouw_schil := Maatschappelijk/gebouwen/Kmi_GebouwVerbetering - Origkosten/table/K08_Gebouw_schil
```

Direct daaronder staat `K09` ([r. 1117–1131](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1117-L1131)), waar `R_AardG` wél voor elke term staat (`Kmi_30`, `_20`, `_15`, LTAS, proceskosten en inpandige kosten). Ook `K04`, `K05`, `K07` en `K15` bevatten deze factor.

Hierdoor worden in buurten met een bestaand warmtenet isolatiekosten gerekend voor álle woningen, ook voor woningen die al op het warmtenet zitten. De energiekosten worden wel gecorrigeerd, de isolatiekosten niet. Strategieën met veel isolatie (label B: `S1`, `S3`) vallen daardoor relatief duurder uit dan strategieën met weinig isolatie (label D).

**Oplossing:** Vermenigvuldig `K08`, `K19` en `K21` met `R_AardG`, analoog aan `K09`.

### A2. Meterkastkosten staan in het rapport, maar ontbreken in de uitkomsten

**Locatie:** [GebouwOptieT.dms (r. 121–124 en 146–147)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/TussenResultaten/GebouwOptieT.dms#L121-L147), [AllocatieResultatenComponent.dms (r. 116–122)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AllocatieResultatenComponent.dms#L116-L122) en [PlanregioResults.dms (K09, r. 1117–1131)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1117-L1131).

Volgens het concept-rapport (§4.12.3, Tabel 42) en het Achtergrondrapport ASA2026 (Tabel 54, p. 111) maken de kosten voor het aanpassen van de meterkast onderdeel uit van de strategieën. In de code wordt `Ki_Meterkast` (€ 1.200 / € 850 met warmtepomp, € 400 / € 300 zonder) echter alleen opgeteld in `Ki_totaal` en `Ki30_totaal`. Die twee grootheden worden sinds januari 2020 (commit `37c7e3b`) nergens meer uitgelezen. De allocatie neemt wel `Ki30_LO`, `Ki20_LO`, `Ki15_LO`, `Ki_GV`, `Ki_LTAS` en `Ki_pm` over, maar niet `Ki_Meterkast`.

De git-historie bevestigt dit beeld: `Ki_Meterkast` is toegevoegd in commits `8a2d52b` (14 okt 2024) en `93f492d` (19 nov 2024), waarin alleen `GebouwOptieT.dms` en `Classifications.dms` zijn gewijzigd. Bij de proceskosten (`120ede0`, 20 jan 2025) zijn wél ook `AllocatieResultatenComponent.dms`, `Maatschappelijk.dms` en `PlanregioResults.dms` aangepast.

Over 30 jaar afgeschreven (2,25%) ontbreekt per woning per jaar ca. € 40–55 bij warmtepompopties (`S1`, `S4` en warmtepompen in `S3`) en ca. € 15–20 bij opties zonder (`S2`). Strategieën met een warmtepomp zijn daarmee circa € 25–37 per woning per jaar te goedkoop ten opzichte van `S2`.

**Oplossing:** Neem `Ki_Meterkast` mee in `Ki30_LO` of als afzonderlijke post in de allocatieresultaten en `K09` (zoals bij `Ki_pm`).

### A3. Koudelevering in WKO-varianten als kosten geteld in plaats van als baat

**Locatie:** [PlanregioResults.dms (K13, r. 1176)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1176), met opbouw in [WKOWoningData.dms (r. 186–199)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOWoningData.dms#L186-L199) en [WKOUtilData.dms (r. 175–184)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOUtilData.dms#L175-L184).

Kostenpost `K13` (inkoop elektriciteit) bevat deze regel met een plusteken:

```dms
+ R_AardG * StateNaAllocatie/totaal/jaarlijks/ge/Om_ge_comfort //jaarlijkse comfortwinst nieuwe warmtenetten
```

`Om_Comfort` is in de code gedefinieerd als *"jaarlijkse maatschappelijke opbrengsten in de vorm van comfort door koudelevering"* (geleverde koude $\times$ eindgebruikersprijs elektriciteit gedeeld door 1,00 voor woningen en 1,56 voor utiliteit). Een maatschappelijke opbrengst wordt hier dus bij een kostenpost opgeteld. In de Startanalyse 2019 stond nog een minteken; sinds een herindeling in februari 2020 staat er een plus.

Uit een regressie over 1.695 gepubliceerde buurten ($\ge 50$ woningen, geen bestaand net, $< 3\%$ utiliteit) blijkt dat `K13` per GJ koude stijgt met € 61 (`s3b`) en € 57 (`s3g`) — vrijwel gelijk aan de kleinverbruiksprijs voor elektriciteit in 2030 — en € 38–42 bij `s3c`, `s3d` en `s3h`. Bij varianten zonder koude (`s3a`, `s3e`) is het effect vrijwel nul. WKO-varianten met koeling worden hierdoor € 110–225 per aangesloten woning per jaar te duur gerekend. Bovendien stelt het informatieblad *Koudevraag in de gebouwde omgeving* (PBL, maart 2025, publicatie 5840) dat koudelevering buiten de nationale kosten valt.

**Oplossing:** Verwijder de term uit `K13` in lijn met het informatieblad, of neem haar met een minteken op als comfortbaat (mits koeling bij andere strategieën, zoals lucht-waterwarmtepompen, op gelijke wijze wordt gewaardeerd).

### A4. Geen transportkosten voor warmtenet zodra er een bestaand net in de buurt ligt (`K06`)

**Locatie:** [PlanregioResults.dms (K06, r. 1085–1095)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1085-L1095).

Zodra `R_StadV > 0` zet het model de transportkosten (`K06`) voor de hele buurt op `0`, ongeacht hoe klein het bestaande warmtenet is. Het model veronderstelt daarmee dat de bestaande hoofdtransportleiding zonder extra kosten alle nieuwe aansluitingen in de buurt kan accommoderen.

In buurten met een bestaand warmtenet (558 buurten; 0,83 miljoen woningen) wordt een warmtenetstrategie hierdoor mogelijk aanzienlijk te goedkoop gerekend. In de praktijk is de vrije capaciteit van bestaande hoofdtransportleidingen meestal beperkt en moeten bij uitbreiding wel degelijk transportkosten worden gemaakt. Ter vergelijking: in buurten zonder bestaand net bedragen de transportkosten bij `S2d` in de mediaan ca. € 130 per woningequivalent per jaar, en in de regio Leiden ca. € 220.

**Oplossing:** Reken transportkosten naar rato van de uitbreiding van het aantal aansluitingen en de beschikbare restcapaciteit, door de factor R_AardG toe te passen op `K06`.

---

## B. Kleinere issues

1. **B1. Terugvaloptie `LokalePraktijkFactor` pakt het eerste woningtype in de buurt**  
   *Locatie:* [BebouwingsComponentT.dms (r. 262–270)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/TussenResultaten/BebouwingsComponentT.dms#L262-L270).  
   De terugvaloptie zoekt achtereenvolgens op `buurt+woningtype`, buurt, wijk en gemeente, maar het invoerbestand bevat alleen regels per `buurt+woningtype`. Een `rjoin` op buurtcode pakt daardoor de factor van het eerste woningtype in die buurt in plaats van een buurtgemiddelde. Een ontbrekend woningtype krijgt zo de factor van een ander type.

2. **B2. Onverklaarde opslag op alle elektriciteit per buurt via landelijk aandeel (`Kj_OpslagWP`)**  
   *Locatie:* [PlanregioResults.dms (r. 1008–1018 en r. 1180)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L1008-L1018).  
   Via `Kj_OpslagWP` komt op *al* het elektriciteitsgebruik in de buurt — dus ook op het gewone huishoudelijke verbruik — een opslag van € 0,005 per kWh maal het landelijke aandeel aansluitingen met een warmtepomp in die specifieke run (`AandeelWP_NL`), opgeteld bij `K13`. Doordat wordt vermenigvuldigd met het landelijke warmtepompaandeel in de gekozen run, krijgt een run met veel warmtepompen (`S1`) een hogere opslag dan een run zonder (`S2`), wat `S1` ca. € 15–20 per woning per jaar duurder maakt. De post wordt niet genoemd in het achtergrondrapport (mogelijk is het een ongedocumenteerde correctie voor hogere stroomprijzen in de winter).

3. **B3. Koudeverkoop telt mee bij de selectie van woningen voor een 70 °C-WKO-net (`S3c`, `S3h`)**  
   *Locatie:* [WKO.dms (r. 30)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKO.dms#L30), [WKOWoningData.dms (r. 31–33, 185–199)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOWoningData.dms#L185-L199), [WKOData.dms (r. 133, 146–150)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOData.dms#L133-L150) en [WKOIter.dms (r. 82, 170)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOIter.dms#L82).  
   Volgens Tabel 1 van het informatieblad *Koudevraag* is koeling bij `S3c` en `S3h` niet mogelijk. Bij warmtelevering op 70 °C is dat natuurlijk ook logisch. Beide varianten draaien toch met `KoudeBeschikbaar := true`, waardoor opbrengsten uit koudeverkoop (`Oj_Koude`, `Oj_VastrechtK`) de aansluitmarge verhogen en het secundaire net 20% extra investering krijgt (`1.2 * secundairnet/Ki_wd`). Daardoor kunnen zowel de samenstelling van het WKO-cluster als de nationale kosten wijzigen.

4. **B4. Dubbele correctie in de berekening van de waarde van klimaatneutraal gas**  
   *Locatie:* [Combineer.dms (r. 821–829)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/CombineerRuns/Combineer.dms#L821-L829) en [PlanregioResults.dms (VraagKG, r. 46–53)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/LeidraadResultaten/PlanregioResults.dms#L46-L53).  
   In `Besparing := (1d - checks/R_StadV) * (BesteGasloosVariantH16 - BesteKGvariantH16)` zijn `H16` (op `A1` na) en de noemer `VraagKG` al met `R_AardG` gecorrigeerd. De teller krijgt die factor dus dubbel, waardoor de waarde van klimaatneutraal gas in buurten met een bestaand warmtenet te laag uitvalt, en deze buurten soms onterecht geen klimaatneutraal gas toegeweze krijgen (in ASA2026 alleen gebruikt in de gevoeligheidsanalyses van 0,5 en 1 bcm).

5. **B5. Kopieerfouten in beheer en onderhoud van de collectieve warmtepomp (`S3`)**  
   *Locatie:* [WKOData.dms (r. 84, 118–129)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/WKOData.dms#L118-L129); gelijk in [LtData.dms (r. 100–107)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/LtData.dms#L100-L107) en [TEOData.dms (r. 145–154)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/AanbodOpties/TEOData.dms#L145-L154).  
   In `Kj_wd_adm` staat `Kj_wd_oh_ewp` (onderhoud, 3,5%) in plaats van `Kj_wd_adm_ewp` (administratie, 2,5%). Bovendien bevat `Ki_wd` de warmtepomp al (`Ki_wd_gebouw + Ki_wd_eWP`), waardoor ook 1% leidingonderhoud over de warmtepomp wordt gerekend. De varianten `s3c`, `s3d`, `s3f` en `s3h` zijn daardoor 1 procentpunt per jaar (en mogelijk 2 procentpunt per jaar) van de warmtepompinvestering ($\times$ `Leercurves/OenM`) te duur.

6. **B6. Verkeerde factor bij label D in de gevoeligheidsanalyse `G05`**  
   *Locatie:* [GebouwOptieT.dms (r. 165–171)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/TussenResultaten/GebouwOptieT.dms#L165-L171).  
   Zowel bij doellabel B (r. 167) als bij doellabel D (r. 169) gebruikt de code `GA05_factor_B` in plaats van `GA05_factor_D`. De warmtevraag voor D-varianten in gevoeligheidsanalyse `G05` is daardoor circa 25–30% te laag (`G05` te gunstig); op de hoofduitkomsten heeft dit geen effect.

7. **B7. Eenhedenfout in elektrische pompenergie voor industriële restwarmte**  
   *Locatie:* [WarmteBronnen.dms (container Industrie, r. 192–200)](https://github.com/pbl-nl/model-vesta-public/blob/0f7a3e4462dbb9ec33d04ffe73a76572ef9c555d/model/stam/CalculationSchemes/Kengetallen/WarmteBronnen.dms#L192-L200).  
   De code rekent met `0.0000025 [GJ / GJ]`, terwijl in de originele bron, het *Functioneel Ontwerp Vesta MAIS 3.0* wordt uitgegaan van 2,5 kWh pompenergie per GJ warmte (2,5*0,0036 = 0,009 GJ). De rekenwaarde in de code is daarmee een factor 3.600 te laag, waardoor de variabele pompenergiekosten van restwarmte vrijwel op nul uitkomen (maakt restwarmte enkele tienden van een euro per GJ te goedkoop).

## Bronvermelding

Bij hergebruik of verwerking in modelupdates en rapportages graag verwijzen naar:  
Jeroen Bussmann / Regionale Expertgroep Duurzaam Energiesysteem (REDE) (2026). *Codebeoordeling Vesta MAIS – Actualisatie Startanalyse aardgasvrije buurten 2026 (commit `0f7a3e4`)*. Eerste versie: 24 november 2026.
