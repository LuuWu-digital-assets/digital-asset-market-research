# digital-asset-market-research

Research notes and reproducible analyses on crypto derivatives market microstructure, liquidity, liquidations, and risk. The current portfolio project studies a one-hour Hyperliquid liquidation and ADL stress event using public raw-data traces, cleaned evidence tables, notebooks, and a short research brief.

## Featured Study

### Hyperliquid Liquidation Cascade and ADL Stress Study

**Bottom line.** Hyperliquid ADL behaved like forced risk transfer during a short liquidation cascade, not like a normal high-volume trading period.

![50 bps bid-depth decile comparison](projects/hyperliquid-liquidation-cascade-2025-10/report/assets/hyperliquid_adl_evidence_chart.png)

## Key Findings

- **ADL matched liquidation-side fills.** The cleaned event table contains 35,022 ADL rows, and every row matched a liquidation-side fill on the exact trade key in this one-hour sample.
- **The clearest signal was liquidity collapse.** At exact ADL peaks, median 50 bps bid-side depth was zero; by comparison, normal fill peaks recovered to 1.38x the hourly baseline by p70.
- **The event was not just generic volume concentration.** ADL top-five coin notional share was 76.6%, close to all-fill top-five share at 77.6%, so the stronger evidence is the execution-regime shift: spread widening, bid-depth collapse, and composition change.
- **Mechanism claims are bounded.** The analysis verifies fill-level pairing and market-state stress, but does not claim to prove ADL queue/ranking, bankruptcy-price logic, full account state, or final loss-bearer accounting.

## Artifacts

- [One-page research brief](projects/hyperliquid-liquidation-cascade-2025-10/report/hyperliquid_adl_one_page.md)
- [Core notebooks](projects/hyperliquid-liquidation-cascade-2025-10/notebooks/)
- [Evidence tables](projects/hyperliquid-liquidation-cascade-2025-10/evidence/)
- [Dataset configuration and field notes](projects/hyperliquid-liquidation-cascade-2025-10/reproducibility/)

## Projects

- [Hyperliquid Liquidation Cascade and ADL Stress Study](projects/hyperliquid-liquidation-cascade-2025-10/)
