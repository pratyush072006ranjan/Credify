# Credify

**AI-Powered News Fact-Checking & Verification Platform**

Credify is a full-stack AI-powered news verification platform that analyzes user-submitted claims using multi-source news aggregation, semantic stance detection, trusted-source weighting, and Gemini-based evidence synthesis.

The platform is designed to help users understand whether a news claim is **TRUE, PARTIALLY TRUE, UNCERTAIN, or FALSE** based on supporting and contradicting evidence from multiple sources.

---

## 🚀 Key Features

- 🔎 **Claim Verification** — Submit a news claim for automated verification.
- 📰 **Multi-Source News Aggregation** — Collects evidence from RSS feeds, GNews, and NewsAPI.
- 🤖 **AI-Powered Stance Detection** — Uses Gemini to classify evidence as:
  - `SUPPORTS`
  - `REFUTES`
  - `UNRELATED`
- ⭐ **Trusted Source Weighting** — Gives higher importance to evidence from trusted news organizations.
- 📊 **Credibility Scoring** — Calculates an evidence-based support/refute score.
- 🧠 **AI Evidence Synthesis** — Gemini generates the final verification explanation and verdict.
- 📄 **Article Extraction** — Extracts readable article content for evidence analysis.
- ⚡ **Caching** — Reduces repeated API requests and improves response time.
- 🛡️ **REST API Backend** — Built with Node.js and Express.js.
- 🌐 **Web Frontend** — Provides a user-friendly interface for submitting and viewing verification results.

---

## 🏗️ System Architecture

```text
                         ┌─────────────────────┐
                         │      User / UI      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Credify Backend   │
                         │ Node.js + Express   │
                         └──────────┬──────────┘
                                    │
                     ┌──────────────┴──────────────┐
                     ▼                             ▼
            ┌─────────────────┐           ┌─────────────────┐
            │ News Aggregation│           │  Claim Analysis │
            └────────┬────────┘           └────────┬────────┘
                     │                             │
          ┌──────────┼──────────┐                  ▼
          ▼          ▼          ▼          ┌─────────────────┐
        RSS       GNews      NewsAPI       │ Gemini AI       │
                                          │ Stance + Final  │
                                          │ Synthesis        │
                                          └────────┬────────┘
                                                   │
                                                   ▼
                                          ┌─────────────────┐
                                          │ Credibility /   │
                                          │ Verdict Engine  │
                                          └────────┬────────┘
                                                   │
                                                   ▼
                                          ┌─────────────────┐
                                          │ Verification    │
                                          │ Result          │
                                          └─────────────────┘
```

---

## 🔄 Verification Workflow

```text
User Claim
    │
    ▼
Claim Validation
    │
    ▼
News Search
    │
    ├── RSS
    ├── GNews
    └── NewsAPI
    │
    ▼
Article Extraction
    │
    ▼
Relevant Evidence Selection
    │
    ▼
Gemini Stance Detection
    │
    ├── SUPPORTS
    ├── REFUTES
    └── UNRELATED
    │
    ▼
Trusted-Source Weighting
    │
    ▼
Credibility Score
    │
    ▼
Verdict Generation
    │
    ▼
Gemini Evidence Synthesis
    │
    ▼
Final Verification Result
```

---

## 🧰 Tech Stack

### Frontend
- Next.js
- React.js
- JavaScript / JSX
- CSS / responsive UI

### Backend
- Node.js
- Express.js
- Axios
- CORS
- dotenv

### AI / NLP
- Google Gemini API
- `@google/generative-ai`
- Semantic stance classification
- Evidence synthesis

### News & Data Sources
- GNews API
- NewsAPI
- Trusted RSS feeds
- `@extractus/article-extractor`

### Performance
- In-memory caching
- Bounded cache with TTL
- Limited evidence processing to control API usage

---

## 📁 Project Structure

```text
Credify/
│
├── backend/
│   ├── services/
│   │   ├── aiService.js
│   │   ├── mlService.js
│   │   └── rssService.js
│   │
│   ├── server.js
│   ├── package.json
│   ├── package-lock.json
│   ├── .env
│   └── ...
│
├── frontend/
│   ├── app/
│   ├── components/
│   ├── public/
│   ├── package.json
│   └── ...
│
└── README.md
```

> The exact frontend folder structure may vary depending on the current UI implementation.

---

# ⚙️ Installation & Setup

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd Credify
```

---

## 2. Backend Setup

Navigate to the backend:

```bash
cd backend
```

Install dependencies:

```bash
npm install
```

---

## 3. Configure Environment Variables

Create a `.env` file inside the `backend` directory:

```env
GEMINI_API_KEY=your_gemini_api_key

GNEWS_API_KEY=your_gnews_api_key

NEWS_API_KEY=your_newsapi_key

PORT=5000
```

### API Keys

You need API keys for:

- Google Gemini
- GNews
- NewsAPI

Do **not** commit your `.env` file to GitHub.

Add this to `.gitignore`:

```gitignore
.env
.env.local
node_modules/
```

---

# ▶️ Running the Backend

From the `backend` directory:

```bash
node server.js
```

For development with Nodemon:

```bash
npx nodemon server.js
```

The backend will run at:

```text
http://localhost:5000
```

---

# ▶️ Running the Frontend

Open another terminal and navigate to the frontend:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The frontend will normally be available at:

```text
http://localhost:3000
```

---

# 🔌 API

## Verify News

### Endpoint

```http
POST /verify-news
```

### Request

```json
{
  "claim": "Example news claim to verify"
}
```

### Example cURL

```bash
curl -X POST http://localhost:5000/verify-news \
  -H "Content-Type: application/json" \
  -d "{\"claim\":\"Example news claim to verify\"}"
```

### Response

The API returns a structured verification result containing information such as:

```json
{
  "verdict": "TRUE",
  "confidence": 0.87,
  "explanation": "The available evidence strongly supports the claim.",
  "sources": []
}
```

The exact response fields may vary with the current backend implementation.

---

# 🧠 How Credify Determines a Verdict

Credify combines evidence from multiple sources and classifies each relevant article.

### Evidence Classes

| Classification | Meaning |
|---|---|
| `SUPPORTS` | Evidence supports the submitted claim |
| `REFUTES` | Evidence contradicts the submitted claim |
| `UNRELATED` | Article does not provide meaningful evidence |

Trusted sources receive greater weight when calculating the overall evidence score.

### Verdict Thresholds

The current backend uses approximately:

```text
Score >= 0.75  → TRUE
Score >= 0.45  → PARTIALLY TRUE
Score >= 0.25  → UNCERTAIN
Score <  0.25  → FALSE
```

These thresholds can be adjusted in the backend according to future testing and evaluation.

---

# 📰 Trusted Sources

Credify can prioritize evidence from established news organizations, including sources such as:

- Reuters
- BBC
- Associated Press
- The New York Times
- Al Jazeera
- The Times of India
- The Indian Express
- The Guardian
- Bloomberg
- The Washington Post
- The Hindu

The trusted-source list can be modified in the backend.

---

# ⚡ Caching

Credify uses a bounded in-memory cache to reduce repeated API calls.

Current design includes:

- Cache expiration: approximately **1 hour**
- Maximum cached entries: **200**
- Repeated claims can be served from cache instead of triggering the complete verification pipeline.

This helps reduce external API usage and improve response time.

---

# 🔐 Security

Recommended security practices:

- Keep API keys inside `.env`.
- Never expose Gemini, GNews, or NewsAPI keys in frontend code.
- Never commit `.env` files to GitHub.
- Validate incoming claims on the backend.
- Use CORS configuration appropriate for production.
- Add rate limiting before public deployment.
- Rotate exposed API keys immediately if they are accidentally committed.

---

# 🌐 Deployment

Credify can be deployed using:

```text
Frontend → Vercel
Backend  → Render
```

### Production Architecture

```text
User
 │
 ▼
Vercel
Next.js Frontend
 │
 │ HTTPS API Requests
 ▼
Render
Node.js + Express Backend
 │
 ├── Gemini API
 ├── GNews API
 ├── NewsAPI
 └── RSS Sources
```

### Important

In production, the frontend must use the deployed backend URL instead of:

```text
http://localhost:5000
```

For example:

```text
https://your-backend.onrender.com
```

Configure production API URLs through environment variables rather than hardcoding them.

---

# 🧪 Development Notes

To control Gemini API usage during development:

- Limit the number of articles sent to stance detection.
- Avoid unnecessary repeated Gemini calls.
- Use caching for repeated claims.
- Keep claim decomposition/query expansion disabled when API quota is limited.
- Use test data when developing UI components that do not require live AI responses.

The current implementation intentionally limits evidence processing to reduce API consumption.

---

# 🚀 Future Improvements

- 🔹 Advanced claim decomposition
- 🔹 Multilingual fact-checking
- 🔹 Improved semantic similarity and duplicate detection
- 🔹 Source reliability scoring based on historical performance
- 🔹 Browser extension for instant fact-checking
- 🔹 User accounts and verification history
- 🔹 Real-time news monitoring
- 🔹 Explainable evidence graphs
- 🔹 Vector database for semantic evidence retrieval
- 🔹 Human-in-the-loop fact verification
- 🔹 Automated misinformation trend analysis
- 🔹 More robust AI response validation and fallback handling

---

# 📊 Example Use Case

### Input

```text
"Example claim circulating on social media"
```

### Credify Process

```text
Claim
  ↓
Search multiple news sources
  ↓
Extract article content
  ↓
Analyze evidence
  ↓
Classify stance
  ↓
Weight trusted sources
  ↓
Calculate credibility
  ↓
Generate AI explanation
  ↓
Display final verdict
```

---

# 👨‍💻 Project Purpose

Credify aims to provide a transparent and automated way to evaluate online news claims by combining **multi-source journalism, AI-based semantic analysis, and evidence-weighted verification**.

It is intended as a decision-support and research tool rather than a replacement for professional journalism or human fact-checking.

---

## 📜 License

This project is intended for educational, research, and hackathon purposes.

Add an appropriate open-source license such as MIT if you plan to publish the project for public reuse.

---

## ⭐ Acknowledgements

- Google Gemini
- GNews
- NewsAPI
- Trusted RSS news providers
- Extractus Article Extractor
- Node.js & Express.js
- Next.js & React
