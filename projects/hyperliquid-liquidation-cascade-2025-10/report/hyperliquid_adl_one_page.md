---
geometry: margin=0.34in
fontsize: 8pt
header-includes:
  - \usepackage{titlesec}
  - \titlespacing*{\section}{0pt}{5pt}{2pt}
  - \titlespacing*{\subsection}{0pt}{4pt}{1pt}
  - \setlength{\parskip}{2pt}
  - \setlength{\parindent}{0pt}
---

# Hyperliquid ADL Marked Liquidity Collapse, Not Just Volume

**Quant research brief | Main cascade: about 8 minutes inside 2025-10-10 21:00-22:00 UTC**

## Executive Summary

**Bottom line.** Hyperliquid ADL behaved like forced risk transfer during a short liquidation cascade, not like a normal high-volume trading period.

**Evidence.** I traced raw S3 `node_fills`, `misc_events`, and `l2_book`: 35,022 cleaned ADL rows matched liquidation-side fills, ADL notional was $2.10B, and two HLP Liquidator addresses traded about $4.35B notional in the same event study.

**Why it matters.** The strongest signal was liquidity collapse: at ADL peaks, 50 bps bid-side depth was pinned near zero through p70, while normal fill peaks recovered to 1.38x baseline by p70. For a trader, this changes quoting, hedging, and exposure decisions.

![50 bps bid-depth decile comparison](assets/hyperliquid_adl_evidence_chart.png)

## Three Findings Worth Discussing

**1. The ADL flow mapped to backstop liquidation, not generic metadata.** Each ADL row matched a liquidation-side fill; the top notional ADL cases were tagged `backstop`. Roughly nine out of ten cleaned ADL events pointed to two HLP Liquidator labels, and the unwind looked position-level rather than account-level: large coins appeared first, then the tail followed.

**2. Coin concentration alone is not the story.** ADL top-five coin share was 76.6%, close to all-fill top-five share at 77.6%, so that number is not distinctive by itself. The better finding is composition shift: ADL versus fill coin-share L1 distance was 0.1156, meaning ADL was not just a copy of normal volume.

**3. The order book test is the cleanest trading evidence.** At exact ADL peaks, median spread was 5.92x hourly baseline; in the +/-1 minute ADL window, max-spread reached 10.11x. Median 50 bps bid-depth ratio was 0. This directly supports the idea that ADL marks a worse execution regime.

## Trading Implication

**I would monitor ADL as a live stress-state input.** The useful trigger is not ADL alone, but ADL/backstop liquidation clustering plus spread widening, bid-depth collapse, and coin-composition shift. In a live trading stack, that can drive smaller or paused quotes, tighter inventory limits, faster risk reduction, or lower venue exposure until depth comes back.

## Caveat And Next Test

This is a one-hour event study, with the main cascade inside that hour. It proves a reproducible stress-regime hypothesis, not a universal model. The next test is to replay more stress windows and measure liquidity recovery time after ADL-led depth collapse: how long it takes for 50 bps depth and spreads to return near baseline.
