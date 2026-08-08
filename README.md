import datetime
import time
import pandas as pd
import requests
import yfinance as yf

# ==========================================
# CONFIGURATION
# ==========================================
BOT_TOKEN = "8442827788:AAFrMrr6OB5m1Oy64U63O1KNM0eyKqIaeAY"
CHAT_ID = "5983230232"
SYMBOL = "GC=F"  # Gold Futures (XAU/USD)
CHECK_INTERVAL_SECONDS = 3600  # ពិនិត្យ និងផ្ញើ Signal រៀងរាល់ ១ ម៉ោងម្តង (3600s)


def send_telegram(message):
    """មុខងារផ្ញើសារទៅ Telegram"""
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage"
    payload = {"chat_id": CHAT_ID, "text": message, "parse_mode": "HTML"}
    try:
        requests.post(url, json=payload, timeout=10)
    except Exception as e:
        print(f"Error sending Telegram message: {e}")


def is_market_open():
    """ពិនិត្យមើលថាទីផ្សារ Gold បើក ឬបិទ (បិទថ្ងៃសៅរ៍ និងអាទិត្យ)"""
    now = datetime.datetime.utcnow()
    # 0 = Monday, 5 = Saturday, 6 = Sunday
    weekday = now.weekday()

    # ផ្សារ Gold បិទនៅចុងសប្តាហ៍ (Weekend)
    if weekday == 5 or (
        weekday == 4 and now.hour >= 21
    ):  # ថ្ងៃសៅរ៍ ឬ យប់ថ្ងៃសុក្រ
        return False
    if weekday == 6 and now.hour < 22:  # ថ្ងៃអាទិត្យមុនម៉ោងបើក
        return False

    return True


def analyze_gold_market():
    """វិភាគទីផ្សារ Gold និងបង្កើត Report"""
    # ១. ពិនិត្យម៉ោងទីផ្សារ
    if not is_market_open():
        print("Market is currently CLOSED. Skipping analysis...")
        return

    # ២. ទាញយកទិន្នន័យ Candle 1H ចំនួន 100 Candles ចុងក្រោយ
    df = yf.download(tickers=SYMBOL, period="7d", interval="1h", progress=False)

    if df.empty or len(df) < 50:
        print("Unable to fetch data.")
        return

    # រៀបចំ Columns ឱ្យស្រួលប្រើ
    if isinstance(df.columns, pd.MultiIndex):
        df.columns = df.columns.get_level_values(0)

    # គណនា Indicators
    df["SMA_20"] = df["Close"].rolling(window=20).mean()
    df["SMA_50"] = df["Close"].rolling(window=50).mean()
    df["ATR"] = (
        df["High"] - df["Low"]
    )  # គណនា Average True Range លក្ខណៈសាមញ្ញ

    current_price = float(df["Close"].iloc[-1])
    sma_20 = float(df["SMA_20"].iloc[-1])
    sma_50 = float(df["SMA_50"].iloc[-1])

    # គណនា Volatility (ចលនាតម្លៃ)
    recent_volatility = float(df["ATR"].tail(10).mean())

    # ៣. វិភាគថាតើ "អាច TRADE បាន ឬ មិនបាន"
    # បើ Volatility ទាបពេក (Sideway រ៉ិចរ៉ុច) ឬ SMA20 កៀក SMA50 ពេក -> មិនคว រ Trade
    price_sma_diff = abs(sma_20 - sma_50) / current_price * 100

    if price_sma_diff < 0.15:
        market_status = "⚠️ មិនគួរ TRADE ទេ (Market Sideway / គ្មាន Trend ច្បាស់)"
        can_trade = False
    else:
        market_status = "✅ អាច TRADE បាន (Market Has Strong Trend)"
        can_trade = True

    # ៤. វិភាគភាគរយ % ឡើង ឬ ចុះ (Trend Direction)
    high_24h = float(df["High"].tail(24).max())
    low_24h = float(df["Low"].tail(24).min())

    upside_pct = ((high_24h - current_price) / current_price) * 100
    downside_pct = ((current_price - low_24h) / current_price) * 100

    if sma_20 > sma_50:
        trend = "BULLISH (ឡេីង) 📈"
        signal = "BUY 🟢"
        confidence = 80 if can_trade else 50
    else:
        trend = "BEARISH (ចុះ) 📉"
        signal = "SELL 🔴"
        confidence = 80 if can_trade else 50

    # ៥. រៀបចំសារ Telegram
    message = f"""
🤖 <b>AUTO GOLD (XAU/USD) ANALYSIS</b>
----------------------------------
💵 <b>តម្លៃបច្ចុប្បន្ន:</b> ${current_price:.2f}
📊 <b>ស្ថានភាពទីផ្សារ:</b> {market_status}

🔍 <b>ការវិភាគទិសដៅទីផ្សារ:</b>
• Trend ទូទៅ: <b>{trend}</b>
• ឱកាសឡើង (Upside Potential): <b>+{upside_pct:.2f}%</b>
• ឱកាសចុះ (Downside Potential): <b>-{downside_pct:.2f}%</b>

🎯 <b>AI Signal Recommendation:</b>
• សកម្មភាព: <b>{signal}</b>
• កម្រិតទំនុកចិត្ត: <b>{confidence}%</b>
----------------------------------
⏰ <i>រ៉ាន់ស្វ័យប្រវត្តិដោយ AI Server</i>
"""

    # ផ្ញើសារទៅ Telegram
    send_telegram(message)
    print(f"[{datetime.datetime.now()}] Signal sent successfully!")


# ==========================================
# MAIN LOOP (រ៉ាន់រហូតដោយស្វ័យប្រវត្តិ)
# ==========================================
if __name__ == "__main__":
    print("🚀 Auto Gold Trading Analysis Bot Started...")
    send_telegram("🚀 <b>System Started:</b> ប្រព័ន្ធវិភាគ Gold ស្វ័យប្រវត្តិចាប់ផ្តើមដំណើរការ!")

    while True:
        try:
            analyze_gold_market()
        except Exception as e:
            print(f"Error occurred: {e}")

        # រង់ចាំរហូតដល់គ្រប់ម៉ោងកំណត់ (ឧទាហរណ៍ 3600 វិនាទី = 1 ម៉ោង) ទើបវិភាគម្តងទៀត
        time.sleep(CHECK_INTERVAL_SECONDS)
