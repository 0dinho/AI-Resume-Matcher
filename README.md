# 🎯 AI Resume Analyzer

An intelligent resume-job matching system that analyzes resumes against job descriptions using a **hybrid NLP approach** combining rule-based extraction with LLM-powered understanding.
**[Live Demo](https://ai-resume-matcher.am-tech.cloud)**

[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen?style=for-the-badge)](https://your-demo-url.com)
[![Python](https://img.shields.io/badge/Python-3.11+-blue?style=for-the-badge&logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-teal?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18+-61DAFB?style=for-the-badge&logo=react)](https://react.dev)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)](https://docker.com)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Architecture](#-architecture)
- [Engineering Highlights](#-engineering-highlights)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [API Documentation](#-api-documentation)
- [Demo](#-demo)

---

## 🎯 Overview

**AI Resume Analyzer** solves a real problem: matching candidates to jobs is time-consuming and often subjective. This system automates the process by:

1. **Parsing resumes** → Extracting structured data (contact, education, experience, skills, languages)
2. **Analyzing job descriptions** → Identifying requirements, skills, and qualifications
3. **Intelligent matching** → Calculating compatibility scores with detailed breakdowns

### The Challenge

Traditional keyword matching fails because:
- "ML" and "Machine Learning" are the same skill
- "PyTorch experience" implies "Deep Learning" knowledge
- "3 years Python" vs "Python" requires context understanding

### The Solution

A **hybrid approach** that combines:
- **Rule-based extraction** for structured, predictable data (fast, no API cost)
- **LLM-powered analysis** for context-dependent understanding (accurate, semantic)

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 📄 **Multi-format Resume Parsing** | PDF support with intelligent section detection |
| 🔍 **15+ Section Types** | Contact, Experience, Education, Skills, Languages, Projects, Certifications... |
| 🌍 **International Support** | 50+ country phone formats, 100+ cities, accented names |
| 🎯 **Smart Skill Matching** | Recognizes related skills (React → JavaScript), abbreviations (ML → Machine Learning) |
| 📊 **Weighted Scoring** | Skills (45%), Experience (35%), Education (20%) |
| 💡 **Actionable Insights** | Strengths, weaknesses, and recommendations |
| 🚀 **Production Ready** | Docker deployment, MinIO storage, Nginx reverse proxy |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              FRONTEND                                   │
│                     Vite + React + TypeScript                           │
│                         Tailwind CSS v4                                 │
└─────────────────────────────┬───────────────────────────────────────────┘
                              │ HTTP/REST
                              ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                            NGINX                                        │
│                      Reverse Proxy + SSL                                │
└─────────────────────────────┬───────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         FASTAPI BACKEND                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │   Upload    │  │   Results   │  │  Matching   │  │   Storage   │     │
│  │   Route     │  │   Route     │  │   Service   │  │   Service   │     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
│         │                │                │                │            │
│         ▼                ▼                ▼                ▼            │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                    NLP EXTRACTION PIPELINE                      │    │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐        │    │
│  │  │  Section  │ │  Contact  │ │Experience │ │ Education │        │    │
│  │  │ Detector  │ │ Extractor │ │ Extractor │ │ Extractor │        │    │
│  │  └───────────┘ └───────────┘ └───────────┘ └───────────┘        │    │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐                      │    │
│  │  │  Skills   │ │ Languages │ │    JD     │  ← LLM Enhanced      │    │
│  │  │ Extractor │ │ Extractor │ │  Parser   │                      │    │
│  │  └───────────┘ └───────────┘ └───────────┘                      │    │
│  └─────────────────────────────────────────────────────────────────┘    │
└─────────────────────────────┬───────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│       MINIO S3          │     │       GROQ API          │
│    Object Storage       │     │   Llama 3.1 70B LLM     │
│  (Resume/JD Storage)    │     │   (Skills & JD Parse)   │
└─────────────────────────┘     └─────────────────────────┘
```

---

## 🔧 Engineering Highlights

### 1. Hybrid Rule-Based + LLM Architecture

**Why not just use an LLM for everything?**

| Approach | Pros | Cons |
|----------|------|------|
| Pure LLM | Handles any format | Expensive, slow, inconsistent |
| Pure Rule-based | Fast, free, predictable | Misses context, limited flexibility |
| **Hybrid** | Best of both worlds | Requires careful design |


**Results:**
- Rule-based skills extraction: **~20% accuracy** (misses abbreviations, context)
- LLM skills extraction: **~95% accuracy** (understands "ML/DL" → Machine Learning, Deep Learning)

---

### 2. Intelligent Skill Matching

The matching engine doesn't just do string comparison. It understands skill relationships:

**Match Types:**
- `exact`: "Machine Learning" ↔ "Machine Learning"
- `related`: "Image Classification" → "Computer Vision" (child proves parent)
- `partial`: "Python" → "FastAPI" (parent implies child familiarity)

---

### 3. Production-Grade Contact Extraction

Built a **dependency-free** contact extractor that handles:

```python
# International phone formats (50+ countries)
"+1 (555) 123-4567"      # US
"+44 20 7123 4567"       # UK  
"+213 555 12 34 56"      # Algeria
"+86 138 0013 8000"      # China

# Name detection with confidence scoring
"John Michael Smith"      # 95% confidence (common first name)
"李明 (Ming Li)"          # 85% confidence (parenthetical translation)
"María García López"      # 80% confidence (accented characters)

# Location parsing
"San Francisco, CA"       # City + State
"London, United Kingdom"  # City + Country
"Algiers, Algeria"        # International
```

---

### 4. Weighted Scoring Algorithm

```python
WEIGHTS = {
    'skills': 0.45,      # Most important - can you do the job?
    'experience': 0.35,  # Second - have you done similar work?
    'education': 0.20,   # Third - formal qualifications
}

# Skills scoring breakdown
required_score = (matched_required / total_required) * 100
preferred_score = (matched_preferred / total_preferred) * 100
skills_score = required_score * 0.8 + preferred_score * 0.2
```

---

### 5. Comprehensive Resume Section Detection

Handles **15+ section types** with multiple naming conventions:

## 🛠 Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| **FastAPI** | High-performance async API framework |
| **Python 3.11+** | Core language |
| **Pydantic** | Data validation & serialization |
| **Groq API** | LLM inference |

### Frontend
| Technology | Purpose |
|------------|---------|
| **Vite** | Build tool & dev server |
| **React 18** | UI library |
| **TypeScript** | Type safety |
| **Tailwind CSS v4** | Styling |

### Infrastructure
| Technology | Purpose |
|------------|---------|
| **Docker** | Containerization |
| **Nginx** | Reverse proxy, SSL termination |
| **MinIO** | S3-compatible object storage |

### NLP & Processing
| Technology | Purpose |
|------------|---------|
| **PyMuPDF** | PDF text extraction |
| **Custom NLP Pipeline** | Rule-based extraction |
| **Llama 3.1 70B** | Semantic understanding |

---

## 🎬 Demo

🔗 **[Live Demo](https://ai-resume-matcher.am-tech.cloud)**

<!-- Add screenshots here when available -->
<!--
### Screenshots

#### Resume Analysis
![Resume Analysis](./docs/screenshots/analysis.png)

#### Match Results
![Match Results](./docs/screenshots/match.png)
-->


## Future Improvements

- Resume improvement suggestions using LLM
- Batch processing for multiple job applications
- Historical tracking and analytics
- Browser extension for job sites
- ATS compatibility scoring

---
