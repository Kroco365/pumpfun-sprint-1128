# Curve Surfer — Pump.fun Bot Sprint

[![make.fun](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)
[![rank](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/Kroco365/pumpfun-sprint-1128/rank.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)
[![reward](https://make.fun/badge/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed/Kroco365/pumpfun-sprint-1128/reward.svg)](https://make.fun/bounty/u-riidzhsn-pump-fun-bot-sprint-make-bot-and-win-cb203fed)

Entry for **Pump.fun Bot Sprint: Make Bot And Win** — a deterministic
paper-trading bot for the pump.fun bonding-curve replay
(`agent-fun/replay/v2`, 10 SOL paper capital, max 5 open positions).

## Strategy

Curve Surfer watches every new bonding curve from its `CreateEvent` and
snipes a small fixed stake (up to 0.06 SOL, capped at 1% of the curve's real
SOL reserve) at the first sign of life: within the first 10 trades, price
still within 0.95–1.30x of the opening price, at least 3 buy trades and
0.2 SOL of cumulative buy volume, with buys at least 55% of all volume.

Exits are fast and mechanical so capital recycles into the next fresh coin:
take profit at **+12%** of cost, cut the loss at **−7%**, or flatten a stale
position after **1200 slots** (~8 minutes) while it is still at or below
cost. One buy per coin, every round trip at least 0.01 SOL.

All lamport math is exact `BigInt`; price ratios are compared by
cross-multiplication. No clock, no randomness, no network, no file reads —
the orders are a pure function of the harness messages, so the two grading
executions produce byte-identical action logs.

## Run

The entrypoint is `main.ts` (Node 22+, no dependencies). The grader runs
it as `node /bot/main.ts` and speaks the replay protocol over stdin/stdout.
