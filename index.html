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
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 15px;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            padding: 15px 0;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 20px;
        }

        .header h1 {
            color: var(--accent-color);
            margin: 0 0 5px 0;
            font-size: 22px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .header p {
            margin: 0;
            font-size: 13px;
            color: #8b949e;
        }

        .card {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 16px rgba(0,0,0,0.4);
            margin-bottom: 20px;
        }

        .card h2 {
            margin-top: 0;
            font-size: 18px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 12px;
            color: #f0f6fc;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .price-display {
            font-size: 32px;
            font-weight: bold;
            color: #ffffff;
            margin: 15px 0;
            text-align: center;
        }

        .status-box {
            background: #21262d;
            border-radius: 8px;
            padding: 10px;
            text-align: center;
            font-size: 14px;
            margin-bottom: 15px;
            border: 1px solid var(--border-color);
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            font-size: 15px;
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
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s ease;
            margin-top: 10px;
        }

        .btn-action:active {
            opacity: 0.8;
        }

        .chart-container {
            height: 400px;
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            overflow: hidden;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="header">
            <h1>⚡ Gold AI Market Analytics</h1>
            <p>ប្រព័ន្ធវិភាគទីផ្សារ និង ផ្ញើ Signals ស្វ័យប្រវត្តិទៅ Telegram</p>
        </div>

        <!-- Gold Analytics Card -->
        <div class="card">
            <h2>📈 XAU/USD (Gold) Analytics</h2>
            
            <div class="price-display" id="gold-price">Loading...</div>

            <div class="status-box">
                ស្ថានភាពទីផ្សារ: <b id="gold-status" style="color: var(--accent-color);">-</b>
            </div>

            <div class="stat-row">
                <span>Trend ទូទៅ:</span>
                <span id="gold-trend" style="font-weight: bold;">-</span>
            </div>
            <div class="stat-row">
                <span>AI Signal Recommendation:</span>
                <span id="gold-signal" style="font-weight: bold;">-</span>
            </div>
            <div class="stat-row">
                <span>ឱកាសឡើង (Upside):</span>
                <span class="green-text" id="gold-up">+0.00%</span>
            </div>
            <div class="stat-row">
                <span>ឱកាសចុះ (Downside):</span>
                <span class="red-text" id="gold-down">-0.00%</span>
            </div>

            <button class="btn-action" onclick="fetchAnalysis()">🚀 វិភាគឡើងវិញ & ផ្ញើ Signal ទៅ Telegram</button>
        </div>

        <!-- TradingView Chart Widget -->
        <div class="chart-container">
            <div id="tradingview_gold" style="height:100%;"></div>
        </div>
    </div>

    <!-- TradingView Library -->
    <script type="text/javascript" src="https://s3.tradingview.com/tv.js"></script>
    <script>
        // URL API របស់ PythonAnywhere របស់អ្នក
        const API_URL = "https://Ratry.pythonanywhere.com/api/analyze";

        async function fetchAnalysis() {
            document.getElementById('gold-price').innerText = "Analyzing...";
            try {
                const response = await fetch(API_URL);
                const data = await response.json();

                document.getElementById('gold-price').innerText = `$${data.price.toLocaleString()}`;
                document.getElementById('gold-status').innerText = data.status;
                document.getElementById('gold-trend').innerText = data.trend;
                document.getElementById('gold-signal').innerText = data.signal;
                document.getElementById('gold-up').innerText = `+${data.upside}%`;
                document.getElementById('gold-down').innerText = `-${data.downside}%`;
            } catch (error) {
                document.getElementById('gold-price').innerText = "Error!";
                alert("មិនអាចភ្ជាប់ទៅ Server របស់ PythonAnywhere បានទេ! ៖ " + error.message);
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
            "hide_legend": true,
            "container_id": "tradingview_gold"
        });

        // ដំណើរការវិភាគភ្លាមៗពេលបើក Web
        fetchAnalysis();
    </script>
</body>
</html>
