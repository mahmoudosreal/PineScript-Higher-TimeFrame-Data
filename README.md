# PineScript-Higher-TimeFrame-Data

### Detailed Explanation of Pine Script Code

The Pine Script provided defines an indicator for displaying an Exponential Moving Average (EMA) from a higher timeframe on the current chart. Here’s a breakdown of each part of the script:

#### Indicator Definition
- **Function**: `indicator()`
  - Sets the indicator's name and specifies that it should be overlaid on the main price chart.

#### Inputs
- `resolution`: Allows the user to select the timeframe for the EMA. Default is daily ("D").
- `emaLen`: Allows the user to set the length of the EMA. Default is 50.
- `emaColor`: Boolean input to decide if the EMA line should change color based on its position relative to the close price.
- `smooth`: Boolean input to choose between a smooth transition of the EMA values across different bars or a step-like transition.

#### EMA Calculation
- **Function**: `ta.ema()`
  - Calculates the EMA based on the close prices of the current chart and the length specified.

#### Fetching Higher Timeframe Data
- `emaSmooth`: Uses `request.security()` to fetch the EMA data from the specified `resolution`, allowing for a smooth line when there are gaps in data due to the higher timeframe.
- `emaStep`: Similar fetch but with `gaps = barmerge.gaps_off`, providing a stepped look to the EMA line.

#### Plotting
- **Function**: `plot()`
  - Conditionally uses `emaSmooth` or `emaStep` depending on the `smooth` input.
  - Dynamically sets the color of the EMA line:
    - Green if the current close is above the EMA.
    - Red if below.
    - Blue if `emaColor` is false.
  - Sets the line width to 3 for better visibility.

### Concise Comments Inside Pine Script Code

```pinescript
// This Pine Script™ code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © mahmoudos

//@version=5
indicator("EMA - Security Function, Higher TimeFrame Data", overlay = true)

// User Input for Indicator Settings
resolution = input.timeframe(title = "EMA Timeframe", defval = "D")
emaLen = input.int(title = "EMA Length", defval = 50)
emaColor = input.bool(title = "EMA Color?", defval = true)
smooth = input.bool(title = "Smooth?", defval = true)

// Calculate EMA based on current price
ema = ta.ema(close, emaLen)

// Fetch EMA from higher timeframe with or without smoothing
emaSmooth = request.security(symbol = syminfo.tickerid, timeframe = resolution, expression = ema[barstate.isrealtime ? 1 : 0], gaps = barmerge.gaps_on, lookahead = barmerge.lookahead_off)
emaStep = request.security(symbol = syminfo.tickerid, timeframe = resolution, expression = ema[barstate.isrealtime ? 1 : 0], gaps = barmerge.gaps_off, lookahead = barmerge.lookahead_off)

// Plot EMA with optional color coding
plot(series = smooth ? emaSmooth : emaStep, title = "EMA Higher Timeframe", color = emaColor ? close > emaStep ? color.green : color.red : color.blue, linewidth = 3)
```

---
