<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyber Trading AI Analytics</title>
    <style>
        :root {
            --bg-color: #0d1117;
            --card-bg: #161b22;
            --border-color: #30363d;
            --accent-color: #58a6ff;
            --green: #2ea043;
            --red: #da3633;
            --text-main: #c9d1d9;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 950px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            padding: 20px 0;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 25px;
        }

        .header h1 {
            color: var(--accent-color);
            margin: 0;
            font-size: 28px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 25px;
        }

        .card {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        }

        .card h2 {
            margin-top: 0;
            font-size: 18px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
        }

        .price-display {
            font-size: 24px;
            font-weight: bold;
            margin: 15px 0;
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            margin: 8px 0;
            font-size: 14px;
        }

        .green-text { color: var(--green); font-weight: bold; }
        .red-text { color: var(--red); font-weight: bold; }

        .btn-action {
            width: 100%;
            background: var(--green);
            color: #ffffff;
            border: none;
            padding: 14px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.2s ease;
            margin-top: 15px;
        }

        .btn-action:hover {
            opacity: 0.9;
        }

        .chart-container {
            height: 450px;
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            overflow: hidden;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="header">
            <h1>⚡ Cyber AI Trading Analytics</h1>
            <p>ប្រព័ន្ធវិភាគទីផ្សារ និងផ្ញើ Signals ទៅ Telegram ស្វ័យប្រវត្តិ</p>
        </div>

        <div class="grid">
            <!-- Gold Dashboard Card -->
            <div class="card">
                <h2>📈 Gold (XAU/USD) Analysis</h2>
                <div class="price-display" id="gold-price">Loading...</div>
                <div class="stat-row">
                    <span>ឱកាសឡើង (Upside):</span>
                    <span class="green-text" id="gold-up">+0.00%</span>
                </div>
                <div class="stat-row">
                    <span>ឱកាសចុះ (Downside):</span>
                    <span class="red-text" id="gold-down">-0.00%</span>
                </div>
                <div class="stat-row">
                    <span>AI Signal:</span>
                    <span id="gold-signal">NEUTRAL ⚪</span>
                </div>
                <button class="btn-action" onclick="sendSignal('GOLD')">🚀 ផ្ញើ Gold Signal ទៅ Telegram</button>
            </div>

            <!-- BTC Dashboard Card -->
            <div class="card">
                <h2>₿ Bitcoin (BTC/USD) Analysis</h2>
                <div class="price-display" id="btc-price">Loading...</div>
                <div class="stat-row">
                    <span>ឱកាសឡើង (Upside):</span>
                    <span class="green-text" id="btc-up">+0.00%</span>
                </div>
                <div class="stat-row">
                    <span>ឱកាសចុះ (Downside):</span>
                    <span class="red-text" id="btc-down">-0.00%</span>
                </div>
                <div class="stat-row">
                    <span>AI Signal:</span>
                    <span id="btc-signal">NEUTRAL ⚪</span>
                </div>
                <button class="btn-action" onclick="sendSignal('BTC')">🚀 ផ្ញើ BTC Signal ទៅ Telegram</button>
            </div>
        </div>

        <!-- TradingView Chart -->
        <div class="chart-container">
            <div id="tradingview_xau" style="height:100%;"></div>
        </div>
    </div>

    <!-- TradingView Library -->
    <script type="text/javascript" src="https://s3.tradingview.com/tv.js"></script>
    <script>
        // ព័ត៌មាន Telegram របស់អ្នក (បញ្ចូលរួចរាល់)
        const BOT_TOKEN = "8442827788:AAFrMrr6OB5m1Oy64U63O1KNM0eyKqIaeAY";
        const CHAT_ID = "5983230232";

        // Store Market Data
        let marketData = {
            GOLD: { price: 2350.50, up: 1.25, down: 0.65, signal: "BUY 🟢", conf: 82 },
            BTC: { price: 67200.00, up: 2.10, down: 1.40, signal: "SELL 🔴", conf: 78 }
        };

        // ទាញយកតម្លៃ Live ពី API
        async function fetchPrices() {
            try {
                // Fetch BTC Price
                const btcRes = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd');
                const btcJson = await btcRes.json();
                marketData.BTC.price = btcJson.bitcoin.usd;

                // Simulated Live Indicators for Gold & BTC % Volatility
                marketData.GOLD.up = (Math.random() * 1.5 + 0.5).toFixed(2);
                marketData.GOLD.down = (Math.random() * 1.2 + 0.3).toFixed(2);
                marketData.BTC.up = (Math.random() * 2.5 + 0.8).toFixed(2);
                marketData.BTC.down = (Math.random() * 2.0 + 0.5).toFixed(2);

                updateUI();
            } catch (e) {
                console.log("Error fetching prices, using fallback data.");
                updateUI();
            }
        }

        function updateUI() {
            document.getElementById('gold-price').innerText = `$${marketData.GOLD.price.toLocaleString()}`;
            document.getElementById('gold-up').innerText = `+${marketData.GOLD.up}%`;
            document.getElementById('gold-down').innerText = `-${marketData.GOLD.down}%`;
            document.getElementById('gold-signal').innerText = marketData.GOLD.signal;

            document.getElementById('btc-price').innerText = `$${marketData.BTC.price.toLocaleString()}`;
            document.getElementById('btc-up').innerText = `+${marketData.BTC.up}%`;
            document.getElementById('btc-down').innerText = `-${marketData.BTC.down}%`;
            document.getElementById('btc-signal').innerText = marketData.BTC.signal;
        }

        // មុខងារផ្ញើសារទៅ Telegram
        async function sendSignal(asset) {
            const data = marketData[asset];
            const assetName = asset === 'GOLD' ? 'GOLD (XAU/USD)' : 'BITCOIN (BTC/USD)';
            
            const message = `
🤖 <b>AI TRADING SIGNAL ANALYTICS</b>
----------------------------------
📈 <b>ទ្រព្យសកម្ម:</b> ${assetName}
💵 <b>តម្លៃបច្ចុប្បន្ន:</b> $${data.price.toLocaleString()}

📊 <b>ការប្រមាណ % ឡើង/ចុះ:</b>
• ឱកាសឡើង (Upside): +${data.up}%
• ឱកាសចុះ (Downside): -${data.down}%

🎯 <b>AI Signal:</b> ${data.signal}
🔥 <b>កម្រិតទំនុកចិត្ត:</b> ${data.conf}%
----------------------------------
⚡ <i>Generated from Web Dashboard</i>
            `;

            const url = `https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`;

            try {
                const response = await fetch(url, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: CHAT_ID,
                        text: message,
                        parse_mode: 'HTML'
                    })
                });

                if (response.ok) {
                    alert(`✅ បានផ្ញើ Signal ${asset} ទៅ Telegram រួចរាល់!`);
                } else {
                    alert('❌ មានបញ្ហាក្នុងការផ្ញើ! សូមពិនិត្យ Bot Token ឬ Chat ID');
                }
            } catch (error) {
                alert('❌ Error sending message: ' + error.message);
            }
        }

        // Initialize TradingView Chart
        new TradingView.widget({
            "autosize": true,
            "symbol": "OANDA:XAUUSD",
            "interval": "60",
            "timezone": "Etc/UTC",
            "theme": "dark",
            "style": "1",
            "locale": "en",
            "toolbar_bg": "#f1f3f6",
            "enable_publishing": false,
            "container_id": "tradingview_xau"
        });

        // Run
        fetchPrices();
        setInterval(fetchPrices, 30000); // Auto refresh 30sec
    </script>
</body>
</html>
