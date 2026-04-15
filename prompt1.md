Act as a senior backend engineer.

Build a Python pipeline to process a CoinDCX Excel file and generate a trade summary using LLM.

Requirements:

1. Input
- Excel file path
- Sheet name is always: "Instant Orders"

2. Step 1 — Read Raw Sheet
- Read Excel using pandas with header=None
- Use openpyxl engine
- Do not assume header is first row

3. Step 2 — Detect Header Row (LLM-based)
- Take first N rows (configurable, default 15)
- Convert preview to string table
- Send to LLM
- LLM must return JSON:
  {
    "header_row_index": number,
    "confidence": number,
    "reasoning": string
  }
- Extract and validate JSON safely

4. Step 3 — Load Clean Data
- Re-read Excel using detected header_row_index as header
- Remove:
  - fully empty rows
  - rows where "Crypto" column is null
- Reset index

5. Step 4 — Prepare Data for Summary
- Convert only first 20 rows to CSV string (no index)
- Do not send full dataset to LLM

6. Step 5 — Generate Summary (LLM-based)
- Provide:
  - trimmed CSV data
  - computed stats (see below)
- LLM should return:
  - total trades
  - buy vs sell count
  - total investment (buy side)
  - average buy price
  - key insights

7. Step 6 — Compute Stats (deterministic, not LLM)
- total_trades = len(df)
- buy_trades = count where Side == "Buy"
- sell_trades = count where Side == "Sell"
- total_buy_amount = sum of "Gross Amount Paid/Received by the user(in INR)" where Buy
- avg_buy_price = mean of "Avg Buying/Selling Price(in INR)" where Buy

8. Constraints
- Do not re-read file unnecessarily
- Do not pass full dataframe to LLM
- Handle invalid/missing JSON from LLM
- Keep functions modular:
  - detect_header_with_llm
  - load_clean_dataframe
  - compute_trade_stats
  - summarize_with_llm

9. Output
- Final function should return:
  {
    "stats": {...},
    "summary": "...",
    "header_detection": {...}
  }

10. Code Quality
- Clean, readable, production-ready
- Proper error handling
- No redundant logic
- No hardcoding except sheet name
- Minimal comments, only where needed

Tech Stack:
- Python
- pandas
- openai client
