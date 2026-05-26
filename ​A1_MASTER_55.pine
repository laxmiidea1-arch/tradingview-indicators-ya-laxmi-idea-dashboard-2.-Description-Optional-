//@version=5
indicator("A1 MASTER 55", overlay=true, max_labels_count=500)

// ==========================================
// SECTION 1: NEEV MEMORY (Jas ka Tas #55)
// ==========================================
varip float r_in = 0.0
varip float r_ep = 0.0
varip int x_timer = 0
varip string x_msg = ""
var int pinBarState = 0
// Multi-Pass Variable Declarations to solve compilation hierarchy loops
bool x_t = false
bool c_t = false
varip bool isSystemSleeping = false
varip int last_exit_bar = 0

// ==========================================
// SECTION 2: NEEV ADDRESSES (Full Mathematical Frame)
// ==========================================
m_l = ta.sma(close, 20), st_dev = ta.stdev(close, 20)
u_b = m_l + 2 * st_dev, l_b = m_l - 2 * st_dev
e200 = ta.ema(close, 200), e9 = ta.ema(close, 9), vw = ta.vwap 
v100 = ta.highest(high, 100), s100 = ta.lowest(low, 100)
v20 = ta.highest(high, 20), s20 = ta.lowest(low, 20)
mxP = (ta.highest(high, 50) + ta.lowest(low, 50)) / 2 

// ==========================================
// SECTION 3: THE 8-GATE SENSORS & HISTORICAL FLAT GRID ENFORCER (Logic Lock #55)
// ==========================================
isFlatV20  = (v20[1] == v20[2]) and (v20[2] == v20[3]) and (v20 == v20[1])
isFlatS20  = (s20[1] == s20[2]) and (s20[2] == s20[3]) and (s20 == s20[1])
isFlatV100 = (v100[1] == v100[2]) and (v100[2] == v100[3]) and (v100 == v100[1])
isFlatS100 = (s100[1] == s100[2]) and (s100[2] == s100[3]) and (s100 == s100[1])
isFlatMxP  = (mxP[1] == mxP[2]) and (mxP[2] == mxP[3]) and (mxP == mxP[1])

buff = syminfo.mintick * 450 
isStatU = isFlatV20 and math.abs(high - v20) < buff
isStatD = isFlatS20 and math.abs(low - s20) < buff
isJackU = isStatU and isFlatV100 and math.abs(v20 - v100) < (syminfo.mintick * 200)
isJackD = isStatD and isFlatS100 and math.abs(s20 - s100) < (syminfo.mintick * 200)

hMid = (ta.crossover(close, m_l) or ta.crossunder(close, m_l)) 
hBnd = (high >= u_b or low <= l_b)   
h200 = (ta.crossover(close, e200) or ta.crossunder(close, e200)) 
hD   = (isFlatV100 and ta.crossover(close, v100)) or (isFlatS100 and ta.crossunder(close, s100)) 

string cName = h200 ? "200 EMA HALT" : hMid ? "20 EMA HALT" : hBnd ? "BB BND HALT" : hD ? "CROSS HALT" : ""
valid8P = cName != ""

// ==========================================
// SECTION 4: SURGICAL ENGINE (Two Shields Pure Priority Hierarchy - Absolutely NO IF)
// ==========================================
isHighPriorityX = (high >= v100 or high >= v20 or low <= s20 or low <= s100)

// BB Height Stopping Engine Mapped on Pure Volatility Coordinates
bb_height = u_b - l_b
isBbHeightStopped = (bb_height <= bb_height[1]) or ((bb_height - bb_height[1]) <= (syminfo.mintick * 10))

isWideBB = bb_height > ta.sma(bb_height, 20)
isExpanding = bb_height > bb_height[1]
isWalking = (high >= u_b or low <= l_b) and isExpanding and isWideBB

// Base Candle Components & Volumetric Sensitivity
isGreenCandle = close > open
isRedCandle   = close < open
c_body = math.abs(close - open)
u_wick = high - math.max(open, close)
l_wick = math.min(open, close) - low
tot_h  = high - low

// Cache Memory for Boundary Breaches
isConf2_CacheOutsideU = (high >= u_b) or (high[1] >= u_b[1]) or (high[2] >= u_b[2])
isConf2_CacheOutsideD = (low <= l_b) or (low[1] <= l_b[1]) or (low[2] <= l_b[2])

// Thoda Bhi Pin Bar Requirement Restored
isThodaPinBarU = (u_wick >= (tot_h * 0.15)) and tot_h > 0
isThodaPinBarD = (l_wick >= (tot_h * 0.15)) and tot_h > 0

// --- ALL LIVE CANCEL (C) LOOPS REMOVED FOR UNINTERRUPTED WALKING ---
c_t := false

// --- TWO SHIELDS EXIT CORE ENGINE ---

// PRIORITY RANK #1: Last Walking Candle Reversal (बाउंड्री के अंदर वापस क्लोज़ होना)
isLastWalkingCandleU = (r_in == 1.0) and (close[1] >= u_b[1]) and (close < u_b)
isLastWalkingCandleD = (r_in == -1.0) and (close[1] <= l_b[1]) and (close > l_b)
isExit_LastCandle = isLastWalkingCandleU or isLastWalkingCandleD

// PRIORITY RANK #2: 9EMA Shield (सफेद रेखा टूटना)
isExit_9EmaShieldU = (r_in == 1.0) and (close < e9) and isRedCandle
isExit_9EmaShieldD = (r_in == -1.0) and (close > e9) and isGreenCandle
isExit_9EmaShield   = isExit_9EmaShieldU or isExit_9EmaShieldD

// Unified Exit Matrix Resolved Strictly via 2 Core Shields (Station & Jackson Completely Excluded)
isSupremeXTriggered = isExit_LastCandle ? true : isExit_9EmaShield ? true : false
x_t := (r_in != 0.0) and isSupremeXTriggered

// Universal Sleep State Allocation Circuit
isAnyExitTriggered = x_t or c_t
isSystemSleeping := isAnyExitTriggered ? true : isSystemSleeping

// Memory Lock for the 5-Bar Anti-Flood Cool Down Shield
last_exit_bar := isAnyExitTriggered ? bar_index : last_exit_bar
bars_since_last_exit = bar_index - last_exit_bar
isCoolDownActive = (bars_since_last_exit >= 0 and bars_since_last_exit <= 5)

x_timer := isAnyExitTriggered ? bar_index : x_timer
x_msg   := isExit_LastCandle ? "PRIORITY 1: LAST WALK CANDLE" : isExit_9EmaShield ? "PRIORITY 2: 9EMA SHIELD BREAK" : cName

// ==========================================
// SECTION 5: ENTRY ENGINE WITH ZERO-LAG OVERRIDE CIRCUIT & 5-BAR WINDOW
// ==========================================
last_x_high = ta.valuewhen(x_t, high, 0)
last_x_low  = ta.valuewhen(x_t, low, 0)

isNextBarBreakBuy  = ta.crossover(close, last_x_high) and ta.barssince(x_t) <= 5
isNextBarBreakSell = ta.crossunder(close, last_x_low) and ta.barssince(x_t) <= 5

isInstantE = (r_in[1] == 0.0 and x_t[1])
isSqueezing = (u_b - l_b) < (u_b[1] - l_b[1])
isGateRun = ((close > m_l and close > high[1]) or (close < m_l and close < low[1])) and isSqueezing
isVReversal = (close > close[1] and close[1] < close[2]) and (close > m_l)

pinStateTrigger = (r_in == 0.0 or hMid) ? 0 : pinBarState

isPriorityBuyEntry  = (pinStateTrigger[1] == -1 or pinStateTrigger[2] == -1) and close > open and close > e9
isPrioritySellEntry = (pinStateTrigger[1] == 1 or pinStateTrigger[2] == 1) and close < open and close < e9

last_rally_high = ta.valuewhen(r_in[1] == 0.0 and r_in != 0.0, high, 0)
last_rally_low  = ta.valuewhen(r_in[1] == 0.0 and r_in != 0.0, low, 0)

isReEntryBuy  = (last_rally_high > 0.0) and ta.crossover(close, last_rally_high) and close > e9
isReEntrySell = (last_rally_low > 0.0) and ta.crossunder(close, last_rally_low) and close < e9

canBaseE = (r_in == 0.0) and (isNextBarBreakBuy or isNextBarBreakSell or isInstantE or bar_index - x_timer >= 0 or isVReversal or isGateRun or isPriorityBuyEntry or isPrioritySellEntry or isReEntryBuy or isReEntrySell)

// --- 🚨 FIXED BRAHMASTRA: ZERO-LAG TREND OVERRIDE MATRIX ---
// Mid-line (20 SMA) aur 9 EMA dono ke ek sath clear crossover close hone par 5-bar ka taala immediate smash ho jayega
isOverrideCloseBuy  = (close > m_l) and (close > e9) and isGreenCandle
isOverrideCloseSell = (close < m_l) and (close < e9) and isRedCandle

isBreakoutOverride = isNextBarBreakBuy or isNextBarBreakSell or isOverrideCloseBuy or isOverrideCloseSell
canE = canBaseE and (not isCoolDownActive or isBreakoutOverride)

// The Supreme 9EMA Flip Lock Matrix Enforced
isOldRallyDeadU = (r_in[1] == -1.0 or r_in[2] == -1.0) and (close > e9)
isOldRallyDeadD = (r_in[1] == 1.0 or r_in[2] == 1.0) and (close < e9)
isAllowOppositeBuy  = (r_in[1] == 0.0) or isOldRallyDeadU
isAllowOppositeSell = (r_in[1] == 0.0) or isOldRallyDeadD

// Sideways Entry Ban Layer
isBuyBan  = (high >= u_b) and (not isWalking)
isSellBan = (low <= l_b) and (not isWalking)

eB = canE and (close > m_l) and (close > e9) and (close > open) and not isBuyBan and isAllowOppositeBuy
eS = canE and (close < m_l) and (close < e9) and (close < open) and not isSellBan and isAllowOppositeSell

r_in := x_t ? 0.0 : (eB ? 1.0 : (eS ? -1.0 : r_in))
r_ep := (eB or eS) ? close : (x_t ? 0.0 : r_ep)

r_in := (x_t and eB) ? 1.0 : (x_t and eS) ? -1.0 : r_in
r_ep := (x_t and (eB or eS)) ? close : r_ep

// ==========================================
// SECTION 6: VISUALS (Instant Entry Rail & Restore)
// ==========================================
isTradeActive = (r_in != 0.0) or (eB or eS)
h20_v = ta.valuewhen(eB or eS, ta.highest(high, 20), 0), l20_v = ta.valuewhen(eB or eS, ta.lowest(low, 20), 0)
rng = h20_v - l20_v

current_ep = (eB or eS) ? close : r_ep
t1 = (eB or (r_in == 1.0 and not x_t)) ? current_ep + (rng * 0.236) : (eS or (r_in == -1.0 and not x_t)) ? current_ep - (rng * 0.236) : na
t2 = (eB or (r_in == 1.0 and not x_t)) ? current_ep + (rng * 0.382) : (eS or (r_in == -1.0 and not x_t)) ? current_ep - (rng * 0.382) : na
t3 = (eB or (r_in == 1.0 and not x_t)) ? current_ep + (rng * 0.618) : (eS or (r_in == -1.0 and not x_t)) ? current_ep - (rng * 0.618) : na
t4 = (eB or (r_in == 1.0 and not x_t)) ? current_ep + (rng * 0.786) : (eS or (r_in == -1.0 and not x_t)) ? current_ep - (rng * 0.786) : na

plot(u_b, "Upper", color=#2196F3, style=plot.style_linebr)
plot(l_b, "Lower", color=#2196F3, style=plot.style_linebr)
plot(m_l, "Mid Rail", color=#FF9800, linewidth=3)
plot(e9, "9 EMA", color=color.white)
plot(vw, "VWAP", color=#9C27B0, linewidth=2) 
plot(e200, "200 EMA", color=#00FFFF, linewidth=3)
plot(mxP, "MaxPain", color=#00FF00, linewidth=2)

plot(isTradeActive ? current_ep : na, "Entry Rail", color=color.yellow, linewidth=3, style=plot.style_linebr)
plot(isTradeActive ? t1 : na, "T1", color=color.new(color.green, 50), style=plot.style_linebr, linewidth=2)
plot(isTradeActive ? t2 : na, "T2", color=color.new(color.blue, 50), style=plot.style_linebr, linewidth=2)
plot(isTradeActive ? t3 : na, "T3", color=color.new(color.orange, 50), style=plot.style_linebr, linewidth=2)
plot(isTradeActive ? t4 : na, "T4", color=color.new(color.red, 50), style=plot.style_linebr, linewidth=2)

plot(v20, "VP20", color=color.yellow, style=plot.style_circles)
plot(s20, "SP20", color=color.orange, style=plot.style_circles)
plot(v100, "VP100", color=#00FF00, style=plot.style_circles) 
plot(s100, "SP100", color=#FF0000, style=plot.style_circles) 

plotshape(r_in == 1.0 and r_in[1] == 0.0, "BUY", shape.labelup, location.belowbar, #00FF00, 0, "RALLY", #FFFFFF, size=size.small)
plotshape(r_in == -1.0 and r_in[1] == 0.0, "SELL", shape.labeldown, location.abovebar, #FF0000, 0, "RALLY", #FFFFFF, size=size.small)

// Supreme Hierarchical White 'X' Stamp
plotshape(x_t, "EXIT", shape.xcross, location.abovebar, #FFFFFF, 0, "X", #FFFFFF, size=size.small)

lbl_x = x_t ? label.new(bar_index, high, x_msg, color=color.new(color.black, 40), textcolor=color.white, style=label.style_label_down, size=size.small) : na

// ==========================================
// SECTION 7: STATUS & MONITOR TABLES
// ==========================================
var table info = table.new(position.top_right, 2, 1, color.black, color.gray, 1, color.white, 1)
table.cell(info, 0, 0, "A1 MASTER MAIND", bgcolor=color.new(color.blue, 30))
table.cell(info, 1, 0, r_in != 0 ? "TRAIN ACTIVE" : "SCANNING", bgcolor=r_in != 0 ? color.green : color.gray)

var table mon = table.new(position.bottom_right, 4, 5, #12151c, #2a2e39, 1, #d1d4dc, 1)
rr_val = r_in != 0 ? math.abs(close - r_ep) / (st_dev * 2) : 0.0
d_pct  = (close - close[1]) / close[1] * 100

table.cell(mon, 0, 0, "FLOW", bgcolor=#2962ff, text_color=color.white)
table.cell(mon, 1, 0, r_in > 0 ? "BUY" : r_in < 0 ? "SELL" : "WAIT", bgcolor=r_in > 0 ? #00c073 : r_in < 0 ? #f05350 : #787b86, text_color=color.white)
table.cell(mon, 2, 0, "Z-SCR", bgcolor=#2962ff, text_color=color.white)
table.cell(mon, 3, 0, str.tostring((close - m_l) / st_dev, "#.##"), text_color=color.white)
table.cell(mon, 0, 1, "DELTA%", text_color=#787b86), table.cell(mon, 1, 1, str.tostring(d_pct, "#.##") + "%", text_color=d_pct >= 0 ? #00c073 : #f05350)
table.cell(mon, 2, 1, "RSI", text_color=#787b86), table.cell(mon, 3, 1, str.tostring(ta.rsi(close, 14), "#.#"), text_color=color.white)
table.cell(mon, 0, 2, "RR RATIO", text_color=#787b86), table.cell(mon, 1, 2, str.tostring(rr_val, "1:#.#"), text_color=#9c27b0)
table.cell(mon, 2, 2, "PAIN", text_color=#787b86), table.cell(mon, 3, 2, str.tostring(mxP, "#.#"), text_color=#e1d533)
table.cell(mon, 0, 3, "ENT", text_color=#787b86), table.cell(mon, 3, 3, str.tostring(vw, "#.#"), text_color=color.white)
table.cell(mon, 2, 3, "VPOC", text_color=#787b86), table.cell(mon, 3, 3, str.tostring(v20, "#.#"), text_color=#e1d533)
table.cell(mon, 0, 4, "SPOC", text_color=#787b86), table.cell(mon, 1, 4, str.tostring(s20, "#.#"), text_color=#ff9800)
table.cell(mon, 2, 4, "VWAP", text_color=#787b86), table.cell(mon, 3, 4, str.tostring(ta.vwap, "#.#"), text_color=#f05350)
