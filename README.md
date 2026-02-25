# Quant Data Validator

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**A Python toolkit for validating financial data quality in quantitative research.**

> ⚠️ **Disclaimer**: This tool is for research and educational purposes only. Always verify data quality before making investment decisions.

---

## 🎯 Why This Matters

As quantitative researchers, we all know: **garbage in, garbage out**.

Data quality issues I've encountered:
- Stock prices showing as `0` on certain dates
- Adjusted prices with 50%+ differences between data sources
- Volume data exceeding market cap
- Missing trading days without warning

This toolkit helps you catch these issues **before** they ruin your backtests.

---

## ✨ Features

### Data Validation
- ✅ **Price Anomaly Detection**: Identify zero, negative, or extreme prices
- ✅ **Return Validation**: Flag suspicious daily returns (>20% moves)
- ✅ **Volume Checks**: Detect abnormal volume patterns
- ✅ **Missing Data**: Find gaps in trading days

### Cross-Source Comparison
- 🔍 Compare Tushare vs EODHD vs your local database
- 📊 Identify discrepancies in adjusted prices
- 🎯 Determine which source is more reliable

### Reporting
- 📈 HTML visualization reports
- 📄 CSV export of anomalies
- 📋 JSON summary for automation

---

## 🚀 Quick Start

### Installation

```bash
git clone https://github.com/xiaobeidev/quant-data-validator.git
cd quant-data-validator
pip install -r requirements.txt
```

### Configuration

Create `config.ini`:

```ini
[tushare]
token = your_tushare_token

[eodhd]
token = your_eodhd_api_key

[local_db]
host = localhost
port = 5432
database = financial_data
```

### Basic Usage

```python
from quant_data_validator import DataValidator

# Initialize validator
validator = DataValidator()

# Validate single stock
validator.check_stock(
    symbol='000001.SZ',
    start_date='2020-01-01',
    end_date='2024-12-31'
)

# Compare multiple data sources
validator.compare_sources(
    symbol='600519.SH',
    sources=['tushare', 'eodhd', 'local_db']
)

# Generate report
validator.generate_report(output='validation_report.html')
```

### Command Line Usage

```bash
# Validate single stock
python -m quant_data_validator check 000001.SZ --start 2020-01-01 --end 2024-12-31

# Batch validation
python -m quant_data_validator batch --file stock_list.txt

# Compare sources
python -m quant_data_validator compare 600519.SH --sources tushare,eodhd
```

---

## 📊 Real-World Findings

### Case 1: Price Discrepancy Between Sources

```python
Stock: 600519.SH (Kweichow Moutai)
Date: 2019-06-28

Tushare (adjusted):     9.22
EODHD (adjusted):       8.03
Local DB (adjusted):    7.98

Difference: Up to 15%!

Root Cause: Different adjustment base dates
```

### Case 2: Zero Price Anomaly

```python
Stock: 000001.SZ (Ping An Bank)
Date: 2021-08-20

Issue: Close price = 0.0
Impact: Invalid return calculations
Action: Data point excluded from analysis
```

### Case 3: Volume Outlier

```python
Stock: 002735.SZ
Date: 2020-03-15

Volume: 50x average daily volume
Check: No major news on that date
Verdict: Likely data error
```

---

## 🔧 Advanced Usage

### Custom Validation Rules

```python
from quant_data_validator import ValidatorConfig

# Configure custom rules
config = ValidatorConfig()
config.max_daily_return = 0.15  # 15% threshold
config.volume_zscore_threshold = 3.0
config.price_min = 0.01

validator = DataValidator(config=config)
```

### Integration with Backtesters

```python
# Pre-process data before backtesting
validator = DataValidator()
clean_data = validator.clean_data(raw_data)

# Use in vnpy
clean_data = validator.validate_for_vnpy(symbol, start, end)

# Use in Backtrader
clean_data = validator.validate_for_backtrader(symbol, start, end)
```

---

## 📚 Documentation

- [Installation Guide](docs/installation.md)
- [API Reference](docs/api.md)
- [Validation Rules](docs/rules.md)
- [Case Studies](docs/cases.md)

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Tushare](https://tushare.pro/) for providing financial data
- [EODHD](https://eodhd.com/) for alternative data source
- [vnpy](https://www.vnpy.com/) community for inspiration

---

## 📬 Contact

- **Email**: xiaobeibot@gmail.com
- **Twitter**: [@xiaobeidev](https://twitter.com/xiaobeidev)
- **知乎**: [小贝极客er](https://www.zhihu.com/people/xiaobei-dev)

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=xiaobeidev/quant-data-validator&type=Date)](https://star-history.com/#xiaobeidev/quant-data-validator&Date)

---

**If this tool helps your research, please consider giving it a star! ⭐**
