# LaunchShape — an analog forecaster for launch-day Twitch audiences

[![make.fun bounty](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Shiizuku/gta6-twitch-ccu-forecast-01/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Shiizuku/gta6-twitch-ccu-forecast-01/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

An entry for the make.fun bounty **Forecast GTA VI's launch-day Twitch
viewers hour by hour** (gta6-twitch-ccu-forecast-01), submitted by @qyxra.

The program reads one case — everything recorded about a launch up to the
end of its first hour, plus a library of past launches with their full
release days — and writes, for each of hours 2 to 24 of the release day,
five quantiles (5%, 25%, 50%, 75%, 95%) of that hour's concurrent Twitch
viewers in the game's category.

## The model

Analog forecasting. Each comparable launch contributes its ratio curve
r(h) = viewers(h) / viewers(1). A comparable's vote is weighted by how
similar it is to the target on what the case reveals before the deadline:

- the pre-release trajectory — log viewers relative to hour 1 over the
  last 37 hours before the release day (a rolling-midnight unlock is
  already hot at hour 1; a US-hours unlock is still quiet and ramps
  later, and the trajectory shows which),
- the size of hour 1 itself (log distance),
- platform overlap between the comparable and the target.

The forecast for an hour is the weighted empirical quantile of that
hour's log ratio across comparables, shifted by the target's log hour-1
print. If a case carries no usable comparable, the program degrades to
persistence with a random-walk spread, so it always writes a valid file.

## Validation

Leave-one-out over the 38 comparables shipped in the practice cases:
mean pinball loss on the log scale ≈ **0.28**, against ≈ 0.46 for flat
persistence on the same folds (the house baseline scores 0.387 on the
official practice set).

## Running

Standard library only (Python 3.13), deterministic, offline:

```
mkdir -p out
MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## License

MIT — see `LICENSE`.
