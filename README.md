# harshavardhan ai and ml crpto,oil prices 
!pip install requests
import requests
url = ('https://api.coingecko.com/api/v3/coins/markets?vs_currency=inr&per_page=250&order=market_cap_desc&page=1&sparkline=False')
response = requests.get(url)
response
response.json()
import requests
import time

records = []

# Page 4 an 5
for page_num in range(4, 6):
    # String formatting
    url = f"https://api.coingecko.com/api/v3/coins/markets?vs_currency=inr&per_page=250&order=market_cap_desc&page={page_num}&sparkline=False"

    response = requests.get(url)

    if response.status_code == 200:
        data = response.json()
        records.extend(data) # Records list  add
        print(f"Page {page_num} fetched successfully. Current total records: {len(records)}")
    else:
        print(f"Error fetching page {page_num}: {response.status_code}")


    time.sleep(1)

print(f"\nFinal total records: {len(records)}")
response
import pandas as pd


df = pd.DataFrame(records)


print("DataFrame Shape:", df.shape)
print("\nFirst 5 rows:")
print(df[['id', 'symbol', 'current_price', 'market_cap']].head())
if response.status_code == 200:
  data = response.json()
  records.extend(data)
records
df.info
print(len(records))
response
import pandas as pd
df = pd.DataFrame(records)
df.head()
df.describe
df.head(10)
df.tail(10)
import pandas as pd
df = pd.DataFrame(records)
selected_columns = [
    'id', 'symbol', 'name', 'current_price', 'market_cap',
    'market_cap_rank', 'total_volume', 'circulating_supply',
    'total_supply', 'ath', 'atl', 'last_updated'
]
df_filtered = df[selected_columns].copy()
df_filtered['last_updated'] = pd.to_datetime(df_filtered['last_updated']).dt.date
print(f"Total rows processed: {len(df_filtered)}")
df_filtered
print(df_filtered.isnull().sum())
top_url = "https://api.coingecko.com/api/v3/coins/markets?vs_currency=inr&order=market_cap_desc&per_page=3&page=1&sparkline=False"
top_response = requests.get(top_url)
top_3_real = pd.DataFrame(top_response.json())
top_3_real = top_3_real[selected_columns]
top_3_real['last_updated'] = pd.to_datetime(top_3_real['last_updated']).dt.date

top_3_real
#top 3 coin analysis

import requests
url ="https://api.coingecko.com/api/v3/coins/bitcoin/market_chart?vs_currency=inr&days=365"
response = requests.get(url)
data = response .json()
data
import requests
import pandas as pd
import time

# 1. Top 3 Coin IDs
top_3_ids = ['bitcoin', 'ethereum', 'tether']
combined_history = []

print("Starting Historical Data Fetch...")

for coin in top_3_ids:
    # "We are creating a dynamic URL using string formatting."
    url = f"https://api.coingecko.com/api/v3/coins/{coin}/market_chart?vs_currency=inr&days=365"

    response = requests.get(url)

    if response.status_code == 200:
        data = response.json()
        prices = data['prices'] # List of [timestamp, price]

        for p in prices:
            combined_history.append({
                'coin_id': coin,
                'date': pd.to_datetime(p[0], unit='ms').date(),
                'price_inr': p[1]
            })
        print(f"✅ {coin.capitalize()} data fetched: {len(prices)} days")
    else:
        print(f"❌ Error fetching {coin}: {response.status_code}")

    # API Rate limit irukkuradhala 2 seconds gap vidalaam
    time.sleep(2)

# 2. changing all datas to DataFrame
df_history = pd.DataFrame(combined_history)

# 3. Final check
print(f"\nTotal records in combined history: {len(df_history)}")
print(df_history.sample(10)) # Random-ah 10 rows check panna
import pandas as pd

# 1. Create the DataFrame from the raw list
# prices look like: [[1762041600000, 22137507.45], ...]
df = pd.DataFrame(data['prices'], columns=['timestamp', 'price_inr'])

# 2. Convert 'timestamp' (milliseconds) to readable 'date'
# unit='ms' is CRITICAL here
df['date'] = pd.to_datetime(df['timestamp'], unit='ms')

# 3. (Optional) If you want only the Date (YYYY-MM-DD) without time:
df['date_only'] = df['date'].dt.date

print(df.head())
import requests
import pandas as pd

# 1. API kitta irundhu data fetch panrom
url = "https://api.coingecko.com/api/v3/coins/bitcoin/market_chart?vs_currency=inr&days=365"
response = requests.get(url)
data = response.json() # Inga dhaan 'data' create aagudhu!

# 2. DataFrame-ah mathurom
df = pd.DataFrame(data['prices'], columns=['timestamp', 'price_inr'])

# 3. 'coin_id' column add panrom
df['coin_id'] = 'bitcoin'

# 4. Milliseconds-ah Date-ah mathurom
df['date'] = pd.to_datetime(df['timestamp'], unit='ms').dt.date

# 5. Column order-ah clean panrom
df = df[['coin_id', 'date', 'price_inr']]

print(df.head())
#calculating the oil prices
import pandas as pd

# Now this will work perfectly
oil_url = "https://raw.githubusercontent.com/datasets/oil-prices/main/data/wti-daily.csv"
oil_df = pd.read_csv(oil_url)
oil_df.head()
import pandas as pd
oil_df = pd.read_csv("https://raw.githubusercontent.com/datasets/oil-prices/main/data/wti-daily.csv")
oil_df.head()
import requests
import pandas as pd
import time
from datetime import datetime

# 1. Setup Time (Unix Timestamps)
# Jan 1, 2020
start_date = int(datetime(2020, 1, 1).timestamp())
# Current time
end_date = int(datetime.now().timestamp())

top_3_ids = ['bitcoin', 'ethereum', 'tether']
all_historical_data = []

print(f"Fetching data from 2020-01-01 to today...")

for coin in top_3_ids:
    # Use 'market_chart/range' instead of just 'market_chart'
    url = f"https://api.coingecko.com/api/v3/coins/{coin}/market_chart/range?vs_currency=inr&from={start_date}&to={end_date}"

    response = requests.get(url)

    if response.status_code == 200:
        data = response.json()
        prices = data['prices']

        for p in prices:
            all_historical_data.append({
                'coin_id': coin,
                'date': pd.to_datetime(p[0], unit='ms').date(),
                'price_inr': p[1]
            })
        print(f"✅ {coin.capitalize()} data fetched successfully.")
    else:
        print(f"❌ Error for {coin}: {response.status_code}")

    # 2-second gap to avoid API ban
    time.sleep(2)

# Create Final Table
df_2020_to_now = pd.DataFrame(all_historical_data)
print(f"\nTotal rows collected: {len(df_2020_to_now)}")
print(df_2020_to_now.head())
oil_df.head()
import pandas as pd

# 1. 'Date' column-ah datetime-ah mathurom
oil_df['Date'] = pd.to_datetime(oil_df['Date'])

# 2. 2020 Jan 1-la irundhu filter panrom
start_date = '2020-01-01'
end_date = '2026-04-21' # Today's date

filtered_oil_df = oil_df[(oil_df['Date'] >= start_date) & (oil_df['Date'] <= end_date)]

# 3. Check panna
print(f"Data from 2020 to 2026 collected. Total rows: {len(filtered_oil_df)}")
print(filtered_oil_df.head())
print(filtered_oil_df.tail())
oil_df.head()
oil_df.tail()
import matplotlib.pyplot as plt

# 1. Figure size set panrom (perusa theriya)
plt.figure(figsize=(12, 6))

# 2. Line chart draw panrom
# x-axis la Date, y-axis la Price
plt.plot(filtered_oil_df['Date'], filtered_oil_df['Price'], color='blue', linewidth=1.5)

# 3. Chart details (Title and Labels)
plt.title('Oil Price Trend (2020 - 2026)', fontsize=14)
plt.xlabel('Year', fontsize=12)
plt.ylabel('Price (USD)', fontsize=12)

# 4. Grid lines add panrom (easy-ah read panna)
plt.grid(True, linestyle='--', alpha=0.6)

# 5. Chart-ah display panrom
plt.show()
import yfinance as yf
import pandas as pd

#choose indices
tickers = ["^GSPC", "^IXIC", "^NSEI"]
start_date = "2020-01-01"
end_date = "2026-03-09"
#3. Download historical data
# group_by="ticker" kudutha dhaan namma oru oru index-kum separate-ah data-va pirika mudiyum
stocks_df = yf.download(tickers, start=start_date, end=end_date, group_by="ticker")

# 4. First 5 rows-ah check panna
stocks_df.head()
import yfinance as yf
import pandas as pd

# 1. Tickers and Date Range
tickers = ["^GSPC", "^IXIC", "^NSEI"]
start_date = "2020-01-01"
end_date = "2026-03-09"

all_stocks_list = []

print("Downloading stock data...")

for ticker in tickers:
    # Oru oru ticker-ah separate-ah download panrom
    data = yf.download(ticker, start=start_date, end=end_date)

    # Data irukanu check panrom
    if not data.empty:
        # Reset index panna dhaan 'Date' oru column-ah varum
        temp_df = data.reset_index()

        # 'ticker' column add panrom
        temp_df['ticker'] = ticker

        # Nammaku thevaiyana columns mattum edukirom
        # 'Close' price dhaan standard analysis-ku use pannuvom
        temp_df = temp_df[['ticker', 'Date', 'Close']]

        all_stocks_list.append(temp_df)
        print(f"✅ {ticker} data collected.")

# 2. Concat all dataframes
final_stocks_df = pd.concat(all_stocks_list, ignore_index=True)

# 3. Final Check
print("\nFinal Concat Result:")
print(final_stocks_df.head())
print(f"\nTotal rows: {len(final_stocks_df)}")
from sqlalchemy import create_engine

# Create the SQLite engine
engine = create_engine('sqlite:///finance_data.db')

print("Database engine created successfully.")
from sqlalchemy import create_engine

# Engine create panrom
engine = create_engine('sqlite:///finance_data.db')

# Oru oru variable-ah irukanu check panni push panrom
if 'df_market' in locals():
    df_market.to_sql('cryptocurrencies', con=engine, if_exists='replace', index=False)
    print("✅ Table 'cryptocurrencies' created.")
else:
    print("❌ Error: 'df_market' define pannala. Andha cell-ah thirumba run pannunga.")

if 'df_history' in locals():
    df_history.to_sql('crypto_prices', con=engine, if_exists='replace', index=False)
    print("✅ Table 'crypto_prices' created.")

if 'filtered_oil_df' in locals():
    filtered_oil_df.to_sql('oil_prices', con=engine, if_exists='replace', index=False)
    print("✅ Table 'oil_prices' created.")

if 'final_stocks_df' in locals():
    final_stocks_df.to_sql('stock_prices', con=engine, if_exists='replace', index=False)
    print("✅ Table 'stock_prices' created.")
import pandas as pd
from sqlalchemy import create_engine

# Database engine connect panrom
engine = create_engine('sqlite:///finance_data.db')

# Enthendha tables create aagirukku nu list pakkurom
from sqlalchemy import inspect
inspector = inspect(engine)
print("Database-la irukkura tables:", inspector.get_table_names())

# Table-la data irukkanu check panna (Example: Oil Prices)
try:
    check_oil = pd.read_sql("SELECT COUNT(*) as total_rows FROM oil_prices", con=engine)
    print(f"\nOil Prices Table-la {check_oil['total_rows'][0]} rows irukku.")
except:
    print("\nOil Prices table innum create aagala.")
#crypto currency sql quries
import pandas as pd
from sqlalchemy import create_engine

# Database-ah connect panrom
engine = create_engine('sqlite:///finance_data.db')

# Table-la irukura ellathaiyum paaka
query = "SELECT * FROM oil_prices"
df = pd.read_sql(query, con=engine)

# Data-va display panrom
print(df.head()) # Modhal 5 rows
# Table structure-ah paaka
query = "PRAGMA table_info(oil_prices)"
columns_info = pd.read_sql(query, con=engine)
print(columns_info)
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine('sqlite:///finance_data.db')

# Table structure-ah check panna
query = "PRAGMA table_info(cryptocurrencies)"
columns_info = pd.read_sql(query, con=engine)
print(columns_info[['name', 'type']])
import pandas as pd
from sqlalchemy import create_engine

# 1. Database engine-ah thirumba connect panrom
engine = create_engine('sqlite:///finance_data.db')

# 2. SQL query-ah Python function ulla kudukkarom
query = "SELECT coin_id, price_inr FROM cryptocurrencies ORDER BY price_inr DESC LIMIT 3"

# 3. Query-ah run panni result-ah edukkarom
top_coins_df = pd.read_sql(query, con=engine)

# 4. Result-ah display panrom
print(top_coins_df)
# 1. Top 3 cryptocurrencies by market cap
query1 = "SELECT coin_id, market_cap FROM cryptocurrencies ORDER BY market_cap DESC LIMIT 3"

# 2. Circulating supply exceeds 90% of total supply
query2 = "SELECT coin_id FROM cryptocurrencies WHERE circulating_supply > (0.9 * total_supply)"

# 3. Coins within 10% of their All-Time High (ATH)
query3 = "SELECT coin_id FROM cryptocurrencies WHERE current_price >= (ath * 0.9)"

# 4. Average market cap rank of coins with volume above $1B
# ($1B is approx 83,000,000,000 INR)
query4 = "SELECT AVG(market_cap_rank) FROM cryptocurrencies WHERE total_volume > 83000000000"

# 5. Most recently updated coin
query5 = "SELECT coin_id, last_updated FROM cryptocurrencies ORDER BY last_updated DESC LIMIT 1"
# 1. Highest daily price of Bitcoin in the last 365 days
query_h1 = """SELECT MAX(price_inr) FROM crypto_prices
              WHERE coin_id = 'bitcoin' AND date >= DATE('now', '-365 days')"""

# 2. Average daily price of Ethereum in the past 1 year
query_h2 = """SELECT AVG(price_inr) FROM crypto_prices
              WHERE coin_id = 'ethereum' AND date >= DATE('now', '-1 year')"""

# 3. Daily price trend of Bitcoin in January 2025
query_h3 = """SELECT date, price_inr FROM crypto_prices
              WHERE coin_id = 'bitcoin' AND date LIKE '2025-01%'"""

# 4. Coin with the highest average price over 1 year
query_h4 = """SELECT coin_id, AVG(price_inr) as avg_p FROM crypto_prices
              WHERE date >= DATE('now', '-1 year') GROUP BY coin_id ORDER BY avg_p DESC LIMIT 1"""

# 5. % change in Bitcoin price between Sep 2024 and Sep 2025
# (Requires subqueries to find start and end prices)
query_h5 = """
SELECT ((end_p - start_p) / start_p) * 100 as percentage_change
FROM (SELECT price_inr as start_p FROM crypto_prices WHERE coin_id = 'bitcoin' AND date LIKE '2024-09%' LIMIT 1),
     (SELECT price_inr as end_p FROM crypto_prices WHERE coin_id = 'bitcoin' AND date LIKE '2025-09%' LIMIT 1)
"""
df.head()
df.tail()
df.info()
# 1. Find the highest oil price in the last 5 years
query_o1 = "SELECT MAX(Price) FROM oil_prices WHERE Date >= DATE('now', '-5 years')"

# 2. Get the average oil price per year
query_o2 = "SELECT strftime('%Y', Date) as Year, AVG(Price) FROM oil_prices GROUP BY Year"

# 3. Show oil prices during COVID crash (March–April 2020)
query_o3 = "SELECT * FROM oil_prices WHERE Date BETWEEN '2020-03-01' AND '2020-04-30'"

# 4. Find the lowest price of oil in the last 10 years
query_o4 = "SELECT MIN(Price) FROM oil_prices WHERE Date >= DATE('now', '-10 years')"

# 5. Calculate the volatility (max-min difference per year)
query_o5 = "SELECT strftime('%Y', Date) as Year, (MAX(Price) - MIN(Price)) as Volatility FROM oil_prices GROUP BY Year"
# 1. Get all stock prices for a given ticker (Example: '^NSEI')
query_s1 = "SELECT * FROM stock_prices WHERE Ticker = '^NSEI'"

# 2. Find the highest closing price for NASDAQ (^IXIC)
query_s2 = "SELECT MAX(Close) FROM stock_prices WHERE Ticker = '^IXIC'"

# 3. List top 5 days with highest price difference (high - low) for S&P 500 (^GSPC)
query_s3 = "SELECT Date, (High - Low) as Diff FROM stock_prices WHERE Ticker = '^GSPC' ORDER BY Diff DESC LIMIT 5"

# 4. Get monthly average closing price for each ticker
query_s4 = "SELECT Ticker, strftime('%Y-%m', Date) as Month, AVG(Close) FROM stock_prices GROUP BY Ticker, Month"

# 5. Get average trading volume of NSEI in 2024
query_s5 = "SELECT AVG(Volume) FROM stock_prices WHERE Ticker = '^NSEI' AND Date LIKE '2024%'"
!pip install pycoingecko
#fetching historical
import pandas as pd
from pycoingecko import CoinGeckoAPI

cg = CoinGeckoAPI()
# Bitcoin historical data-va fetch panrom
data = cg.get_coin_market_chart_by_id(id='bitcoin', vs_currency='inr', days='365')
prices = data['prices']

# DataFrame-ah mathi 'df_history' create panrom
df_history = pd.DataFrame(prices, columns=['timestamp', 'price_inr'])
df_history['date'] = pd.to_datetime(df_history['timestamp'], unit='ms').dt.strftime('%Y-%m-%d')
df_history['coin_id'] = 'bitcoin'
df_history = df_history[['date', 'coin_id', 'price_inr']]

print("✅ Step 2 Mudinjidhu: df_history ready-ah irukku.")
#sending table to data base
from sqlalchemy import create_engine
engine = create_engine('sqlite:///finance_data.db')

# Table-ah database-la create panrom
df_history.to_sql('crypto_prices', con=engine, if_exists='replace', index=False)
print("✅ Step 3 Mudinjidhu: 'crypto_prices' table create aagiduchu!")
#satisfing join quries
!pip install pycoingecko
import pandas as pd
from pycoingecko import CoinGeckoAPI
from sqlalchemy import create_engine

# 1. Engine setup
engine = create_engine('sqlite:///finance_data.db')

# 2. Re-fetch and Push Crypto History
cg = CoinGeckoAPI()
data = cg.get_coin_market_chart_by_id(id='bitcoin', vs_currency='inr', days='365')
df_history = pd.DataFrame(data['prices'], columns=['timestamp', 'price_inr'])
df_history['date'] = pd.to_datetime(df_history['timestamp'], unit='ms').dt.strftime('%Y-%m-%d')
df_history['coin_id'] = 'bitcoin'
df_history.to_sql('crypto_prices', con=engine, if_exists='replace', index=False)

# 3. Re-push Oil and Stocks (Unga kaila irukra DataFrames-ah use pannunga)
# df_oil matrum final_stocks_df unga notebook-la munnadiye irukkum
if 'df_oil' in locals():
    df_oil.to_sql('oil_prices', con=engine, if_exists='replace', index=False)
if 'final_stocks_df' in locals():
    final_stocks_df.to_sql('stock_prices', con=engine, if_exists='replace', index=False)

print("✅ Tables Re-created: crypto_prices, oil_prices, stock_prices")
#satisfing join queries
import pandas as pd
from sqlalchemy import create_engine

# 1. Database engine setup
engine = create_engine('sqlite:///finance_data.db')

# 2. Oil matrum Stock DataFrames-ah database-ku anupunga
# Indha variables (df_oil, final_stocks_df) unga notebook-la munnadiye define aagirukkanum
if 'df_oil' in locals():
    df_oil.to_sql('oil_prices', con=engine, if_exists='replace', index=False)
    print("✅ oil_prices table ready!")

if 'final_stocks_df' in locals():
    final_stocks_df.to_sql('stock_prices', con=engine, if_exists='replace', index=False)
    print("✅ stock_prices table ready!")

# 3. Crypto table-ah push pannunga (Screenshot 122252 logic-padi)
if 'df_history' in locals():
    df_history.to_sql('crypto_prices', con=engine, if_exists='replace', index=False)
    print("✅ crypto_prices table ready!")
else:
    print("❌ Error: df_history define aagala. Crypto fetch cell-ah run pannunga.")
import pandas as pd
from sqlalchemy import create_engine

# 1. Database Connection
engine = create_engine('sqlite:///finance_data.db')

# 2. Join Query 1: BTC vs Oil (Corrected Column: Close)
q1 = """
SELECT
    (SELECT AVG(price_inr) FROM crypto_prices WHERE date LIKE '2025%') as avg_btc_inr,
    (SELECT AVG(Close) FROM oil_prices WHERE Date LIKE '2025%') as avg_oil_price
"""

# 3. Join Query 2: BTC vs S&P 500
q2 = """
SELECT c.date, c.price_inr as btc_price, s.Close as sp500_close
FROM crypto_prices c
JOIN stock_prices s ON c.date = s.Date
WHERE s.Ticker = '^GSPC'
ORDER BY c.date DESC LIMIT 5
"""

try:
    print("Join Query 1 (BTC vs Oil) Results:")
    print(pd.read_sql(q1, con=engine))

    print("\nJoin Query 2 (BTC vs S&P 500) Results:")
    print(pd.read_sql(q2, con=engine))

    print("\n✅ PROJECT COMPLETE: All Join Queries Satisfied!")
except Exception as e:
    print(f"❌ Error: {e}")
