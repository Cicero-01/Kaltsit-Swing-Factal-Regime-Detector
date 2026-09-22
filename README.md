# Kaltsit-Swing-Factal-Regime-Detector
*Kaltsit* 是一个基于道氏理论的市场状态分类工具。它通过识别市场分型 `Fractals`与波段高低点 `Swing High/Low`，自动将市场划分为**上升趋势、下降趋势与震荡状态**。

*Kaltsit*, a Market Regime Classification Tool, is developed based on the Classical Dow Theory. It automatically categorizes the market into **Trend Up, Trend Down, and Chaos/Range states** by identifying market `Fractals` and `Swing Highs/Lows`.

其计算的原理是:

How it work:

(1)将一条K线加上**其左侧和右侧各n条**K线作为一个分形`Fractal`，并找到这个分形中的波段最高点`Swing High`和波段最低点`Swing Low` 。

(1)Treat a single candlestick **along with its N left and right adjacent bars** as a `fractal`. The local maximum in this window is defined as a `Swing High`, and the local minimum as a `Swing Low`.
  
(2)计算这个分形和**前一个分形**间，最高点和最低点的变化趋势，可以将趋势分为：

(2)Compare the current extreme points with the previous ones to determine the market state. The trends are classified into three states:

- `State 1` (上升趋势 Trend Up):       波段高点、低点均抬高 (Higher High & Higher Low).
- `State 2` (下降趋势 Trend Down):  波段高点、低点均降低 (Lower High & Lower Low).
- `State 0` (震荡 Chaos):                  任何其他组合形式 / Any other combination

## 如何开始 Quick Start 

1. 传入CSV文件 / Prepare your csv files 
`df_raw = pd.read_csv("YOUR_CSV_NAME.csv")`
2. 运行Main.py / Run: python Kaltsit_Main.py

```
├──Kaltsit_Main.py               # run this file / 运行这个
├──backtesting_daily.csv         # example file / 示例文件
└──example_kline.csv             # output file (if have) / 输出文件（运行后产生）
```
3. （可选）若您使用我们的可视化工具*Eyjafalla*，则可以自动生成一份叠加了状态背景色块的 HTML 交互式图表。

(Optional) If you use our backtesting visualization tool *Eyjafalla*, the output file is compatible and it would generate a`.html` file displaying different market states using color-coded regions.

[Eyjafalla-Quant Visualizer](https://github.com/Cicero-01/Eyjafalla-Quant-Visualizer) 

## 如何使用自己的数据 / How to use your own data

您可以将自己关注的市场数据的导入我们的可视化工具中进行分类。**您只需要确保文件使用我们的标准格式进行命名**。所有的数据**都应由`.csv`格式储存**。

You can import your own market data to classfiy the regimes, only **ensure that the columns are named and structured using our standard format.**  All data must be saved **in `.csv` format.** 

**必须含有以下几列 /These columns are necessary: **

| Open Time | Open | Close | High | Low |
| --------- | ---- | ----- | ---- | --- |
|           |      |       |      |     |

**解释/ Description：**

| 列名 / Column | 说明 / Description                   | 数据类型 / Type  |
| ----------- | ---------------------------------- | ------------ |
| `Open Time` | 一根K线的开盘时间/ Open time of the candle | `datetime64` |
| `Open`      | 开盘价/ Open price                    | `float`      |
| `Close`     | 收盘价/ Close                         | `float`      |
| `High`      | 最高价/ High price                    | `float`      |
| `Low`       | 最低价/ Low price                     | `float`      |

## 局限与讨论 / Limitations and Discussion

### 1.机械分型不能等同于人类视觉判断 (Rigid Fractals vs. Human Visual "Waves")
（）
### 2.状态闪烁现象 (State Flickering / Whipsaw Effect)
（）
### 3.状态判断的算法滞后性 (Inherent Algorithmic Lag)
（）

## ⚠️**免责声明 / Disclaimer:** 

本工具仅供研究学习使用，**不构成任何投资或财务建议**。开发者及贡献者对因使用本软件或其中代码所造成的任何直接或间接财务损失，不承担任何法律责任。金融市场交易具有极高风险，请在真实交易前进行充分测试，并自行承担所有风险 (DYOR)。
This project and its tools are provided for educational and research purposes only and **do not constitute financial or investment advice**. The developers and contributors assume no legal responsibility or liability for any direct or indirect financial losses incurred from the use of this software. Trading in financial markets involves significant risk. Always do your own research (DYOR) and test thoroughly before real trading.
