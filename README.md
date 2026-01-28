import yfinance as yf
import pandas as pd

start = "2022-11-01"
end   = "2023-10-31"

gspc = yf.download("^GSPC", start=start, end=end, auto_adjust=False, progress=False)
gspc.to_csv("gspc_ohlcv.csv", index=True)
print(gspc.head())
