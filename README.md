# CoinDCX Trade Analysis Pipeline

A Python-based data processing pipeline that intelligently analyzes cryptocurrency trade data from CoinDCX Excel exports using AI-powered header detection and trade summarization.

## 🚀 Features

- **AI-Powered Header Detection**: Uses LLM to automatically identify header rows in Excel sheets
- **Dynamic Column Mapping**: Intelligently maps column names without hardcoding
- **Trade Statistics**: Computes comprehensive buy/sell statistics and investment metrics
- **Optimized Processing**: Single LLM call for efficient processing
- **Modular Architecture**: Clean, production-ready code with proper error handling

## 📋 Requirements

- Python 3.11+
- CoinDCX Excel export file (with "Instant Orders" sheet)
- GROQ API key for LLM functionality

## 🛠️ Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ishapatelneosoft/assessment-3_4.git
   cd assessment-3_4
   ```

2. **Create virtual environment:**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install pandas groq python-dotenv openpyxl
   ```

4. **Configure environment:**
   Create a `.env` file in the root directory:
   ```
   GROQ_API_KEY=your_groq_api_key_here
   ```

## 📖 Usage

### Basic Usage

```python
from assessment_3_optimized import process_coindcx_file

# Process a CoinDCX Excel file
result = process_coindcx_file("path/to/your/coindcx_export.xlsx")

print("Trade Statistics:", result["stats"])
print("AI Summary:", result["summary"])
```

### Jupyter Notebook

Run the provided notebooks for interactive analysis:
- `assessment_3.ipynb` - Original implementation
- `assessment_3_optimized.ipynb` - Optimized single-LLM-call version
- `assessment_3-4.ipynb` - Development/testing notebook

## 🏗️ Project Structure

```
assessment-3_4/
├── assessment_3.ipynb              # Original implementation notebook
├── assessment_3_optimized.ipynb    # Optimized implementation notebook
├── assessment_3-4.ipynb           # Development and testing notebook
├── prompt1.md                     # Original requirements specification
├── prompt2.md                     # Optimization requirements specification
├── .gitignore                     # Git ignore rules
├── .env                          # Environment variables (not committed)
└── README.md                     # This file
```

## ⚙️ Configuration

### Environment Variables

- `GROQ_API_KEY`: Your GROQ API key for LLM functionality

### LLM Model

The pipeline uses `openai/gpt-oss-120b` model via GROQ for intelligent analysis.

## 🔧 Core Functions

### `process_coindcx_file(file_path)`
Main entry point that processes a CoinDCX Excel file and returns:
- `stats`: Computed trade statistics
- `summary`: AI-generated trade summary
- `header_detection`: LLM header detection results
- `column_mapping`: Dynamic column mapping

### Key Components

1. **Header Detection**: Identifies the actual header row in Excel sheets
2. **Data Cleaning**: Removes empty rows and invalid data
3. **Column Resolution**: Maps semantic column names to actual Excel columns
4. **Statistics Computation**: Calculates buy/sell counts, totals, and averages
5. **AI Summarization**: Generates insights using LLM analysis

## 📊 Output Format

```json
{
  "stats": {
    "total_trades": 150,
    "buy_trades": 75,
    "sell_trades": 75,
    "total_buy_amount": 500000.00,
    "avg_buy_price": 65000.00
  },
  "summary": "AI-generated trade analysis and insights...",
  "header_detection": {
    "header_row_index": 3,
    "confidence": 0.95,
    "reasoning": "Row contains column headers followed by data"
  },
  "column_mapping": {
    "crypto": "Crypto",
    "side": "Side",
    "gross_amount": "Gross Amount Paid/Received by the user(in INR)",
    "avg_price": "Avg Buying/Selling Price(in INR)"
  }
}
```

## 🐛 Error Handling

The pipeline includes robust error handling for:
- Missing or invalid Excel files
- LLM API failures
- Invalid JSON responses
- Missing required columns
- Data validation issues

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- CoinDCX for the trade data format
- GROQ for providing fast LLM inference
- OpenAI for the underlying GPT model

---

**Note**: This is an assessment project demonstrating AI-powered data processing capabilities. Ensure you have proper API access and follow rate limits when using LLM services.</content>
<parameter name="filePath">/home/neosoft/Documents/task-3,4/README.md