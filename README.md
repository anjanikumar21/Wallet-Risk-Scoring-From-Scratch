📌 Objective
To build a lightweight, explainable and scalable pipeline for risk profiling Ethereum wallets based on their DeFi activity.

📥 1. Data Collection
Source: Covalent API

Protocol: Compound V2

Contracts Queried:

cDAI: 0x5d3a536e4d6dbd6114cc1ead35777bab948e3643

cUSDC: 0x39aa39c021dfbae8fac545936693ac917d5e7563

cETH: 0x4ddc2d193948926d02f9b1fe9e1daa0718270ed5

cWBTC: 0xccf4429db6322d5c611ee964527d42e5d685dd6a

Tools: Python script (requests, pandas) to fetch token transfers for each wallet using the GoldRush (Covalent) API key.

🧪 2. Feature Engineering
For each wallet, the following features were extracted:

Feature	Description
tx_count	Number of Compound transactions
total_value	Total value moved across all interactions
days_since_last_tx	Recency of last transaction (lower = safer)

Features were cleaned, normalized, and saved into wallet_features.csv.

📈 3. Scoring Methodology
Wallets were scored using a custom weighted formula:

Normalization
tx_count and total_value scaled using MinMaxScaler

total_value was log-transformed to control outliers

days_since_last_tx was inverted so that recent = higher score

Final Score (0–1000 scale)
python
Copy
Edit
score = (
    0.4 * tx_score +
    0.3 * val_score +
    0.3 * recent_score
) * 1000
Scores were saved into wallet_risk_scores.csv.

📊 4. Analysis
Please refer to analysis.md for:

Score distribution chart

Behavioral patterns of high vs. low-risk wallets

Comments on data skew and anomalies

🔄 Future Work
Add Aave, MakerDAO activity for more holistic scoring

Use labeled datasets from TrueFi, Goldfinch for supervised ML

Create a dashboard or API to serve scores live