import datetime
import backtrader as bt

class SMABacktest(bt.Strategy):
    params = (
        ("short_period", 20),
        ("long_period", 50),
        ("risk_factor", 0.02),
    )

    def __init__(self):
        self.short_ma = bt.indicators.SimpleMovingAverage(self.data.close, period=self.params.short_period)
        self.long_ma = bt.indicators.SimpleMovingAverage(self.data.close, period=self.params.long_period)
        self.crossover = bt.indicators.CrossOver(self.short_ma, self.long_ma)

    def next(self):
        if self.crossover > 0 and self.position.size == 0:
            # Buy signal
            self.buy(size=self.calculate_position_size())

        elif self.crossover < 0 and self.position.size > 0:
            # Sell signal
            self.sell(size=self.position.size)

    def calculate_position_size(self):
        risk = self.params.risk_factor * self.broker.cash
        stop_loss = self.data.close[0] - self.atr[0] * 2  # Example: 2 ATR stop loss
        position_size = risk / stop_loss
        return position_size

if __name__ == '__main__':
    cerebro = bt.Cerebro()

    # Add data
    data = bt.feeds.YahooFinanceData(dataname='AAPL',
                                     fromdate=datetime.datetime(2020, 1, 1),
                                     todate=datetime.datetime(2021, 1, 1))

    cerebro.adddata(data)
    cerebro.addstrategy(SMABacktest)

    # Set initial cash and commission
    cerebro.broker.set_cash(100000)
    cerebro.broker.setcommission(commission=0.001)

    # Print the starting cash
    print(f"Starting Portfolio Value: {cerebro.broker.getvalue()}")

    # Run the backtest
    cerebro.run()

    # Print the final cash
    print(f"Ending Portfolio Value: {cerebro.broker.getvalue()}")
- 👋 Hi, I’m @Asilbekshox
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Asilbekshox/Asilbekshox is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
