# BPR-Ai-Code

strategy Code ; copy from line 5 to the end

//@version=5
strategy("SuperTrend Swing Strategy (Long Only)", overlay=true, default_qty_type=strategy.percent_of_equity, default_qty_value=100)

// === Inputs ===
atrLength = input.int(10, title="ATR Length")
factor = input.float(3.0, title="Multiplier")

// === SuperTrend Calculation ===
[supertrend, direction] = ta.supertrend(factor, atrLength)

// direction = 1 (downtrend/red), -1 (uptrend/green)

// === Conditions ===
longCondition = direction == -1 and direction[1] == 1   // Red → Green flip
exitCondition = direction == 1 and direction[1] == -1   // Green → Red flip

// === Strategy Execution ===
if (longCondition)
    strategy.entry("Long", strategy.long)

if (exitCondition)
    strategy.close("Long")

// === Plotting ===
plot(supertrend, color = direction == -1 ? color.green : color.red, linewidth=2, title="SuperTrend")
