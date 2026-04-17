# ⚡ Backtesting Engine

> A high-performance Python framework for simulating and evaluating algorithmic trading strategies on historical market data.

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)]()

---

## 📌 Overview

The **Backtesting Engine** is a quantitative finance tool designed to simulate trading strategies against historical OHLCV (Open, High, Low, Close, Volume) market data. It provides a clean, extensible interface for strategy authoring, position management, and performance analytics — enabling rapid iteration and validation of systematic trading ideas before live deployment.

Built with performance and flexibility in mind, the engine supports a wide range of asset classes including equities, forex, and crypto.

---

## 🧠 Key Features

- **Strategy Framework** — Define custom strategies by implementing `init()` and `next()` methods; full access to price data and indicator series
- **Order Management** — Support for market orders, limit orders, stop-loss, and take-profit levels
- **Performance Analytics** — Computes Sharpe Ratio, Sortino Ratio, Max Drawdown, CAGR, Win Rate, Profit Factor, and more
- **Built-in Optimizer** — Grid-search over strategy parameters to find optimal configurations
- **Interactive Visualization** — HTML-based interactive plots of equity curves, drawdowns, and trade annotations
- **Indicator-Agnostic** — Compatible with any indicator library (TA-Lib, pandas-ta, custom functions)
- **Composable Utilities** — Shared library of reusable base strategies, crossover detection, and signal helpers

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Core Engine | Python 3.8+ |
| Data Handling | pandas, NumPy |
| Visualization | Bokeh (interactive HTML plots) |
| Optimization | Itertools / SciPy-compatible |
| Testing | pytest |

---

## 🚀 Quick Start

### Installation

```bash
git clone https://github.com/vedakshari1-collab/Backtesting-Engine.git
cd Backtesting-Engine
pip install -r requirements.txt
```

### Example — SMA Crossover Strategy

```python
from bactesting import Backtest, Strategy
from bactesting.lib import crossover, SMA

class SmaCross(Strategy):
    n1 = 10
    n2 = 20

    def init(self):
        price = self.data.Close
        self.ma1 = self.I(SMA, price, self.n1)
        self.ma2 = self.I(SMA, price, self.n2)

    def next(self):
        if crossover(self.ma1, self.ma2):
            self.buy()
        elif crossover(self.ma2, self.ma1):
            self.sell()

bt = Backtest(data, SmaCross, commission=0.002, exclusive_orders=True)
stats = bt.run()
bt.plot()
```

### Sample Output

```
Return [%]                   589.35
Buy & Hold Return [%]        703.46
Sharpe Ratio                   0.66
Sortino Ratio                  1.30
Max. Drawdown [%]            -33.08
Win Rate [%]                  53.76
# Trades                         93
Profit Factor                  2.13
```

---

## 📁 Project Structure

```
Backtesting-Engine/
├── bactesting/
│   ├── __init__.py          # Public API: Backtest, Strategy
│   ├── _core.py             # Order execution & position tracking
│   ├── _stats.py            # Performance metrics computation
│   ├── _plotting.py         # Interactive Bokeh visualizations
│   ├── lib.py               # Composable utilities & base strategies
│   └── test/                # Sample datasets and indicator helpers
├── requirements.txt
└── README.md
```

---

## 📊 Performance Metrics Computed

| Metric | Description |
|---|---|
| **CAGR** | Compound Annual Growth Rate |
| **Sharpe Ratio** | Risk-adjusted return (annualized) |
| **Sortino Ratio** | Downside-risk-adjusted return |
| **Calmar Ratio** | CAGR / Max Drawdown |
| **Max Drawdown** | Largest peak-to-trough equity decline |
| **Win Rate** | Percentage of profitable trades |
| **Profit Factor** | Gross profit / Gross loss |
| **Expectancy** | Average expected return per trade |
| **Kelly Criterion** | Optimal position sizing fraction |

---

## 🔧 Strategy Optimization

```python
stats, heatmap = bt.optimize(
    n1=range(5, 30, 5),
    n2=range(10, 70, 5),
    maximize='Sharpe Ratio',
    constraint=lambda p: p.n1 < p.n2
)
```

---

## 🤝 Contributors

| Name | GitHub |
|---|---|
| Vedakshari | [@vedakshari1-collab](https://github.com/vedakshari1-collab) |

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

> *Built for quants, by quants. Simulate first, trade smart.*
