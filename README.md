# InvoiceTariff — Free US Tariff Calculator & HTS Code Lookup

https://invoicetariff.com

![ruleset](https://img.shields.io/badge/ruleset-0.3.2-blue) ![data as of](https://img.shields.io/badge/as_of-2026--09--24-informational) ![license](https://img.shields.io/badge/license-MIT-green) ![no sign-up](https://img.shields.io/badge/sign--up-none-success)

Free, browser-based United States import tariff toolkit for cross-border sellers, e-commerce operators, and small importers. Enter a declared value and an HTS code, get the fully stacked duty estimate — MFN, Section 301, Section 232, de minimis, MPF/HMF fees — in seconds, then generate a compliant commercial invoice in one click. Every rate in the result is traceable to its source file and legal basis.

## 🌐 Live Website

**https://invoicetariff.com** — free to use, no account required, everything runs in your browser.

## Features

| Tool | What it does | Link |
|---|---|---|
| Tariff Calculator | Declared value + HTS code → stacked US duty estimate (MFN + 301 + 232 + fees) | [tariff-calculator](https://invoicetariff.com/tariff-calculator) |
| Landed Cost Calculator | Duty + MPF/HMF + freight/VAT reference → per-unit landed cost | [landed-cost-calculator](https://invoicetariff.com/landed-cost-calculator) |
| HTS Code Lookup | Search 5,600+ HTS 8-digit codes by code, keyword, or Chinese keyword | [hts](https://invoicetariff.com/hts) |
| China Tariff Guide | Section 301 China lists (1–4A), exclusions, and the 2026 global program | [china-tariff](https://invoicetariff.com/china-tariff) |
| Section 301 Reference | Every 301 line with legal basis and Federal Register source | [section-301](https://invoicetariff.com/section-301) |
| De Minimis Rules | US de minimis threshold and how it applies to small parcels | [de-minimis](https://invoicetariff.com/de-minimis) |
| Tariff Change Radar | Changelog of published rule changes with dates and sources | [radar](https://invoicetariff.com/radar) |
| IEEPA Refund Checker | Which refund channel applies after the 2026 IEEPA ruling | [ieepa-refund](https://invoicetariff.com/ieepa-refund) |

## 📦 Open data assets

All rate logic of the calculator ships as versioned JSON in [`data/`](./data):

- [`data/ruleset-seed.json`](./data/ruleset-seed.json) — the full stacked-rate ruleset: MFN (curated 460+ codes), China Section 301 lists + exclusions, Global Section 301 program (60 country tiers), Section 232 measures, Section 338, IEEPA historical layer, and the FY2026 fee table (MPF/HMF/informal entry).
- [`data/mfn-full.json`](https://github.com/invoicetariff/hs-codes-database) — full 5,689-code HTS 8-digit table with US general duty rates (kept in the [hs-codes-database](https://github.com/invoicetariff/hs-codes-database) repo).
- [`data/destinations.json`](./data/destinations.json) — 16 destination countries: simplified VAT/GST rates and de minimis reference config for landed-cost estimates.
- [`data/changelog.json`](./data/changelog.json) — machine-readable changelog of published rule changes (41 entries), the source of the [Tariff Change Radar](https://invoicetariff.com/radar).
- [`data/refund-channels.json`](./data/refund-channels.json) — the four post-IEEPA refund channels with legal basis and CBP source.

## Data provenance & quality

- `ruleset-seed.json` **v0.3.2** (as of **2026-09-24**).
- MFN rates: USITC Harmonized Tariff Schedule (reststop export), official `general` column, 8-digit codes.
- Section 301 / 232 / IEEPA / fee lines carry a `source` object (title + URL) and `legalBasis` string on every entry.
- ⚠️ **Quality flag**: except the FY2026 fee table, rate lines in v0.3.x carry `sample`/`unverified` annotations — good for estimates and demos, not yet certified for production customs declarations. The `unverifiedNotes` and `meta.nextHardDates` fields list what is being verified next.
- The ruleset is updated monthly (see `changelog.json`); next hard dates include the FY2027 MPF change (2026-10-01) and the 2026-11-10 China exclusions review.

## Example: stacking an 8471.30 laptop from China (ruleset v0.3.2)

```text
HTS 84713000 (portable automatic data processing machines)
  MFN general          : Free            (data/ruleset-seed.json → mfn)
  Global 301, CN tier  : 12.5%           (→ global301.tiers, effective 2026-07-24)
  Section 301 China    : n/a for this code in v0.3.2
  MPF (FY2026)         : 0.3464% of value, min $33.58, max $651.50  (→ fees)
  HMF (ocean)          : 0.125%          (→ fees)
```

Run the same stack live at https://invoicetariff.com/tariff-calculator — every line in the UI result links back to the JSON entry above.

## Related repos

- [us-tariff-rates-2026](https://github.com/invoicetariff/us-tariff-rates-2026) — 2026 US tariff rate layers (301/232/IEEPA/fees) as a standalone dataset.
- [hs-codes-database](https://github.com/invoicetariff/hs-codes-database) — 5,689 HTS 8-digit codes with US general duty rates.

## License

MIT — see [LICENSE](./LICENSE). Tariff and fee rates originate from US government publications (USITC, USTR, CBP, Federal Register) and carry their own source references in the data files.
