# Significant, not tradable: a Hawkes forecast of the Bitcoin perpetual–spot basis against the spread and the fees

A four-dimensional Hawkes process fitted to the price moves of the Binance BTC perpetual and spot markets gives a closed-form forecast of their basis a few seconds ahead. This note tests it as a trading signal, out of sample, at the mid, at the touch, with latency and with fees.

**Paper:** [paper/btc-basis-signal-vs-fees.pdf](paper/btc-basis-signal-vs-fees.pdf) (7 pages).
A plain-language explanation in Italian is in [paper/btc-basis-signal-vs-fees-spiegazione.pdf](paper/btc-basis-signal-vs-fees-spiegazione.pdf).

## What it finds

- **The forecast works.** Fitted on one four-hour window and applied to the next, it earns 0.4 to 1.3 bp per round trip at the mid and 0.1 to 0.7 bp at the touch (0.44 bp on average over ten BTC days). It is positive on all twelve trading days and stable for latencies up to about ten milliseconds.
- **Naive lead–lag does not.** A naive lead–lag rule looks better at the mid and is destroyed at the touch by adverse selection.
- **The touch costs more than the average spread.** At the instant of a decision the spread of the leg that has just moved is two to three times its average.
- **Nothing covers the fees.** A round trip on both legs costs from 2.2 bp (top-tier maker) to 30 bp (regular taker). A conservative bound on maker execution shows that posting passively loses too: the round trips filled as a maker on both legs are a small minority, and the rest are filled on one leg, on the wrong side of a common move.

Mean profit per round trip at the touch, latency 0, ten BTC days (bp):

| Signal | gross | net of 2.2 bp | net of 8 bp | net of 30 bp |
|---|---|---|---|---|
| Hawkes, h = 10 s | +0.44 | −1.76 | −7.56 | −29.56 |
| OU, h = 3 s | +0.57 | −1.63 | −7.43 | −29.43 |
| Naive lead–lag | −0.85 | −3.05 | −8.85 | −30.85 |

## Data and sources

Twelve trading days between December 2022 and September 2026 (ten BTC, two ETH), best quotes at microsecond resolution from Tardis.dev. The quotes are licensed and are not in this repository. The estimation and backtest code is part of the author's master thesis and is not public yet.

`paper/src/` holds the LaTeX sources of the paper and of the Italian explanation and the CSV files behind the figures. Build with `latexmk -xelatex paper.tex` from that folder.

## Limits

Execution at the touch assumes the displayed quote is available for the full size; queue position and fail-to-fill are not modelled, and the maker case is a bound, not a model. The walk-forward runs within each day; no result is carried from one day to the next.

This is research, not a trading recommendation. Based on the author's master thesis on Hawkes models of the perpetual–spot basis.
