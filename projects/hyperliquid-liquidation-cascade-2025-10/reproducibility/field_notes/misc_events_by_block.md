# misc_events_by_block

Cleaning script: `scripts/data_cleaning/clean_hyperliquid.py`

Official references:

- Historical data: https://hyperliquid.gitbook.io/hyperliquid-docs/historical-data
- L1 data schemas: https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/nodes/l1-data-schemas

## Raw Fields

| Field | Type | Source | Status | Meaning |
|---|---|---|---|---|
| `local_time` | string timestamp | S3 wrapper | `s3_header` | Time when the S3 record was locally archived or written by the source wrapper. |
| `block_time` | string timestamp | S3 wrapper | `s3_header` | Timestamp string for the block. |
| `block_number` | integer | S3 wrapper | `s3_header` | Block number. |
| `events` | array | S3 wrapper | `s3_header` | Misc event array within the current block. |
| `events[].time` | string timestamp | event payload | `official` | Event time. |
| `events[].hash` | string | event payload | `official` | Event hash. |
| `events[].inner` | object | event payload | `official` | Container for the L1 event variant. |
| `inner.LedgerUpdate` | object | `inner` | `official` | Ledger update event. |
| `inner.Funding` | object | `inner` | `official` | Funding event. |
| `inner.CDeposit` | object | `inner` | `official` | Staking deposit event. |
| `inner.CWithdrawal` | object | `inner` | `official` | Staking withdrawal event. |
| `inner.Delegation` | object | `inner` | `official` | Delegation event. |
| `inner.ValidatorRewards` | object | `inner` | `official` | Validator rewards event. |
| `LedgerUpdate.users` | array | LedgerUpdate | `official` | Users involved in the ledger update. |
| `LedgerUpdate.delta` | object | LedgerUpdate | `official` | Ledger delta payload. |
| `delta.type` | string | LedgerUpdate.delta | `official` | Delta type. |
| `delta.usdc` | string/number | LedgerUpdate.delta | `observed` | USDC quantity field; interpretation depends on `delta.type`. |
| `delta.amount` | string/number | LedgerUpdate.delta | `observed` | Token or staking quantity field; interpretation depends on `delta.type`. |
| `delta.usdcValue` | string/number | LedgerUpdate.delta | `observed` | USDC value field. |
| `delta.fee` | string/number | LedgerUpdate.delta | `observed` | Fee field. |
| `delta.token` | string | LedgerUpdate.delta | `observed` | Token symbol or token id. |
| `delta.destination` | string | LedgerUpdate.delta | `observed` | Destination address. |
| `delta.toPerp` | boolean | LedgerUpdate.delta | `observed` | Direction flag for transfer into perp. |
| `delta.accountValue` | string/number | LedgerUpdate.delta | `observed` | Account value field. |
| `delta.liquidatedNtlPos` | string/number | LedgerUpdate.delta | `observed` | Liquidated notional position field. |
| `delta.leverageType` | string | LedgerUpdate.delta | `observed` | Leverage type field. |
| `delta.liquidatedPositions` | array | LedgerUpdate.delta | `observed` | Positions involved in liquidation. |
| `Funding.deltas` | array | Funding | `official` | Funding delta list. |
| `deltas[].user` | string | Funding.deltas | `official` | User address. |
| `deltas[].coin` | string | Funding.deltas | `official` | Coin. |
| `deltas[].funding_amount` | string/number | Funding.deltas | `official` | Funding amount. |
| `deltas[].szi` | string/number | Funding.deltas | `official` | Signed position size. |
| `deltas[].funding_rate` | string/number | Funding.deltas | `official` | Funding rate. |

## Cleaned Fields

### `blocks_summary`

| Field | Type | Raw source field | Transformation | Status |
|---|---|---|---|---|
| `line_no` | integer | file line order | One-based line number. | `derived` |
| `block_number` | integer | `block_number` | Preserved as-is. | `s3_header` |
| `local_time` | string | `local_time` | Preserved as-is. | `s3_header` |
| `block_time` | string | `block_time` | Preserved as-is. | `s3_header` |
| `event_count` | integer | `events` | Number of events in the current block. | `derived` |
| `local_time_dt` | timestamp | `local_time` | Cast to timestamp. | `derived` |
| `block_time_dt` | timestamp | `block_time` | Cast to timestamp. | `derived` |
| `latency_ms` | float | `local_time`, `block_time` | Milliseconds from `(local_time_dt - block_time_dt)`. | `derived` |

### `events_flat`

| Field | Type | Raw source field | Transformation | Status |
|---|---|---|---|---|
| `event_uid` | string | `block_number`, `event_idx` | Concatenated as `<block_number>:<event_idx>`. | `derived` |
| `line_no` | integer | file line order | One-based line number. | `derived` |
| `block_number` | integer | `block_number` | Preserved as-is. | `s3_header` |
| `local_time` | string | `local_time` | Preserved as-is. | `s3_header` |
| `block_time` | string | `block_time` | Preserved as-is. | `s3_header` |
| `event_idx` | integer | `events` order | Zero-based index within each block. | `derived` |
| `event_time` | string | `events[].time` | Renamed. | `official` |
| `hash` | string | `events[].hash` | Preserved as-is. | `official` |
| `event_type` | string | `events[].inner` key | First-level key under `inner`. | `derived` |
| `payload_json` | string json | `events[].inner.*` | Event payload serialized as JSON. | `derived` |
| `event_time_dt` | timestamp | `event_time` | Cast to timestamp. | `derived` |
| `minute` | timestamp | `event_time` | Floored to minute. | `derived` |

### `ledger_deltas`

| Field | Type | Raw source field | Transformation | Status |
|---|---|---|---|---|
| `event_uid` | string | `block_number`, `event_idx` | Inherited from `events_flat`. | `derived` |
| `line_no` | integer | file line order | Inherited from `events_flat`. | `derived` |
| `block_number` | integer | `block_number` | Inherited from `events_flat`. | `s3_header` |
| `local_time` | string | `local_time` | Inherited from `events_flat`. | `s3_header` |
| `block_time` | string | `block_time` | Inherited from `events_flat`. | `s3_header` |
| `event_idx` | integer | `events` order | Inherited from `events_flat`. | `derived` |
| `event_time` | string | `events[].time` | Inherited from `events_flat`. | `official` |
| `hash` | string | `events[].hash` | Inherited from `events_flat`. | `official` |
| `event_type` | string | `inner` key | Fixed to `LedgerUpdate`. | `derived` |
| `primary_user` | string/null | `LedgerUpdate.users[0]` | First item in `users`. | `derived` |
| `users_json` | string json | `LedgerUpdate.users` | Serialized as JSON. | `derived` |
| `delta_type` | string | `delta.type` | Renamed. | `official` |
| `delta_json` | string json | `LedgerUpdate.delta` | Full delta serialized as JSON. | `derived` |
| `usdc` | float | `delta.usdc` | Cast to numeric. | `observed` |
| `amount` | float | `delta.amount` | Cast to numeric. | `observed` |
| `usdcValue` | float | `delta.usdcValue` | Cast to numeric. | `observed` |
| `fee` | float | `delta.fee` | Cast to numeric. | `observed` |
| `token` | string | `delta.token` | Preserved as-is. | `observed` |
| `destination` | string | `delta.destination` | Preserved as-is. | `observed` |
| `toPerp` | boolean | `delta.toPerp` | Preserved as-is. | `observed` |
| `accountValue` | float | `delta.accountValue` | Cast to numeric. | `observed` |
| `liquidatedNtlPos` | float | `delta.liquidatedNtlPos` | Cast to numeric. | `observed` |
| `leverageType` | string | `delta.leverageType` | Preserved as-is. | `observed` |

### `funding_deltas`

| Field | Type | Raw source field | Transformation | Status |
|---|---|---|---|---|
| `event_uid` | string | `block_number`, `event_idx` | Inherited from `events_flat`. | `derived` |
| `line_no` | integer | file line order | Inherited from `events_flat`. | `derived` |
| `block_number` | integer | `block_number` | Inherited from `events_flat`. | `s3_header` |
| `local_time` | string | `local_time` | Inherited from `events_flat`. | `s3_header` |
| `block_time` | string | `block_time` | Inherited from `events_flat`. | `s3_header` |
| `event_idx` | integer | `events` order | Inherited from `events_flat`. | `derived` |
| `event_time` | string | `events[].time` | Inherited from `events_flat`. | `official` |
| `hash` | string | `events[].hash` | Inherited from `events_flat`. | `official` |
| `event_type` | string | `inner` key | Fixed to `Funding`. | `derived` |
| `delta_idx` | integer | `Funding.deltas` order | Zero-based index within each funding delta list. | `derived` |
| `user` | string | `deltas[].user` | Preserved as-is. | `official` |
| `coin` | string | `deltas[].coin` | Preserved as-is. | `official` |
| `funding_amount` | float | `deltas[].funding_amount` | Cast to numeric. | `official` |
| `szi` | float | `deltas[].szi` | Cast to numeric. | `official` |
| `funding_rate` | float | `deltas[].funding_rate` | Cast to numeric. | `official` |

### Derived Event Tables

| Table | Field | Type | Raw source field | Transformation | Status |
|---|---|---|---|---|---|
| `liquidation_events` | `n_positions` | integer | `delta.liquidatedPositions` | Length of the liquidated position list. | `derived` |
| `liquidation_positions` | `position_idx` | integer | `delta.liquidatedPositions` order | Zero-based index. | `derived` |
| `liquidation_positions` | `coin` | string | `liquidatedPositions[].coin` | Preserved as-is. | `observed` |
| `liquidation_positions` | `szi` | float | `liquidatedPositions[].szi` | Cast to numeric. | `observed` |
| `liquidation_positions` | `position_json` | string json | `liquidatedPositions[]` | Serialized as JSON. | `derived` |
| `minute_event_counts` | `minute` | timestamp | `event_time` | Minute-level timestamp. | `derived` |
| `minute_event_counts` | `event_type` | string | `events_flat.event_type` | Grouping key. | `derived` |
| `minute_event_counts` | `events` | integer | `events_flat` | Count by `minute` and `event_type`. | `derived` |
| `event_type_counts` | `event_type` | string | `events_flat.event_type` | Grouping key. | `derived` |
| `event_type_counts` | `events` | integer | `events_flat` | Count by `event_type`. | `derived` |
| `ledger_delta_type_counts` | `delta_type` | string | `ledger_deltas.delta_type` | Grouping key. | `derived` |
| `ledger_delta_type_counts` | `events` | integer | `ledger_deltas` | Count by `delta_type`. | `derived` |
| `funding_coin_summary` | `coin` | string | `funding_deltas.coin` | Grouping key. | `official` |
| `funding_coin_summary` | `rows` | integer | `funding_deltas` | Count by coin. | `derived` |
| `funding_coin_summary` | `users` | integer | `funding_deltas.user` | Distinct user count. | `derived` |
| `funding_coin_summary` | `funding_amount` | float | `funding_deltas.funding_amount` | Sum by coin. | `derived` |
| `funding_coin_summary` | `abs_funding_amount` | float | `funding_deltas.funding_amount` | Sum of absolute values. | `derived` |
| `funding_coin_summary` | `mean_funding_rate` | float | `funding_deltas.funding_rate` | Mean by coin. | `derived` |
| `funding_coin_summary` | `abs_szi` | float | `funding_deltas.szi` | Sum of absolute values. | `derived` |
| `transfer_summary` | `delta_type` | string | `ledger_deltas.delta_type` | Transfer category grouping key. | `derived` |
| `transfer_summary` | `events` | integer | `ledger_deltas` | Aggregation by transfer delta type. | `derived` |
| `transfer_summary` | `users` | integer | `ledger_deltas.primary_user` | Distinct user count. | `derived` |
| `transfer_summary` | `usdc` | float | `ledger_deltas.usdc` | Sum by delta type. | `derived` |
| `bridge_summary` | `delta_type` | string | `ledger_deltas.delta_type` | Deposit/withdraw grouping key. | `derived` |
| `bridge_summary` | `events` | integer | `ledger_deltas` | Aggregation by deposit/withdraw delta type. | `derived` |
| `bridge_summary` | `users` | integer | `ledger_deltas.primary_user` | Distinct user count. | `derived` |
| `bridge_summary` | `usdc` | float | `ledger_deltas.usdc` | Sum by delta type. | `derived` |
| `bridge_summary` | `fee` | float | `ledger_deltas.fee` | Sum by delta type. | `derived` |
| `staking_summary` | `event_type` | string | staking events | Staking event type grouping key. | `derived` |
| `staking_summary` | `events` | integer | staking events | Aggregation by staking event type. | `derived` |
| `staking_summary` | `users` | integer | staking event user | Distinct user count. | `derived` |
