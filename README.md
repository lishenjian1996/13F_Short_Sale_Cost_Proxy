# Borrow Frictions in Plain Sight: Short-Sale Costs Without Borrow Fees

Official Python replication repository for:

> **Li, Shenjian (2026).** *Borrow Frictions in Plain Sight: Short-Sale Costs Without Borrow Fees.* Working Paper, Available on [SSRN](https://papers.ssrn.com/sol3/cf_dev/AbsByAuth.cfm?per_id=6486180).

This toolkit constructs the public short-sale cost proxy (`SIR_IO`) formulated by **Muravyev, Pearson, and Pollet (2025, *Journal of Finance*)** using public SEC Form 13F filings, Compustat Short Interest, and CRSP monthly stock data. The underlying 13F parsing methodology builds on **Li (2025, SSRN No. 5101829)**.

---

## Execution Order

Run the Jupyter Notebooks in sequential order:

1. **`01_sec_13f_institutional_ownership.ipynb`**  
   Downloads quarterly SEC Form 13F bulk filings (2013Q2–present) from EDGAR, parses common share holdings, handles the 2023Q1 dollar unit shift, deduplicates amendment filings, and produces `data/processed/sec_13f_quarterly_io.csv.gz`.

2. **`02_wrds_short_interest_and_crsp.ipynb`**  
   Standardizes Compustat monthly short interest, bridges the S&P feed migration (`sec_shortint_legacy` to `sec_shortint`), links security identifiers via CCM, and extracts CRSP monthly stock files.

3. **`03_construct_short_sale_cost_proxy.ipynb`**  
   Merges quarterly institutional ownership and mid-month short interest onto CRSP stock-months, constructs split-invariant `SIR_IO`, generates the `SIR_IO > 18%` exclusion indicator, and outputs the final research panel to `data/processed/mpp2025_short_sale_cost_proxy.csv.gz`.

---

## Output Dataset

The standardized research panel (`data/processed/mpp2025_short_sale_cost_proxy.csv.gz`) covers U.S. common equities (P >= $1.0, 2013–2025, N = 531,098). Key variables include:

- `permno`, `month_end`, `cusip8`, `prc_abs`, `ret`, `mktcap_millions`: CRSP market identifiers and returns.
- `sir`: Monthly short interest ratio (`shortint / shares_out`).
- `io_prev_qtr`: Lagged quarterly institutional ownership fraction from SEC Form 13F.
- `sir_io`: Muravyev, Pearson, and Pollet (2025) borrow-cost proxy (`sir / io_prev_qtr`).
- `high_short_cost_18pct`: Primary exclusion indicator (`1` if `sir_io > 0.18`, `0` otherwise).
- `mktcap_ge_50m`: Flag for market equity >= $50M (N = 479,765).

---

## Citation

```bibtex
@article{Li2026_BorrowFrictions,
  title={Borrow Frictions in Plain Sight: Short-Sale Costs Without Borrow Fees},
  author={Li, Shenjian},
  journal={SSRN Working Paper},
  year={2026}
}
```

---

## License

This project is licensed under the [MIT License](LICENSE).
