逻辑1：连续下跌开多
（B_4DOWN_LONG）
触发条件：前 4 根日线连续阴线（收盘价低于开盘价）
动作：在第 5 根 K 线开 多单

逻辑2：大阳反包 当天大阴
（BIG_GREEN_THEN_RED_SHORT）
触发条件：
前一日大阳线反包（收盘价 > 开盘价 × 1.07）
当日收盘形成大阴线（收盘价 < 开盘价）
动作：开 空单

BLACKLIST = {
    "BTCUSDT","ETHUSDT","BNBUSDT","SOLUSDT","XRPUSDT","DOGEUSDT",
    "ADAUSDT","AVAXUSDT","LTCUSDT","BCHUSDT","TONUSDT"
# ================= 逻辑 =================
def trigger_B_4DOWN_LONG(df_s, idx):
    if idx < 4:
        return False
    return all(candle_down(df_s.iloc[i]) for i in range(idx-4, idx))

def trigger_BIG_GREEN_THEN_RED_SHORT(df_s, idx):
    if idx < 1:
        return False
    prev = df_s.iloc[idx-1]
    last = df_s.iloc[idx]
    return (prev['close'] > prev['open']*1.07) and candle_down(last)
    

2026年1月1日回测情况
[逻辑1 4DOWN_LONG] 止盈 2% 总览
触发次数: 19746 | 盈利: 19334 | 亏损: 412 | 胜率: 97.91%
总盈利: 154237.17 USDT | 总亏损: -79306.98 USDT | 净收益: 74930.20 USDT
平均抗单天数: 12.22 | 最大浮亏: 398.34 USDT

[逻辑2 BIG_GREEN_THEN_RED_SHORT] 止盈 2% 总览
触发次数: 17103 | 盈利: 17101 | 亏损: 2 | 胜率: 99.99%
总盈利: 136790.00 USDT | 总亏损: -831.89 USDT | 净收益: 135958.12 USDT
平均抗单天数: 1.09 | 最大浮亏: 1459.99 USDT
