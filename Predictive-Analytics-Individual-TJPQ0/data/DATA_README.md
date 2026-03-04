# Dataset Download Instructions

## Dataset Information

**Name:** US Stock Market Dataset  
**Source:** Kaggle  
**Author:** Paul Mooney  
**License:** Database Contents License (DbCL) v1.0  
**Size:** ~430 KB  
**Format:** CSV  

---

## Download Steps

### 1. Visit Kaggle

Go to: **https://www.kaggle.com/datasets/paultimothymooney/stock-market-data**

### 2. Create Kaggle Account (if needed)

- Free account required (no payment)
- Takes approximately 2 minutes
- Use your email to sign up

### 3. Download the Dataset

- Click the blue **"Download"** button on the dataset page
- File will download as: `Stock Market Dataset.csv`
- Size: approximately 430 KB

### 4. Place in Repository

**Option A: Local setup**
- Move `Stock Market Dataset.csv` to this `data/` folder
- Path should be: `data/Stock Market Dataset.csv`

**Option B: Custom location**
- Place file anywhere on your computer
- Update path in notebook Cell 2:
  ```python
  df = pd.read_csv(r'YOUR_CUSTOM_PATH\Stock Market Dataset.csv')
  ```

### 5. Verify Download

Check the file:
- ✓ File size: ~430 KB
- ✓ Format: CSV with headers
- ✓ First date: approximately 2019-02-04
- ✓ Last date: approximately 2024-02-01
- ✓ Columns: 38 total

---

## Dataset Details

### Overview
- **Observations:** 1,242 daily samples
- **Period:** February 2019 - February 2024 (5 years)
- **Frequency:** Daily
- **Columns:** 38 (1 date + 37 features)

### Assets Included

**Stock Indices:**
- S&P 500 (Price, Volume)
- Nasdaq 100 (Price, Volume)

**Technology Stocks:**
- Apple (Price, Volume)
- Tesla (Price, Volume)
- Microsoft (Price, Volume)
- Google (Price, Volume)
- Nvidia (Price, Volume)
- Amazon (Price, Volume)
- Meta/Facebook (Price, Volume)
- Netflix (Price, Volume)

**Traditional Equities:**
- Berkshire Hathaway (Price, Volume)

**Cryptocurrencies:**
- Bitcoin (Price, Volume)
- Ethereum (Price, Volume)

**Commodities:**
- Gold (Price, Volume)
- Silver (Price, Volume)
- Platinum (Price, Volume)
- Crude Oil (Price, Volume)
- Natural Gas (Price, Volume)
- Copper (Price, Volume)

### Column Structure

Each asset has:
- **Price:** Daily closing price
- **Vol.:** Trading volume for that day

Plus:
- **Date:** Date column (YYYY-MM-DD format)

---

## Why Not Included in Repository?

This dataset is **not** included directly in the GitHub repository for several reasons:

1. **File Size:** 430 KB exceeds recommended Git limits for data files
2. **Licensing:** Kaggle Database Contents License requires download from official source
3. **Attribution:** Ensures proper credit to dataset creator (Paul Mooney)
4. **Freshness:** Users get the most up-to-date version from Kaggle
5. **Best Practice:** GitHub repos should not contain large data files

---

## Troubleshooting

### Problem: Can't access Kaggle
**Solution:** Create a free account at kaggle.com (no payment required, takes 2 minutes)

### Problem: Downloaded wrong file
**Solution:** 
- Look specifically for "Stock Market Dataset.csv"
- File size should be ~430 KB
- Should have 38 columns

### Problem: File won't load in notebook
**Solution:**
- Check file path in notebook Cell 2
- Verify file is named exactly: `Stock Market Dataset.csv`
- Check file is in `data/` folder (or update path accordingly)

### Problem: "File not found" error
**Solution:**
```python
# In notebook Cell 2, update to your actual path:
df = pd.read_csv(r'C:\Users\YourName\Downloads\Stock Market Dataset.csv')
```

### Problem: Data looks wrong
**Solution:**
- Re-download from Kaggle (might have downloaded corrupted file)
- Verify file size is ~430 KB
- Check first few rows have dates starting around 2019-02-04

---

## Data Quality Notes

### Missing Values
- Overall missing rate: 1.53%
- Platinum_Vol.: 48.8% missing (dropped in preprocessing)
- Other features: <5% missing (forward-filled in notebook)

### Outliers
- Crypto volumes have extreme spikes (handled in preprocessing)
- Bitcoin volume: up to 220x normal during major events
- Ethereum volume: up to 30x normal during volatility

### Data Integrity
- No duplicate dates (verified in notebook)
- Chronological order (sorted in preprocessing)
- No negative prices or volumes

---

## Citation

If using this dataset in academic work, cite as:

```
Mooney, P. (2024). US Stock Market Dataset. Kaggle.
https://www.kaggle.com/datasets/paultimothymooney/stock-market-data
```

Or in BibTeX format:

```bibtex
@misc{mooney2024stockmarket,
  author = {Mooney, Paul},
  title = {US Stock Market Dataset},
  year = {2024},
  publisher = {Kaggle},
  howpublished = {\url{https://www.kaggle.com/datasets/paultimothymooney/stock-market-data}},
}
```

---

## License Information

**Dataset License:** Database Contents License (DbCL) v1.0

**Permitted Uses:**
- Academic research
- Educational purposes
- Personal projects

**Restrictions:**
- Must attribute to original creator (Paul Mooney)
- Must download from official Kaggle source
- Cannot redistribute modified versions without permission

**Full License:** See Kaggle dataset page for complete terms

---

## Alternative Data Sources

If Kaggle is completely inaccessible (firewall, regional restrictions, etc.):

1. **Contact course staff** for alternative download options
2. **Use VPN** if regional restrictions apply
3. **Ask classmate** to share (with proper attribution)

**Important:** Do NOT upload the CSV file to:
- This GitHub repository
- Other file sharing platforms
- Cloud storage with public links

This violates Kaggle's terms of service.

---

## Expected Data After Download

**File Name:** `Stock Market Dataset.csv`

**First Few Rows:**
```
Date,S&P_500_Price,S&P_500_Vol.,Bitcoin_Price,Bitcoin_Vol.,...
2019-02-04,2724.87,3678040000,3432.91,6146650,...
2019-02-05,2737.70,3420160000,3398.08,5585140,...
2019-02-06,2731.61,3574980000,3427.22,5906830,...
```

**Last Few Rows:**
```
Date,S&P_500_Price,S&P_500_Vol.,Bitcoin_Price,Bitcoin_Vol.,...
2024-01-30,4845.65,4201370000,42648.61,15892400,...
2024-01-31,4890.98,3986220000,42691.54,14275300,...
2024-02-01,4927.19,3852090000,43145.78,13521600,...
```

---

## Data Processing Pipeline

Once downloaded, the notebook performs:

1. **Loading:** Read CSV with date parsing
2. **Cleaning:** Remove commas, handle missing values
3. **Feature Engineering:** Create 24 technical indicators
4. **Target Creation:** Binary UP/DOWN based on next-day S&P 500
5. **Splitting:** Temporal 60/20/20 train/val/test split
6. **Preprocessing:** Outlier clipping, robust scaling

**Total Features After Engineering:** 61 (37 original + 24 technical)

---

## Support

For dataset-related issues:

1. **First:** Check this README
2. **Second:** Verify file path in notebook Cell 2
3. **Third:** Re-download from Kaggle if corrupted
4. **Last:** Contact course staff if completely unable to access Kaggle

For Kaggle account issues:
- Visit: https://www.kaggle.com/contact

---

**Last Updated:** March 2026  
**Maintainer:** [Your Name - optional]  
**Course:** MSIN0097 Predictive Analytics, UCL
