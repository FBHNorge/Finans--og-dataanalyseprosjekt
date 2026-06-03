# Hva driver aksjeavkastning? — Prediksjon av aksjeavkastning på Oslo Børs med maskinlæring

Et selvstendig dataanalyseprosjekt som analyserer og predikerer månedlig avkastning for seks
selskaper på Oslo Børs ved hjelp av Python, statistisk modellering og maskinlæring.

**Forfatter:** FB · BSc Markedsføringsledelse & Økonomi (BI) → MSc Business Analytics (NHH, 2026)
**Periode:** Juni – august 2026

---

## Oversikt

Prosjektet besvarer to spørsmål:

1. **Analyse — hva har drevet avkastningen historisk?** Korrelasjonsanalyse og OLS-regresjon
   brukes til å identifisere hvilke makrofaktorer som er statistisk signifikante for hvert selskap.
2. **Prediksjon — kan fremtidig avkastning forutsies?** Laggede, lekkasjefrie features brukes til
   å trene og sammenligne tre maskinlæringsmodeller på usett testdata.

**Selskaper (Oslo Børs):** Equinor (EQNR), DNB, Aker BP (AKRBP), Mowi (MOWI),
Norsk Hydro (NHY), Telenor (TEL).

**Forklaringsvariabler:** Brent råoljepris (BZ=F), USD/NOK valutakurs (USDNOK=X) og
VIX-volatilitetsindeksen ("fryktindeksen", ^VIX), samt konstruerte features (laggede verdier,
3-måneders glidende gjennomsnitt og 3-måneders rullende volatilitet).

**Data:** Månedlig, 2015–2024 (119 observasjoner), hentet fra Yahoo Finance via `yfinance`.

---

## Hovedfunn

**Analyse (OLS-regresjon)**

- **Markedsfrykt (VIX) er den mest gjennomgående driveren** — statistisk signifikant og negativ
  for 5 av 6 selskaper. Generell markedsstemning beveger hele børsen.
- **Oljeprisen er signifikant kun for Equinor.** For Aker BP — også et oljeselskap — dominerer
  valutakursen i stedet, fordi olje og valuta overlapper sterkt (multikollinearitet).
- **Telenor er upåvirket av alle makrofaktorer** (R² = 0,04) og oppfører seg som en defensiv aksje.
- **USD/NOK mister forklaringskraft når VIX inkluderes**, noe som avdekker en felles
  "risk-off"-kanal snarere enn en selvstendig valutaeffekt.

**Prediksjon (usett testdata, kronologisk 80/20-splitt)**

| Modell | RMSE | R² (test) |
|---|---|---|
| **Lineær regresjon** | **0,0659** | **+0,069** |
| Random Forest | 0,0707 | −0,073 |
| XGBoost | 0,0742 | −0,182 |

- **Den enkleste modellen vinner.** Lineær regresjon er den eneste med positiv R² på testdata.
  De mer komplekse modellene overtilpasser den støyete og datafattige tidsserien — en sentral
  lærdom om at modellkompleksitet må tilpasses datagrunnlaget.
- Modellen fanger **retningen** i avkastningen svakt, men aldri **størrelsen** på de store
  utslagene — i tråd med teorien om at månedlig aksjeavkastning i stor grad er uforutsigbar.

---

## Metode

1. **Datainnhenting** — månedlige priser hentet fra Yahoo Finance med `yfinance`.
2. **Rensing & feature engineering** — beregnet månedlig avkastning (`pct_change`); konstruerte
   laggede verdier, glidende gjennomsnitt og rullende volatilitet, alle forskjøvet for å unngå
   look-ahead bias.
3. **Eksplorativ dataanalyse** — korrelasjons-heatmap og distribusjonsanalyse.
4. **Statistisk analyse** — OLS multippel regresjon per selskap (koeffisienter, p-verdier, R²).
5. **Maskinlæring** — lineær regresjon (baseline), Random Forest, XGBoost; sammenlignet på
   RMSE, MAE og R² med kronologisk train/test-splitt; analyse av feature importance.
6. **Rapportering** — resultatene eksportert til en Excel-rapport med flere ark.

---

## Verktøy

Python · pandas · NumPy · statsmodels · scikit-learn · XGBoost · matplotlib · seaborn ·
Jupyter Notebook · openpyxl

---

## Mappestruktur

```
stock_project/
├── stock_analysis.ipynb        # Hovednotebook (analyse + ML)
├── data/
│   ├── raw_prices.csv          # Rådata fra Yahoo Finance
│   └── monthly_returns.csv     # Renset månedlig avkastning
├── stock_returns_report.xlsx   # Excel-rapport med funn
└── README.md
```

## Slik kjører du prosjektet

```bash
pip install yfinance pandas numpy matplotlib seaborn statsmodels scikit-learn xgboost openpyxl
jupyter lab
```

Åpne `stock_analysis.ipynb` og kjør alle cellene fra topp til bunn.

---

*Prosjektet er bygget for å lære data analytics og maskinlæring praktisk, med vekt på å forstå
hvorfor hvert steg er viktig, ikke bare å kjøre kode.*
