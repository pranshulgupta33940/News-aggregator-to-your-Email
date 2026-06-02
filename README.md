#  AI News Aggregator to Your Email

<div align="center">

**An intelligent news aggregation system that scrapes AI news from multiple sources, processes them with AI, and delivers personalized digests to your email.**

[![Python Version](https://img.shields.io/badge/Python-3.12+-blue.svg)](https://www.python.org/downloads/)

[![Status](https://img.shields.io/badge/Status-Active-success.svg)](https://github.com/pranshulgupta33940/News-aggregator-to-your-Email)

[Features](#-features) • [Quick Start](#-quick-start) • [Installation](#-installation) • [Usage](#-usage) • [Configuration](#-configuration) • [Architecture](#-architecture)

</div>

---

##  Overview

**AI News Aggregator** is an automated system that:
-  **Scrapes** AI-related news from multiple sources (YouTube, OpenAI, Anthropic)
-  **Processes** content using AI (transcripts, markdown, article summaries)
-  **Creates** intelligent digests with top stories
-  **Delivers** curated news directly to your email inbox

Perfect for staying updated on the latest developments in Artificial Intelligence without information overload!

---

##  Features

<table>
<tr>
<td>

### 📡 Multi-Source Aggregation
- YouTube channel transcripts
- OpenAI blog RSS feed
- Anthropic research updates
- Extensible scraper architecture

</td>
<td>

###  AI-Powered Processing
- Intelligent content curation
- Transcript extraction & analysis
- Automatic digest generation
- Smart summarization

</td>
</tr>
<tr>
<td>

###  Data Management
- PostgreSQL database
- Structured data models
- Full audit trail
- Query repositories

</td>
<td>

###  Email Delivery
- Automated scheduling
- HTML formatted digests
- Personalized content
- Daily/Custom intervals

</td>
</tr>
<tr>
<td>

###  Docker Support
- Containerized setup
- Docker Compose ready
- Production-ready config
- Easy deployment

</td>
<td>

###  Developer Friendly
- Clean architecture
- Well-organized modules
- Configuration management
- Logging & monitoring

</td>
</tr>
</table>

---

##  Quick Start

### Prerequisites
- Python 3.12+
- PostgreSQL
- API Keys (OpenAI, Anthropic)

### One-Command Setup

```bash
# Clone repository
git clone https://github.com/pranshulgupta33940/News-aggregator-to-your-Email.git
cd News-aggregator-to-your-Email

# Install dependencies
pip install -e .

# Configure environment
cp app/example.env .env
# Edit .env with your credentials

# Run the pipeline
python main.py
```

### Using Docker

```bash
docker-compose up -d
```

---

##  Installation

### Standard Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/pranshulgupta33940/News-aggregator-to-your-Email.git
   cd News-aggregator-to-your-Email
   ```

2. **Install dependencies using UV (recommended)**
   ```bash
   pip install uv
   uv pip install -e .
   ```
   
   Or with pip:
   ```bash
   pip install -e .
   ```

3. **Setup PostgreSQL Database**
   ```bash
   # Create database
   createdb ai_news_aggregator
   
   # Run migrations
   python -c "from app.database.create_tables import setup_database; setup_database()"
   ```

4. **Configure Environment Variables**
   ```bash
   cp app/example.env .env
   ```

### Docker Installation

1. **Install Docker & Docker Compose**
   - [Docker](https://docs.docker.com/install/)
   - [Docker Compose](https://docs.docker.com/compose/install/)

2. **Run with Docker Compose**
   ```bash
   docker-compose up -d
   ```

---

##  Configuration

### Environment Variables (`.env`)

Create a `.env` file in the project root:

```bash
# Database Configuration
DATABASE_URL=postgresql://user:password@localhost:5432/ai_news_aggregator

# AI Service APIs
OPENAI_API_KEY=your_openai_api_key_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here

# Email Configuration
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SENDER_EMAIL=your_email@gmail.com
SENDER_PASSWORD=your_app_password
RECIPIENT_EMAIL=recipient@example.com

# YouTube Configuration (optional)
YOUTUBE_API_KEY=your_youtube_api_key_here

# App Configuration
LOG_LEVEL=INFO
SCRAPING_HOURS=24
TOP_N_ARTICLES=10
```

### YouTube Channels

Edit `app/config.py` to add/remove YouTube channels:

```python
YOUTUBE_CHANNELS = [
    "UCawZsQWqfGSbCI5yjkdVkTA",  # Matthew Berman
    # Add more channel IDs here
]
```

### Scheduling

Set up scheduled runs using cron:

```bash
# Run daily at 9 AM
0 9 * * * cd /path/to/project && python main.py >> logs/daily_run.log 2>&1
```

Or use `app/daily_runner.py` with a scheduler like APScheduler.

---

##  Usage

### Run the Complete Pipeline

```bash
# Default: last 24 hours, top 10 articles
python main.py

# Custom parameters
python main.py 48 15
# Arguments: hours=48, top_n=15
```

### Run Individual Components

```bash
# Just scrape sources
python -c "from app.runner import run_scrapers; run_scrapers(hours=24)"

# Process transcripts
python -c "from app.services.process_youtube import process_youtube_transcripts; process_youtube_transcripts()"

# Generate digest
python -c "from app.services.process_digest import process_digests; process_digests(top_n=10)"

# Send email
python -c "from app.services.process_email import send_digest_email; send_digest_email()"
```

### API Usage

```python
from app.runner import run_scrapers
from app.daily_runner import run_daily_pipeline

# Run complete pipeline
result = run_daily_pipeline(hours=24, top_n=10)
print(result)

# Output:
# {
#   "start_time": "2026-06-02T10:30:00",
#   "scraping": {"youtube": 5, "openai": 3, "anthropic": 2},
#   "processing": {...},
#   "digests": {...},
#   "email": {...},
#   "success": True
# }
```

---

##  Architecture

### Project Structure

```
ai-news-aggregator/
│
├── app/
│   ├── agent/                 # AI agent modules
│   │   ├── curator_agent.py   # Content curation
│   │   ├── digest_agent.py    # Digest generation
│   │   └── email_agent.py     # Email formatting
│   │
│   ├── database/              # Data layer
│   │   ├── connection.py      # DB connection
│   │   ├── models.py          # SQLAlchemy models
│   │   ├── repository.py      # Data access
│   │   └── create_tables.py   # Schema setup
│   │
│   ├── scrapers/              # Content scraping
│   │   ├── youtube.py         # YouTube scraper
│   │   ├── openai.py          # OpenAI RSS scraper
│   │   └── anthropic.py       # Anthropic scraper
│   │
│   ├── services/              # Business logic
│   │   ├── process_youtube.py       # Transcript processing
│   │   ├── process_anthropic.py     # Content processing
│   │   ├── process_curator.py       # Curation logic
│   │   ├── process_digest.py        # Digest generation
│   │   └── process_email.py         # Email sending
│   │
│   ├── profiles/              # User profiles
│   │   └── user_profile.py
│   │
│   ├── config.py              # Configuration
│   ├── runner.py              # Scraper orchestration
│   ├── daily_runner.py        # Pipeline orchestration
│   └── __init__.py
│
├── docker/
│   └── docker-compose.yml     # Docker setup
│
├── main.py                    # Entry point
├── pyproject.toml             # Dependencies
├── README.md                  # This file
└── example.env               # Environment template
```

### Data Flow

```
┌─────────────────────────────────────────────────────────┐
│                  SCRAPING LAYER                         │
├──────────────┬──────────────┬──────────────────────────┤
│ YouTube      │ OpenAI RSS   │ Anthropic RSS            │
│ Transcripts  │ Articles     │ Articles                 │
└──────┬───────┴──────┬───────┴─────────┬────────────────┘
       │              │                 │
       └──────────────┼─────────────────┘
                      │
            ┌─────────▼──────────┐
            │  DATABASE STORAGE  │
            └─────────┬──────────┘
                      │
┌─────────────────────▼──────────────────────────────────┐
│              PROCESSING LAYER                          │
├─────────────┬─────────────┬─────────────────────────┤
│ Transcripts │ Markdown    │ Article Summaries      │
│ Analysis    │ Processing  │ Extraction             │
└──────┬──────┴─────────┬───┴──────────────┬─────────┘
       │                │                  │
       └────────────────┼──────────────────┘
                        │
            ┌───────────▼─────────┐
            │  AI CURATION AGENT  │
            │  (Topic Selection)  │
            └───────────┬─────────┘
                        │
            ┌───────────▼──────────┐
            │  DIGEST GENERATION   │
            │  (AI Summarization)  │
            └───────────┬──────────┘
                        │
            ┌───────────▼──────────┐
            │   EMAIL FORMATTING   │
            │   (HTML Templates)   │
            └───────────┬──────────┘
                        │
            ┌───────────▼──────────┐
            │    EMAIL DELIVERY    │
            │    (SMTP Sending)    │
            └──────────────────────┘
```

---

##  Key Dependencies

| Package | Purpose |
|---------|---------|
| **beautifulsoup4** | HTML parsing |
| **feedparser** | RSS feed parsing |
| **youtube-transcript-api** | YouTube transcript extraction |
| **openai** | OpenAI API integration |
| **sqlalchemy** | ORM & database abstraction |
| **psycopg2-binary** | PostgreSQL adapter |
| **pydantic** | Data validation |
| **python-dotenv** | Environment management |

---

##  Troubleshooting

### Database Connection Issues

```bash
# Check PostgreSQL is running
psql -U postgres

# Verify DATABASE_URL in .env
echo $DATABASE_URL
```

### API Key Issues

```bash
# Verify environment variables are loaded
python -c "import os; from dotenv import load_dotenv; load_dotenv(); print(os.getenv('OPENAI_API_KEY'))"
```

### Email Delivery Failures

```bash
# Enable app passwords for Gmail
# 1. Enable 2FA: https://myaccount.google.com/security
# 2. Generate app password: https://myaccount.google.com/apppasswords
# 3. Use app password in SENDER_PASSWORD
```

### Check Logs

```bash
# View application logs
tail -f logs/app.log

# Run with debug logging
DEBUG=1 python main.py
```

---

##  Contributing

Contributions are welcome! Here's how to help:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Commit with clear messages**
   ```bash
   git commit -m "Add amazing feature"
   ```
5. **Push to your fork**
   ```bash
   git push origin feature/amazing-feature
   ```
6. **Open a Pull Request**

### Development Setup

```bash
# Install dev dependencies
pip install -e ".[dev]"

# Run tests
pytest

# Format code
black app/
```

---

##  Roadmap

- [ ] Multi-language support
- [ ] Custom digest templates
- [ ] Web dashboard UI
- [ ] Advanced filtering options
- [ ] Slack integration
- [ ] Telegram bot support
- [ ] Machine learning for topic clustering
- [ ] Analytics and reporting

---

##  FAQ

**Q: How often should I run the pipeline?**  
A: Recommended daily. Use cron or a task scheduler for automation.

**Q: Can I add custom news sources?**  
A: Yes! Create a new scraper in `app/scrapers/` following existing patterns.

**Q: Is this production-ready?**  
A: Yes, with proper environment configuration and database setup.

**Q: How do I modify email templates?**  
A: Edit `app/services/process_email.py` email formatting functions.

**Q: Can I run this on multiple servers?**  
A: Yes, use Docker + Kubernetes or multiple Docker hosts with shared database.

---


##  Author

**Pranshul Gupta**
- GitHub: [@pranshulgupta33940](https://github.com/pranshulgupta33940)


---


---

<div align="center">

### Made with ❤️ by Pranshul Gupta

[Back to Top](#-ai-news-aggregator-to-your-email)

</div>
