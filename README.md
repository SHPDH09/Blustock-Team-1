# 🛠 Data Quality Monitoring System

A Python-based project built in **Google Colab** to monitor, analyze, and report the quality of data.  
This project checks for missing values, duplicates, invalid data types, and generates a **data quality score** with a downloadable **PDF report** and optional **dashboard**.  

---

## 🚀 Features

- ✅ **Data Ingestion** – Load data from CSV or database  
- ✅ **Quality Checks** – Missing values, duplicates, outliers, and schema validation  
- ✅ **Data Quality Score** – Auto-calculated percentage for quick insights  
- ✅ **Reports** – Export summary in **PDF** format  
- ✅ **Dashboard (Optional)** – Streamlit-based UI using `pyngrok` in Colab  
- ✅ **Email Alerts** (Optional) – Send notifications when data quality is low  

---

## 📂 Project Structure
data-quality-monitoring/
│
├── data/ # CSV datasets
│ ├── sales_data.csv
│
├── notebooks/
│ ├── data_quality_monitoring.ipynb
│
├── app.py # Streamlit dashboard
├── report.py # PDF report generator
├── requirements.txt # Dependencies
└── README.md

yaml
Copy
Edit

---

## 🛠 Tech Stack

| Component          | Technology        |
|--------------------|-------------------|
| Data Processing    | Python, Pandas    |
| Visualization      | Streamlit (via pyngrok) |
| Reports            | FPDF              |
| Storage (Optional) | MySQL / PostgreSQL |
| Notebook           | Google Colab      |

---

## 📥 Installation & Setup (Google Colab)

1. Open **Google Colab**  
2. Install dependencies:
    ```python
    !pip install pandas numpy fpdf openpyxl sqlalchemy streamlit pyngrok
    ```
3. Load your dataset:
    ```python
    import pandas as pd

    url = "https://raw.githubusercontent.com/plotly/datasets/master/superstore.csv"
    data = pd.read_csv(url)
    data.head()
    ```

---

## 🔍 Run Data Quality Checks
```python
def check_missing_values(df):
    return df.isnull().sum()

def check_duplicates(df):
    return df.duplicated().sum()

missing = check_missing_values(data)
duplicates = check_duplicates(data)

print("Missing Values:\n", missing)
print("Duplicate Records:", duplicates)
📊 Generate Data Quality Report (PDF)

from fpdf import FPDF

def generate_report(missing, duplicates, score):
    pdf = FPDF()
    pdf.add_page()
    pdf.set_font("Arial", size=12)
    pdf.cell(200, 10, txt="Data Quality Report", ln=True, align="C")
    pdf.cell(200, 10, txt=f"Missing Values: {missing.sum()}", ln=True)
    pdf.cell(200, 10, txt=f"Duplicate Records: {duplicates}", ln=True)
    pdf.cell(200, 10, txt=f"Quality Score: {score:.2f}%", ln=True)
    pdf.output("data_quality_report.pdf")
🌐 Optional: Run Dashboard in Colab

%%writefile app.py
import streamlit as st
import pandas as pd

st.title("Data Quality Monitoring Dashboard")

data = pd.read_csv("sales_data.csv")
missing = data.isnull().sum().sum()
duplicates = data.duplicated().sum()
score = 100 - ((missing / (len(data)*len(data.columns))) * 50) - ((duplicates/len(data))*30)

st.metric("Missing Values", missing)
st.metric("Duplicate Records", duplicates)
st.metric("Quality Score", f"{score:.2f}%")
Run it:

from pyngrok import ngrok
!streamlit run app.py &

url = ngrok.connect(8501)
url
📊 Sample Dataset
You can use this sales dataset:


https://raw.githubusercontent.com/plotly/datasets/master/superstore.csv
✅ Future Enhancements
Integration with Great Expectations for advanced rules

Database storage for historical quality trends

📄 License
This project is licensed under the MIT License.
