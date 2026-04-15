Act as a senior backend engineer.

Optimize the existing CoinDCX trade-summary notebook so it performs the same core processing with only one LLM call.

Requirements:

1. Input
- Excel file path
- Sheet name is always: "Instant Orders"

2. Step 1 - Read Raw Sheet
- Read Excel using pandas with header=None
- Use openpyxl engine
- Do not assume the header is the first row
- Read the Excel sheet once and reuse the raw dataframe for downstream processing

3. Step 2 - Single LLM Call
- Send only a limited raw preview to the LLM
- Default preview configuration:
  - first 15 rows for header detection
  - enough additional rows to include at most 20 possible data rows
- Do not pass the full dataframe to the LLM
- The LLM must return JSON only:
  {
    "header_detection": {
      "header_row_index": number,
      "confidence": number,
      "reasoning": "short explanation"
    },
    "column_mapping": {
      "crypto": "exact crypto/asset column name from the detected header row",
      "side": "exact buy/sell side column name from the detected header row",
      "gross_amount": "exact INR paid/received amount column name from the detected header row",
      "avg_price": "exact average buy/sell price column name from the detected header row"
    },
    "summary": {
      "key_insights": [
        "short qualitative insight based only on the provided sample"
      ],
      "sample_notes": [
        "short note about visible sample patterns or anomalies"
      ]
    }
  }
- The LLM must detect the actual column names from the sheet preview
- Do not hardcode actual Excel column names such as Crypto, Side, gross amount, or average price in the dataframe logic
- The code may use semantic mapping keys only: crypto, side, gross_amount, avg_price
- The LLM must not compute official numeric totals, investments, average prices, or buy/sell counts
- Extract and validate JSON safely
- Raise clear errors for invalid JSON, missing header data, or missing column mappings

4. Step 3 - Load Clean Data
- Build the clean dataframe from the raw dataframe using the detected header_row_index
- Do not re-read the Excel file
- Remove:
  - fully empty rows
  - columns with empty header names
  - rows where the detected crypto column is null or empty
- Reset index

5. Step 4 - Resolve Detected Columns
- Use the LLM-provided column_mapping to locate the dataframe columns
- Prefer exact matches
- Allow safe whitespace/case-insensitive fallback matching
- Raise a clear error if a detected column is missing or ambiguous

6. Step 5 - Compute Stats Deterministically
- total_trades = len(df)
- buy_trades = count where the detected side column value is "Buy"
- sell_trades = count where the detected side column value is "Sell"
- total_buy_amount = sum of the detected gross_amount column where side is "Buy"
- avg_buy_price = mean of the detected avg_price column where side is "Buy"
- Convert numeric columns with pandas.to_numeric(errors="coerce")
- Do not use the LLM for these calculations

7. Step 6 - Build Final Summary
- Build the final summary locally using:
  - deterministic stats
  - qualitative key_insights from the single LLM response
- Numeric values in the final summary must always come from computed stats, not from the LLM
- Keep the output concise and structured

8. Constraints
- Exactly one LLM API call per file processing run
- Do not re-read the file unnecessarily
- Do not pass the full dataframe to the LLM
- Keep the original core data-processing logic intact
- Do not hardcode the actual Excel column names
- Keep functions modular:
  - read_raw_sheet
  - analyze_sheet_with_llm
  - load_clean_dataframe
  - resolve_column_mapping
  - compute_trade_stats
  - build_trade_summary
  - process_coindcx_file

9. Output
- Final function should return:
  {
    "stats": {...},
    "summary": "...",
    "header_detection": {...},
    "column_mapping": {...}
  }

10. Code Quality
- Clean, readable, production-ready
- Proper error handling
- No redundant logic
- No hardcoded local file paths
- Minimal comments, only where needed

Tech Stack:
- Python
- pandas
- Groq client compatible with the existing notebook
