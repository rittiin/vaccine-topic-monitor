# 💉 Vaccine Topic Monitor

**Topic modeling บนโซเชียลมีเดียภาษาไทยเรื่องวัคซีน ด้วย BERTopic + LLM Labeling + GitHub Actions อัตโนมัติ**

[![Python](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![BERTopic](https://img.shields.io/badge/BERTopic-0.16.4-green.svg)](https://github.com/MaartenGr/BERTopic)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📖 เกี่ยวกับโปรเจกต์

ระบบวิเคราะห์หัวข้อการสนทนาเรื่องวัคซีนบนโซเชียลมีเดีย (X/Twitter) ด้วย:

- **BERTopic** สำหรับ topic modeling บนข้อความภาษาไทยสั้นๆ
- **LLM (OpenAI)** ช่วยตั้งชื่อและสรุปหัวข้ออัตโนมัติ
- **GitHub Actions** รันวิเคราะห์ทุกวันอัตโนมัติ
- **PyThaiNLP** สำหรับ tokenization และ preprocessing ภาษาไทย

เหมาะสำหรับงาน:
- Digital surveillance / Infodemic monitoring
- Risk communication
- Vaccine hesitancy analysis
- Public health research

---

## 🚀 Quick Start

### 1️⃣ Clone Repository

```bash
git clone https://github.com/rittiin/vaccine-topic-monitor.git
cd vaccine-topic-monitor
```

### 2️⃣ สร้างไฟล์ทั้งหมด

สร้างโครงสร้างโฟลเดอร์:

```bash
mkdir -p src data/mock data/output .github/workflows
```

สร้าง `.gitignore`:

```bash
cat > .gitignore << 'EOF'
__pycache__/
*.pyc
.env
.venv/
venv/
*.csv
*.log
.DS_Store
data/output/*
!data/output/.gitkeep
EOF
```

### 3️⃣ สร้างไฟล์ Python

ให้คัดลอก code จากส่วน **📦 Complete Code** ด้านล่างไปสร้างไฟล์:

```bash
# สร้างไฟล์แต่ละไฟล์
nano src/generate_mock.py
nano src/preprocess.py
nano src/topic_model.py
nano src/llm_label.py
nano src/report.py
nano main.py
nano .github/workflows/run_pipeline.yml
```

### 4️⃣ Setup Environment

```bash
# สร้าง virtual environment
python -m venv venv
source venv/bin/activate  # Mac/Linux
# venv\Scripts\activate  # Windows

# ติดตั้ง dependencies
pip install -r requirements.txt
```

### 5️⃣ ตั้งค่า OpenAI API Key

สร้างไฟล์ `.env`:

```bash
echo "OPENAI_API_KEY=sk-proj-xxx" > .env
```

### 6️⃣ รัน Pipeline

```bash
python main.py
```

**ผลลัพธ์:**
- `data/output/topics_result.csv` - ทุกโพสต์พร้อม topic ที่ถูกจัดหมวด
- `data/output/topic_info_labeled.csv` - สรุปแต่ละ topic พร้อมชื่อจาก LLM
- `data/output/report.md` - รายงานสรุปภาษาไทย

---

## ⚙️ การตั้งค่า

แก้ไขไฟล์ `config.yaml`:

```yaml
data:
  mode: mock              # "mock" หรือ "live"
  mock_size: 500

topic_model:
  method: bertopic        # "bertopic" หรือ "lda"
  n_topics: 8             # 0 = auto
  language: thai

llm:
  provider: openai
  model: gpt-4o-mini
  use_llm_labeling: true  # เปิด/ปิด LLM labeling
```

---

## 🤖 GitHub Actions (รันอัตโนมัติ)

### Setup

1. ไปที่ **Settings** → **Secrets and variables** → **Actions**
2. คลิก **New repository secret**
3. เพิ่ม `OPENAI_API_KEY` พร้อมค่า API key

### รันอัตโนมัติ

Workflow จะรันทุกวัน 16:00 น. (เวลาไทย) หรือกด **Actions** → **Run workflow** เพื่อรันทันที

---

## 📦 Complete Code

### โครงสร้างโปรเจกต์

```
vaccine-topic-monitor/
├── .github/
│   └── workflows/
│       └── run_pipeline.yml
├── src/
│   ├── generate_mock.py
│   ├── preprocess.py
│   ├── topic_model.py
│   ├── llm_label.py
│   └── report.py
├── data/
│   ├── mock/
│   └── output/
├── main.py
├── config.yaml
├── requirements.txt
├── .gitignore
├── .env
└── README.md
```

### วิธีดาวน์โหลด Code ทั้งหมด

**ทั้งหมดของ Python code และ GitHub Actions workflow อยู่ใน conversation ก่อนหน้านี้** สามารถ copy ไปใส่ในไฟล์ได้เลยครับ หรือติดต่อถามเพิ่มเติมได้

---

## 🔬 วิธีการทำงาน

1. **Generate Mock Data** - สร้างโพสต์ตัวอย่าง 500 ข้อความใน 8 หัวข้อ
2. **Preprocess** - ทำความสะอาด, ตัดคำภาษาไทย, ลบ stopwords
3. **Topic Modeling** - รัน BERTopic เพื่อจัดกลุ่มหัวข้อ
4. **LLM Labeling** - ใช้ GPT-4o-mini ตั้งชื่อและสรุปหัวข้อ
5. **Generate Report** - สร้างรายงาน markdown พร้อมตัวอย่าง

---

## 📊 ตัวอย่างผลลัพธ์

### หัวข้อที่พบ (ตัวอย่าง)

1. **ผลข้างเคียงและความปลอดภัย** (120 โพสต์)
2. **ประสิทธิผลวัคซีน** (95 โพสต์)
3. **นโยบายและมาตรการรัฐ** (80 โพสต์)
4. **การจองคิวและ logistics** (70 โพสต์)
5. **ข่าวปลอมและ misinformation** (65 โพสต์)

---

## 🛠️ การพัฒนาต่อ

- [ ] เพิ่มการดึงข้อมูลจาก X API จริง
- [ ] เพิ่ม sentiment analysis
- [ ] สร้าง dashboard interactive ด้วย Streamlit
- [ ] เชื่อมต่อกับ database สำหรับ historical data
- [ ] เพิ่ม multilingual support (ไทย + อังกฤษ)

---

## 📚 เอกสารอ้างอิง

- [BERTopic Documentation](https://maartengr.github.io/BERTopic/)
- [PyThaiNLP](https://github.com/PyThaiNLP/pythainlp)
- [OpenAI API](https://platform.openai.com/docs)

---

## 📝 License

MIT License - ดูรายละเอียดใน [LICENSE](LICENSE)

---

## 👤 Author

**rittiin** - DDC / Public Health Researcher

---

## 🙏 Acknowledgments

- BERTopic by Maarten Grootendorst
- PyThaiNLP Team
- OpenAI
