# Nifi_Workflows

### 📄 `README.md` — Mobile Phone Data Processing with Apache NiFi

```markdown
# 📱 Mobile Phone Data Processing with Apache NiFi

This project uses **Apache NiFi** to automate the transformation of mobile phone specification data (from a CSV file). It uses NiFi's powerful processors like `UpdateRecord` to categorize phones into segments based on their price.

---

## Features

- Ingest mobile phone data from a CSV file
- Apply real-time transformation using `UpdateRecord`
- Dynamically assign **Segment** based on `Price (USD)`:
  - **Premium**: Price > $1000
  - **Mid-Range**: $700 ≤ Price ≤ $1000
  - **Budget**: Price < $700
- Export the enriched data to a new CSV file

---

## 🧩 NiFi Flow Design

```
GetFile ➡️ UpdateRecord ➡️ PutFile
```

### 1. **GetFile**
- Reads the source file (e.g., `mobile_phones.csv`) from local directory

### 2. **UpdateRecord**
- Adds a new column `Segment` using RecordPath and Expression Language:
  ```text
  /Segment : ${Price (USD):gt(1000):ifElse('Premium', ${Price (USD):gt(700):ifElse('Mid-Range', 'Budget')}} 
  ```

### 3. **PutFile**
- Writes the transformed data with the new `Segment` column to a target directory

---

## 🏗️ Schema Example

Below is a sample Avro schema used in `CSVReader`/`CSVWriter`:

```json
{
  "type": "record",
  "name": "MobileData",
  "fields": [
    {"name": "Brand", "type": "string"},
    {"name": "Model", "type": "string"},
    {"name": "Price (USD)", "type": "double"},
    {"name": "Selling Platform", "type": "string"},
    {"name": "Rating", "type": "double"},
    {"name": "Refresh Rate (Hz)", "type": "int"},
    {"name": "Screen Size (inches)", "type": "double"},
    {"name": "RAM (GB)", "type": "int"},
    {"name": "Storage (GB)", "type": "int"},
    {"name": "Processor", "type": "string"},
    {"name": "Camera Setup", "type": "string"},
    {"name": "Segment", "type": ["null", "string"], "default": null}
  ]
}
```

---

## 🗂️ File Structure

```
📁 nifi-mobile-project/
├── input/
│   └── mobile_phones.csv
├── output/
│   └── mobile_phones_segmented.csv
├── screenshots/
│   └── flow_design.png
├── README.md
```

---

## 🛠️ Requirements

- Apache NiFi 2.x
- Java 17 or 21
- Local file system with sample data

---

## 📸 Sample Output

| Brand   | Model       | Price (USD) | Segment   |
|---------|-------------|-------------|-----------|
| OnePlus | Narzo Edge  | 1286.64     | Premium   |
| Sony    | Pixel Max   | 728.52      | Mid-Range |
| Vivo    | P Series 5G | 390.46      | Budget    |
