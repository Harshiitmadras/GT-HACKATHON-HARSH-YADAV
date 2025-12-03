# 🚀 Auto Insight Engine: The Automated Insight Engine

**Tagline:** An event-driven data pipeline that ingests raw CSV/SQL/JSON logs and generates executive-ready PDF and PowerPoint reports with AI-generated narratives in under 30 seconds.

---

## 1. The Problem (Real World Scenario)

**Context:**  
In the AdTech world, we generate terabytes of data daily foot traffic logs, ad clickstreams, and weather reports. Currently, Account Managers manually download CSVs and take screenshots to create “Weekly Performance Reports.”

**The Pain Point:**  
This process is slow, error-prone, and boring. Delays in reporting mean clients may not know about campaign issues for days, potentially wasting budget.

**My Solution:**  
I built the **Auto Insight Engine**, an event-driven system where you simply drop a raw dataset into a folder. Within **30 seconds**, the system generates a fully analyzed, executive-ready PDF and PPTX report, sent via email or downloadable.

---

## 2. Expected End Result

**For the User:**

- **Input:** Drop a raw CSV, SQL export, or JSON file into the input folder.
- **Action:** Wait 30 seconds.
- **Output:** Receive a professional report containing:
  - Week-over-Week growth charts
  - List of detected anomalies (e.g., "Traffic dropped 40% in Miami")
  - AI-generated executive summary explaining anomalies (correlated with weather/foot-traffic data)

---

## 3. Technical Approach

**System Architecture:**

- **Ingestion (Event-Driven):** Python `watchdog` script listens for new files in real-time.  
- **Data Processing:** Polars is used instead of Pandas for faster, memory-efficient operations with strict schema validation.  
- **Anomaly Detection:** Isolation Forest (Scikit-Learn) identifies statistical outliers automatically.  
- **Generative AI (Executive Analyst):**  
  - Sends anomaly metadata to **Google Gemini 1.5 Pro**.  
  - Uses few-shot prompts to emulate a Senior Data Analyst.  
  - Validation step ensures AI math matches Polars outputs to prevent hallucinations.  
- **Reporting:** WeasyPrint renders HTML/CSS into pixel-perfect PDF; branded PPTX templates are used for PowerPoint exports.  

---

## 4. Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3.11 |
| Data Engine | Polars (Rust-based DataFrame library) |
| Machine Learning | Scikit-Learn (Isolation Forest) |
| AI Model | Google Gemini 1.5 Pro via Vertex AI |
| Orchestration | Docker & Docker Compose |
| Visualization | Plotly & WeasyPrint |
| Frontend | Optional: Next.js / React for file uploads |

---

## 5. Key Features

- Supports CSV, SQL, JSON ingestion.  
- Automatic **PII masking** (emails, phone numbers, IDs).  
- High-performance anomaly detection with Isolation Forest.  
- AI-generated **executive summaries**, hallucination-guarded.  
- **PDF & PPTX reports** with charts & narrative.  
- Frontend for CSV upload and live report download.  
- Dockerized & deployment-ready.  
- Scheduled / event-driven execution possible.

---

## 6. Challenges & Learnings

**Challenge 1: AI Hallucinations**  
- **Issue:** AI invented reasons for anomalies (e.g., claiming "It rained" without weather data).  
- **Solution:** Implemented a "Strict Context" prompt: "Only use the data provided. If unknown, say 'Unknown'." This drastically reduced hallucination.

**Challenge 2: Docker Networking**  
- **Issue:** Python container couldn't send emails to SMTP server.  
- **Solution:** Configured Docker Networks and environment variables in `docker-compose.yml` to allow external traffic.

**Challenge 3: Data Handling**  
- Large CSVs required efficient processing → switched to Polars from Pandas.  
- Ensured PII masking before LLM calls to comply with privacy guidelines.

---

## 7. Visual Proof

- Terminal showing anomalies detected by Isolation Forest  
- Final PDF & PPTX report generated  
- Sample charts included in PDF/PPTX  
- Automated pipeline triggered by file drop

---

## 8. How to Run

```bash
# 1. Clone Repository
git clone https://github.com/Harshiitmadras/GT-HACKATHON-HARSH-YADAV.git
cd GT-HACKATHON-HARSH-YADAV

# 2. Add API Key
export GEMINI_API_KEY="your_key_here"

# 3. Build & Run Containers
docker-compose up --build

# 4. Test Pipeline
# Move a sample dataset to the input folder to trigger analysis
mv data/sample.csv data/input/
