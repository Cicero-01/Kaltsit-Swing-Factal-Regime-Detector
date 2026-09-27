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

### 1.机械分型不能等同于人类视觉判断  Rigid Fractals vs. Human Visual "Trend"
作为主观交易员，不论是从应用目的还是减轻工作量出发，我们希望寻找一种算法来代替主观的判断。但是`Swing`算法划分出的`State 0/1/2`不能完全与肉眼划分的“波段”等同起来。

例如，在一段行情的日线数据的可视化中(Fig 1.)，主观交易员可能会把这段时间分为一个主要的多头浪（May 23, 2023 - Oct 06, 2025）和一个主要的空头浪（Oct 06, 2025之后），或是在这两个大趋势中划分出更精细的形状。但是`Kaltist`在N=2参数下，将这一段数据切割成了非常的细小的周期。因此，简单的局部极值分型，绝不能简单等同于人类视觉感知中的“宏观大趋势”。简单的分型算法无法复现人类大脑对于市场状态的判断。

<img width="2530" height="1027" alt="example_daily" src="https://github.com/user-attachments/assets/1e4c4f6f-7c61-4a24-9da9-3ac14c0c2e3c" />

<p align="center">Fig1. BTC-USDT daily,  2023.05.23-2026.07.27（数据来源/Data Source: Binance）</p>

<p align="center"> 画图工具/ Visual Tool: Eyjafalla-Quant Visualizer </p>

### Limitation 2.状态闪烁 State Flickering 
道氏理论认为：”趋势一旦形成，则大概率会延续，直到出现明确的反转信号。”

人类交易员是在评估了**不确定数量的大量K线**后，通过宏观结构得出结论的，视野更加宏观。相比之下，算法受到参数$N$限制，一次只能在2N+1条K线中寻找极值，因此对于局部噪音极其敏感。会将价格波动误认为“趋势反转的信号”。具体表现为，Swing算法划分的市场状态经常在趋势`State 1/2`和震荡`State 0`间来回切换，每个状态的持续时间很短(Fig 1.)。这个缺陷本质上是参数N取值的问题。
### Limitation 3.状态判断的算法滞后性 Inherent Algorithmic Lag
为了解决N取值带来的小窗口问题，我们尝试增加N的数值,或是使用`Kaltsist`计算周线级别数据、再反向映射回日线，试图让机器拥有更广阔的视野。但是实际测试发现这会导致状态判断出现严重的滞后性(Fig 2.)。

<img width="2525" height="1025" alt="example_weekly" src="https://github.com/user-attachments/assets/5b649104-de68-4eff-ab02-8c82e23346f6" />

<p align="center">Fig1. BTC-USDT weekly,  2023.01.02-2026.07.20（数据来源/Data Source: Binance）</p>

<p align="center">画图工具/ Visual Tool: Eyjafalla-Quant Visualizer </p>

例如，在Fig 2.中，算法将2段明显的上升趋势标注为`Trend Down`(Oct 2, 2023 - Dec 18, 2023)(Apr 14, 2025 - May 19, 2025)，这在人类交易员眼中是不应该发生的事情。

这种滞后性是算法为了避免未来函数而不可避免的问题。在确认一个点为极值时，**必须**等待其右侧走完N+1根K线。这意味着，当算法最终确认并在今天发出“趋势反转”信号时，实际上该极值点在N+1条K线前就已经发生了。只是当时的K线形态是未知的，趋势无法立刻确认。因此，简单粗暴地扩大N或提升时间维度，虽然能一定程度解决状态闪烁问题，但必然导致对真实市场变化的响应极度迟缓。也就是说，这种基于极值的简单分形算法，无法同时做到状态平滑与敏锐。

## ⚠️**免责声明 / Disclaimer:** 

本工具仅供研究学习使用，**不构成任何投资或财务建议**。开发者及贡献者对因使用本软件或其中代码所造成的任何直接或间接财务损失，不承担任何法律责任。金融市场交易具有极高风险，请在真实交易前进行充分测试，并自行承担所有风险 (DYOR)。
This project and its tools are provided for educational and research purposes only and **do not constitute financial or investment advice**. The developers and contributors assume no legal responsibility or liability for any direct or indirect financial losses incurred from the use of this software. Trading in financial markets involves significant risk. Always do your own research (DYOR) and test thoroughly before real trading.
