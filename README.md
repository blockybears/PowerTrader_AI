# PowerTrader AI (CCXT Fork)

This is a fork of the original **[PowerTrader_AI project]([https://python.org](https://github.com/garagesteve1155/PowerTrader_AI))**. This version has been modified to remove the dependency on Robinhood and now uses the **CCXT library**, enabling it to connect to over 100 different cryptocurrency exchanges like KuCoin, Binance, Coinbase, and more.

---

## Original Project Description

PowerTrader_AI is a fully automated crypto trading bot powered by a custom price prediction AI and a structured/tiered DCA system.

> “It’s an instance-based (kNN/kernel-style) predictor with online per-instance reliability weighting, used as a multi-timeframe trading signal.” - ChatGPT on the type of AI used in this trading bot.

When training for a coin, the AI analyzes the entire price history on multiple timeframes, saving each pattern it sees and what happened on the next candle. It uses these saved patterns to generate a predicted candle by taking a weighted average of the closest matches in memory to the current pattern. This process is repeated for each timeframe from 1 hour to 1 week. The low and high prices from these predicted candles are used as trading signals.

After a candle closes, the AI checks its prediction accuracy and adjusts the weight of the "memory patterns" used, improving future predictions.

### Trading Strategy

-   **Trade Entry**: A trade is initiated when the ask price for a coin drops below at least 3 of the AI's predicted low prices for the current candle across all timeframes.
-   **DCA (Dollar-Cost Averaging)**: The bot will DCA based on either crossing subsequent AI prediction levels or a hardcoded drawdown percentage, whichever comes first. It allows a maximum of 2 DCAs within a rolling 24-hour window to manage risk.
-   **Selling**: The bot uses a trailing profit margin to maximize gains. The initial target is a 5% gain. If a DCA occurs, the target is adjusted to 2.5%. The trailing gap is 0.5%.

---

## Key Changes in This Fork

This version replaces the Robinhood-specific API with the CCXT library.

| Before (Original) | After (This Fork) |
| :--- | :--- |
| Exchange: Robinhood only | Exchange: KuCoin, Binance, Coinbase, etc. (configurable) |
| Credentials: `r_key.txt`, `r_secret.txt` | Credentials: `.env` file for API keys |
| Symbol format: `BTC-USD` | Symbol format: `BTC-USD` (internal), auto-converts to `BTC/USDT` etc. |
| Quote currency: USD | Quote currency: USDT (most exchanges), USD (some exchanges) |

✅ **The core trading logic, neural network, GUI, and trade history format remain the same.**

---

## Setup & First-Time Use

### Step 1 — Install Python

1.  Go to **[python.org](https://python.org)** and download the latest version of Python for your OS.
2.  Run the installer. **IMPORTANT**: On Windows, check the box that says **“Add Python to PATH”**.

### Step 2 — Download The Code

1.  Download the code from this repository, preferably using `git clone`.
2.  Place the files in a dedicated folder, for example: `C:\PowerTraderAI\`.

### Step 3 — Install Dependencies

1.  Open a terminal or Command Prompt in your project folder.
2.  Run the following command to install the required Python libraries:
    ```bash
    pip install -r requirements.txt
    ```

### Step 4 — Configure Your Exchange API Keys

This version uses a `.env` file to store your exchange credentials securely.

1.  **Get API Credentials from Your Exchange:**
    *   Log in to your chosen exchange (e.g., KuCoin, Binance).
    *   Navigate to the API management section.
    *   Create a new API key.
    *   **Set permissions**: Enable **Spot Trading**. **NEVER** enable withdrawals.
    *   Securely save your **API Key**, **API Secret**, and (if applicable) **API Passphrase**.

2.  **Configure PowerTrader:**
    *   Run the Hub:
        ```bash
        python pt_hub.py
        ```
    *   In the Hub window, go to **Settings** → **Exchange Configuration**.
    *   Click **"Setup / Update Exchange"**.
    *   Select your exchange, paste your credentials, and click **"Save to .env"**.

### Step 5 — Train The AI

Training builds the system’s coin “memory” so it can generate signals.

1.  In the Hub, click **Train All**.
2.  Wait for the training process to complete for all your selected coins.

### Step 6 — Start Trading

Once training is complete, click **Start All** in the Hub. The Hub will launch the `pt_thinker.py` and `pt_trader.py` scripts in the correct order.

---

## Troubleshooting

-   **Error: "EXCHANGE_ID not set in .env"**: Ensure the `.env` file exists and contains the correct `EXCHANGE_ID` for your exchange (e.g., `EXCHANGE_ID=kucoin`).
-   **Error: "Invalid API key" or "Authentication failed"**: Double-check your API credentials in the `.env` file. Ensure there are no extra spaces and that the permissions are set correctly on the exchange's website.
-   **Error: "Symbol BTC/USD not found"**: This can happen if your exchange uses a different quote currency (like USDT). The `exchange_interface.py` script attempts to handle this automatically, but if issues persist, check the CCXT documentation for your exchange's symbol format.

---

## Donate to the Original Author

PowerTrader AI is a free and open-source project. To support the original creator, please consider donating:

-   **Cash App**: `$garagesteve`
-   **PayPal**: `@garagesteve`
-   **Patreon**: `patreon.com/MakingMadeEasy`

---

## License

PowerTrader AI is released under the **Apache 2.0** license. You are responsible for any actions the software performs. We are not providing financial advice, and we are not responsible for any financial losses.
