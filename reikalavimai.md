# Kursinis darbas: 3 dB Wilkinson galios daliklis 2,4 GHz (ADS)

Šis failas skirtas Claude (ir man) greitai prisiminti užduotį, reikalavimus ir sprendimo būseną naujame pokalbyje.

## 1. Užduotis (standartinė)

3 dB Wilkinson galios daliklis 2,4 GHz dažniui:

1. Centrinis dažnis: **2,4 GHz**
2. S(2,1) ir S(3,1) **-6 dB** pralaidos juostos ribos: ne siauriau nei **2,35-2,45 GHz**
3. S(1,1), S(2,2) ir S(3,3) pralaidos juostoje: **ne daugiau nei -6 dB**
4. Elementų bazė: **ilgosios linijos mikrojuostelės technologija** + **Murata diskretinis varžinis elementas**
5. Padėklo medžiaga: **FR-4**

Pasirinkta užduotis: standartinė (ne alternatyvi). Projektavimo aplinka: **Keysight ADS**; PCB realizacija: **Altium Designer**.

## 2. Reikalaujamos ataskaitos (KD) dalys

1. **Titulinis lapas**: kursinio darbo pavadinimas, studento pavardė ir grupė. Jei dirbama grupelėje: visų narių pavardės ir akademinės grupės.
2. **Užduoties formulavimas**
   - a) standartinė užduotis: aiškiai apibrėžti pasirinktus parametrus
   - b) alternatyvi užduotis: perkopijuoti jos turinį (mūsų atveju netaikoma)
   - jei grupelė: prie užduoties punktų nurodyti, kuris narys už ką atsakingas
3. **Idealaus filtro/mazgo sintezė**: skaičiavimai, iliustracijos, trumpi komentarai
4. **Geometrijos skaičiavimai**, pirminis geometrijos rezultatas ir S parametrai
5. **Optimizavimo uždavinys**: formulavimo komentarai ir iliustracijos. Standartinei užduočiai: pirminės geometrijos (ilgosios linijos / diskretiniai komponentai) optimizavimas **ADS** aplinkoje
6. **Geometrijos realizavimas Altium Designer** aplinkoje (PCB projektas)
7. **Galutinės išvados**, komentarai ir techniniai pasiūlymai

## 3. Jau atlikta (preliminari sintezė, pokalbio pradžia)

- Topologija: vienasekcis Wilkinson daliklis (juosta ≈4 %, pakanka vienos λ/4 sekcijos)
- Z0 = 50 Ω; atšakos Z0·√2 = **70,7 Ω**; izoliacinė varža **100 Ω**
- Padėklas (prielaidos): FR-4, εr ≈ 4,4, tgδ ≈ 0,02, h = 1,6 mm, Cu 35 µm
- Preliminarūs matmenys (h = 1,6 mm): 50 Ω W ≈ 3,06 mm; 70,7 Ω W ≈ 1,62 mm; λg/4 ≈ 17,5 mm (atšakos)
- Rezistorius: 100 Ω, 1 %, 0603 (arba 0402) Murata; tikslų kodą ir S parametrų modelį patikrinti Murata kataloge
- Tikėtini rezultatai: S21/S31 ≈ -3,3…-3,8 dB; S11/S22/S33 < -20 dB (idealiai); izoliacija S23 > 20 dB

Pastaba: matmenys yra pirminiai skaičiavimai, juos reikia patikrinti ADS (LineCalc / schematic) ir EM simuliacijoje.

## 4. Darbo planas

1. Sukurti ataskaitos karkasą pagal 2 skyrių (titulinis, 1-7 dalys)
2. Sintezė: formulės, lentelės, ideali schema ADS (TLIN + R)
3. Geometrija: LineCalc, MLIN / MTEE / MCURVE, pirminiai S parametrai
4. Optimizavimas ADS (Optim / Goals: S11, S22, S33 ≤ -6 dB, S21, S31 ≥ -6 dB juostoje 2,35-2,45 GHz; kintamieji: atšakų ilgis, plotis, atstumas)
5. Altium Designer PCB: išdėstymas, via sienutė, Murata aikštelės, SMA jungtys
6. Išvados ir pasiūlymai

## 5. Atviri klausimai (reikia iš vartotojo)

- Studento vardas, pavardė, grupė (titulinis lapas)
- Ar dirbama grupelėje (jei taip, narių vardai ir atsakomybių pasidalijimas)
- FR-4 storis (h) ir tikslus εr / tgδ, jei žinomi iš gamintojo ar dėstytojo
- Konkretus Murata rezistoriaus kodas (jei nurodytas dėstytojo)
- Ataskaitos formatas (Word / PDF / LaTeX) ir kalba (lietuvių)

## 6. Darbo taisyklės

- Kalba: lietuvių
- Prieš keisdamas ataskaitą, perskaityti šį failą ir atnaujinti 3 ir 4 skyrius
- Visi skaičiavimai turi būti patikrinti (formulės, skaičiavimas kodu)

## 7. Antro varianto (lenktų pečių) geometrija ir simuliacija (2026-11-09)

Būsena: geometrija suskaičiuota ir patikrinta ADS grandinės simuliatoriumi (`hpeesofsim`, paleistas iš komandinės eilutės su atskiru netlistu). **Į ADS schemą (`GalioDaliklis`) reikšmės dar neįvestos**, layout ir Momentum (EM) nepatikrinti.

### 7.1. Kas rasta esamoje schemoje

- **Sukeisti `MTEE_ADS` pločiai (Tee1 ir Tee2).** Įėjimo linija jungiasi prie pin 3, pečiai prie pin 1 ir 2, todėl turi būti `W1=1.588, W2=1.588, W3=3.045 mm`. Schemoje buvo `W1=3.045, W2=1.588, W3=1.588`.
- **Antro varianto lankai neužsidaro.** Su `Angle=150`, `Radius=6.7 mm` pečių galai lieka ≈9,7 mm vienas nuo kito (rezistorius nepasiekia), o išėjimai layout'e eitų ±60° kryptimis ir kryžiuotųsi.
- **Petys per ilgas.** Pataisius trišakio pločius, su 150° / 6,7 mm lankais S11 minimumas nusislenka į 2,20 GHz, S23 į 2,25 GHz.
- **Nesutampa `TanD`.** Schemos `MSub1` turi `TanD=0.0027`, LineCalc ir 3 skyrius: 0,02. FR-4 atveju naudoti 0,02.

### 7.2. Nauja geometrija

Padėklas: H = 1,6 mm, εr = 4,4, T = 35 µm, TanD = 0,02. Kiekvienas petys sudarytas iš dviejų lankų (bendras ilgis 16,15 mm) ir baigiasi lygiagrečiai ašiai, 4,0 mm tarp pečių ašių. Vienas ~180° lankas netinka, nes pečių galai atsisuka vienas į kitą ir išėjimų nebeįmanoma išvesti lanku.

Viršutinis petys iš eilės (apatinis toks pat, veidrodinis):

| Elementas | Parametrai |
|---|---|
| `MLIN` įėjimas | W=3.045 mm, L=10 mm (ilgis laisvas) |
| `MTEE_ADS` | W1=1.588, W2=1.588, W3=3.045 mm |
| `MCURVE` 1 (petys) | W=1.588 mm, Angle=146.47, Radius=4.56 mm |
| `MCURVE` 2 (petys, lenkiasi į priešingą pusę) | W=1.588 mm, Angle=56.47, Radius=4.56 mm |
| `R` tarp abiejų pečių galų | 100 Ω |
| `MSTEP` | W1=1.588, W2=3.045 mm |
| `MCURVE` 3 (išėjimas, į išorę) | W=3.045 mm, Angle=60, Radius=6 mm |
| `MCURVE` 4 (išėjimas, atgal į horizontalę) | W=3.045 mm, Angle=60, Radius=6 mm |
| `MLIN` išėjimas | W=3.045 mm, L=5 mm (ilgis laisvas) |

Apskaičiuoti layout matmenys (ADS layout'e dar nepamatuoti):

- pečio galas yra ≈12,16 mm nuo trišakio ašies;
- tarp 50 Ω linijų vidinių kraštų iškart po `MSTEP` lieka ≈0,95 mm, 0603 rezistorius lituojamas tiesiai tarp jų;
- išėjimo portai išsiskiria per 16 mm, visa struktūra ≈38 × 20 mm.

Formulės geometrijai perskaičiuoti (γ = antro lanko kampas, L = pečio ilgis, s = atstumas tarp pečių ašių gale):

- R = L / ((90° + 2γ)·π/180)
- s/2 = W50/2 + R·(2·cos γ − 1)
- pečio galo x = R·(1 + 2·sin γ)

### 7.3. Simuliacijos rezultatai (grandinės modelis, TanD = 0,02)

| Dažnis | S11 | S22 = S33 | S21 = S31 | S23 |
|---|---|---|---|---|
| 2,35 GHz | −40,3 dB | −54,2 dB | −3,34 dB | −34,4 dB |
| 2,40 GHz | −39,7 dB | −55,3 dB | −3,35 dB | −38,5 dB |
| 2,45 GHz | −34,3 dB | −56,8 dB | −3,35 dB | −38,2 dB |

- S11, S22, S33 ≤ −20 dB nuo 1,94 iki 2,83 GHz; S21 ir S31 ≥ −6 dB visame tikrintame 1–4 GHz ruože. Užduoties reikalavimai (1 skyrius) tenkinami.
- Su TanD = 0,0027: S21 = S31 ≈ −3,07 dB.
- Pečio ilgio jautrumas (s = 4,0 mm): 0,5 mm ilgio pokytis paslenka S11 ir S23 minimumus ≈70 MHz (L = 16,0 mm → S11 min 2,395 GHz, S23 min 2,445 GHz; L = 16,5 mm → 2,325 ir 2,375 GHz).

Palyginimui, esamas antras variantas (150° / 6,7 mm, TanD = 0,02) ties 2,40 GHz:

| Būsena | S11 | S21 | S23 | S11 min ties |
|---|---|---|---|---|
| kaip nubraižyta (sukeisti trišakio pločiai) | −31,3 dB | −3,40 dB | −34,7 dB | 2,28 GHz |
| pataisius trišakio pločius | −26,1 dB | −3,42 dB | −28,8 dB | 2,20 GHz |

### 7.4. Liko padaryti

1. Įvesti 7.2 lentelės reikšmes į ADS schemą, pataisyti `MTEE_ADS` pločius ir `TanD`.
2. Sugeneruoti layout ir patikrinti lankų kryptis; jei `MCURVE` lenkiasi ne į tą pusę, keisti `Angle` ženklą arba apversti komponentą (Mirror).
3. Pamatuoti layout'e 4,0 mm tarpą. Jis apskaičiuotas darant prielaidą, kad trišakio pin 1 ir pin 2 yra per 3,045 mm; jei skiriasi, paderinti antro lanko kampą.
4. Momentum (EM) simuliacija: grandinės modelis nevertina ryšio tarp gretimų linijų ir rezistoriaus aikštelių.
5. Pečio ilgį (lankų `Radius`) padaryti optimizavimo kintamuoju ataskaitos 5 daliai.
