# Systematic Crypto Trader

Systematic trading algorithm for crypto linked futures on the Binance exchange.

## Installation

Please clone this repository.

## Usage

This script requires an active futures Binance account with API and Secret key.

```python
api_key = "INPUT API KEY HERE"
secret_key = "INPUT SECRET KEY HERE"
```

The script comes with a predefined triple SMA strategy. The method defining the strategy (define_strategy) can be adapted as you see fit. A trailing stop loss is used for relatively more sophisticated downside protection while maximising upside potential.

```python
def define_strategy(self):

  data = self.data.copy()

  # Define Strategy Here **************************************************

  data = data[["Close"]].copy()

  data["SMA_S"] = data.Close.rolling(window=self.SMA_S).mean()
  data["SMA_M"] = data.Close.rolling(window=self.SMA_M).mean()
  data["SMA_L"] = data.Close.rolling(window=self.SMA_L).mean()

  data.dropna(inplace=True)

  cond1 = (data.SMA_S > data.SMA_M) & (data.SMA_M > data.SMA_L)
  cond2 = (data.SMA_S < data.SMA_M) & (data.SMA_M < data.SMA_L)

  data["position"] = 0
  data.loc[cond1, "position"] = 1
  data.loc[cond2, "position"] = -1
```

Variables including the underlying, candle stick length, strategy parameters, trade size, leverage and callback rate are all defined outside the class.

```python
symbol = "BTCUSDT"
bar_length = "5m"
sma_s = 10
sma_m = 20
sma_l = 50
units = 0.001
position = 0
leverage = 2
callbackRate = 0.5
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

LICENSE
For more information, please refer to <https://unlicense.org>
