
# Social Media News Verification System

## Overview
This application cross-references **claims, posts, and captions** from **Facebook, Instagram, and TikTok** against reputable online news sources.  

It uses:
- **Data scraping and aggregation** to gather credible news articles
- **Natural Language Processing (NLP)** to extract key facts from posts
- **Semantic similarity and fact-checking algorithms** to find matches or contradictions

This helps detect **misleading or unverified information** circulating on social platforms.

---

## Key Features

1. **Social Media Input**
   - Accepts content via API links, text, screenshots, or videos
   - Extracts captions, comments, and descriptions

2. **News Article Scraper**
   - Uses scraping pipelines to pull articles from trusted news sites and open RSS feeds
   - Filters out duplicate or low-quality sources

3. **Cross-Reference Engine**
   - Compares extracted content from social posts with scraped articles
   - Uses **semantic similarity (vector embeddings)** to find matches

4. **Output**
   - **Credibility Score** based on similarity
   - **Matched Articles List**
   - **Reasoning:** Highlights overlaps or discrepancies

---

## High-Level Workflow

1. **Input Social Content**  
   User provides:
   - A post URL from Facebook, Instagram, or TikTok
   - OR text/image/video files

2. **Content Extraction**  
   - Extracts text and metadata using APIs or OCR (for images/videos)

3. **Data Scraper Module**  
   - Crawls selected reputable sources (e.g., Reuters, BBC, AP News)
   - Collects recent articles (configurable timeframe)

4. **Comparison Engine**
   - Converts post text and article text into embeddings
   - Calculates semantic similarity
   - Flags closest matching articles and scores confidence

5. **Results**
   - Displays a **credibility score (0-100)** and matching sources

---

## Architecture

```
  [Facebook / Instagram / TikTok APIs]
                    │
                    ▼
         ┌─────────────────────┐
         │ Content Extractor   │
         │ (Captions, OCR)     │
         └─────────┬──────────┘
                   │
                   ▼
         ┌─────────────────────┐
         │ News Scraper        │
         │ (RSS + HTML parse)  │
         └─────────┬──────────┘
                   │
                   ▼
         ┌─────────────────────────────┐
         │ NLP Comparison Engine       │
         │ - Summarization (optional)  │
         │ - Embedding Similarity      │
         │ - Fact Matching             │
         └─────────┬──────────────────┘
                   │
                   ▼
          ┌───────────────────┐
          │  Results & Score  │
          └───────────────────┘
```

---

## Modules

### 1. Social Media Content Extractor
- Uses:
  - Facebook Graph API
  - Instagram Basic Display API
  - TikTok API
- For screenshots or videos:
  - OCR via **Tesseract**
  - Speech-to-text for video audio

---

### 2. News Article Scraper

- Built with **Scrapy** or **Newspaper3k**:
  - Fetches news from a predefined list of trusted sites
  - Parses title, body, timestamp
  - Cleans text (removes HTML, ads, and unrelated links)

Example sources:
- Reuters
- BBC
- AP News
- NPR
- Al Jazeera

---

### 3. NLP Comparison Engine

- **Text Preprocessing:**
  - Remove stopwords, normalize text
- **Embeddings:** 
  - Uses `sentence-transformers` (BERT-based) to encode both social content and article text
- **Similarity:**
  - Cosine similarity between vectors
  - Threshold-based filtering for matches
- **Output:**
  - Matched articles with similarity scores

---

## Example Output

```
Credibility Score: 85
Matching Articles:
1. Reuters - "New policy announced for economic reform" (Similarity: 0.92)
2. BBC - "Economic measures detailed" (Similarity: 0.88)

Reasoning:
- Extracted key claim matches top two reputable sources.
- Minor language differences, but factual alignment confirmed.
```

---

## Setup Instructions

### Prerequisites
- Python 3.10+
- Node.js (for frontend dashboard)
- APIs:
  - Facebook Graph API
  - Instagram API
  - TikTok API

---

### Clone Repository
```bash
git clone https://github.com/your-username/news-verification-system.git
cd news-verification-system
```

---

### Backend Setup

1. Install Python dependencies:
```bash
cd backend
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
pip install -r requirements.txt
```

2. Start backend:
```bash
python app.py
```

---

### Scraper Setup

In a separate terminal:
```bash
cd scraper
pip install -r requirements.txt
scrapy crawl trusted_sources
```

This will populate a local folder or database with articles.

---

### Frontend Setup (Optional)
```bash
cd frontend
npm install
npm run dev
```

Access the dashboard at `http://localhost:3000`.

---

## Deployment (Docker/Kubernetes)

1. Build Docker images:
```bash
docker-compose up --build
```

2. Kubernetes:
   - Create manifests in `k8s/`
   - Apply:
     ```bash
     kubectl apply -f k8s/
     ```

---

## Roadmap
- [ ] Add automatic summarization of matching articles
- [ ] Integrate multilingual news sources
- [ ] Support live-stream scanning

---

## Ethical Considerations
- **No definitive labels:** Provides probability-based results
- **Transparency:** Displays matched articles for user validation
- **Respect robots.txt:** Scraper only targets allowed domains

---

## License
MIT License – see the [LICENSE](LICENSE) file.
