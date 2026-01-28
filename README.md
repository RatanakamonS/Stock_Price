## ดาวน์โหลด ^GSPC แล้ว export CSV

import yfinance as yf
import pandas as pd

start = "2022-11-01"
end   = "2025-10-31"

gspc = yf.download("^GSPC", start=start, end=end, auto_adjust=False, progress=False)
gspc.to_csv("gspc_ohlcv.csv", index=True)
print(gspc.head())

## คำนวณ market return จาก Github (simple return จาก Adj Close)
import pandas as pd

### ใช้ RAW link
url = "https://raw.githubusercontent.com/RatanakamonS/Stock_Price/main/gspc_ohlcv.csv"

### ไฟล์มี 2 แถวบน ไม่ใช้จึงข้ามไปแถว1 ถึง2 ไป(Ticker..., Date,,,,)
gspc = pd.read_csv(url, skiprows=[1, 2])

### คอลัมน์แรกในไฟล์ชื่อ "Price" แต่จริง ๆ คือ Date จึง rename ก่อน
gspc = gspc.rename(columns={gspc.columns[0]: "Date"})

### ตั้ง Date เป็น index
gspc["Date"] = pd.to_datetime(gspc["Date"])
gspc = gspc.set_index("Date")

### คำนวณ market return (simple return) จาก Adj Close
gspc["mkt_ret"] = gspc["Adj Close"].pct_change()

out = gspc[["Adj Close", "mkt_ret"]].dropna()
out.to_csv("gspc_market_return.csv", index=True)

print(out.head())
print("rows:", len(out))
