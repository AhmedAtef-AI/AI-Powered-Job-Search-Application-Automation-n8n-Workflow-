# 🤖 AI-Powered Job Search & Application Automation (n8n Workflow)

**Workflow Name:** `Indeed_Job_Finding_and_Submission`  
**Platform:** [n8n](https://n8n.io) (self-hosted)  
**Tech Stack:** OpenAI · LangChain · Apify · Google Sheets · Google Drive · Pinecone · Browserless · Telegram Bot API  

![Uploading image.png…]()

---

## 🧭 Overview

This project is a **fully automated AI-driven job search and application system** built using **n8n**.  
It connects conversational AI, job scraping APIs, Google services, and browser automation into one intelligent workflow.

Users can simply **chat with a Telegram bot** to:
- Search for jobs on **Indeed**
- Log job results into **Google Sheets**
- Upload and store their **CVs**
- Automatically **apply to external job listings**
- Retrieve and manage job or CV data seamlessly

---

## 🎯 Core Capabilities

| Feature | Description |
|----------|-------------|
| **AI Job Search Agent** | Uses a LangChain-based agent that interprets user commands like “Find remote data analyst jobs in the US” and converts them into structured Indeed API queries. |
| **Indeed Job Scraper Integration** | Fetches job listings in real-time via the Apify Indeed Scraper API. |
| **Google Sheets Database** | Stores job listings (title, company, salary, links, descriptions) and CV logs in Google Sheets for organized tracking. |
| **Telegram Chat Interface** | Provides an interactive experience where users control the workflow through Telegram messages. |
| **CV Upload & Parsing** | Allows users to upload their CVs, extracts and vectorizes their content using OpenAI embeddings, and stores them in Pinecone for semantic searches. |
| **Automated Job Application** | Uses Browserless (headless Chrome) to safely autofill and submit job applications on external sites. |
| **AI Memory & Context** | Remembers recent chat history to provide coherent, contextual interactions. |
| **CV Management Tools** | Log, retrieve, and delete CV entries in Google Sheets; integrates with a vector database for smart matching. |

---

## 🧩 Architecture Overview

### 1. **Telegram Entry Point**
- **Node:** `Telegram Trigger`
- Captures user messages from Telegram and forwards them to the AI Agent.

### 2. **AI-Powered Intent Understanding**
- **Node:** `Job Searching Agent`
- A LangChain agent powered by OpenRouter (LLM) and connected to memory.
- Parses user intent → generates structured JSON → triggers job search or CV actions.

### 3. **Job Retrieval (Indeed API)**
- **Node:** `Indeed Job Search Tool`
- Calls Apify’s `misceres~indeed-scraper` API with dynamic parameters (role, location, country, etc.).
- Returns structured job data.

### 4. **Job Logging & Retrieval**
- **Nodes:**  
  - `Jobs_Sheet` → Appends job data into a Google Sheet.  
  - `Search Jobs Sheet` → Searches previously logged jobs.  

### 5. **External Job Application**
- **Nodes:**  
  - `External Link Jobs Apply Tool`  
  - `Website Content Tool1`  
  - `Website Automation Tool1`
- Executes **headless browser automation** with Browserless:
  - Navigates to job application pages  
  - Detects form fields  
  - Fills applicant data (name, email, resume)  
  - Submits or simulates submission  
  - Returns a success status and screenshot

### 6. **CV Handling Workflow**
- **Nodes:**  
  - `On form submission` → Triggers when a CV is uploaded  
  - `Download file` → Retrieves CV from Google Drive  
  - `Extract from File` → Extracts text from the PDF  
  - `Embeddings OpenAI` → Converts text into vector embeddings  
  - `Pinecone Vector Store` → Stores embeddings in Pinecone  
- Enables **semantic CV search** and **job-to-profile matching**.

### 7. **CV Logs Management**
- **Nodes:**  
  - `Log CV` → Appends candidate data to “CV Logs” sheet  
  - `Get CV Logs` → Fetches existing records  
  - `CV Logs Delete` → Deletes outdated entries  

### 8. **AI Vector Retrieval**
- **Node:** `C.V DataBase`
- Retrieves vector data for candidate profiles or job similarity scoring.

### 9. **Message Response**
- **Node:** `Send a text message`
- Returns structured results, job lists, or confirmation messages to Telegram.

---

## 🧱 Data Flow Summary

1. User sends a command to Telegram (e.g., “Find backend developer jobs in Egypt”).  
2. The **AI agent** parses the message → extracts job title, country, location.  
3. Agent sends structured JSON to **Indeed API** via Apify.  
4. Job data is returned → saved to **Google Sheets**.  
5. A summarized message is sent back to the user via Telegram.  
6. If the user uploads a CV:  
   - The file is parsed → embedded via OpenAI → stored in Pinecone → logged in Google Sheets.  
7. When applying to jobs, the **Browserless tool** automates the submission safely.

---

## 🔌 Integrated APIs & Services

| Service | Purpose |
|----------|----------|
| **OpenRouter / OpenAI** | AI language processing & embeddings |
| **Apify Indeed Scraper** | Job listing extraction |
| **Google Sheets** | Job and CV data storage |
| **Google Drive** | CV file storage |
| **Pinecone** | Vector database for semantic CV retrieval |
| **Browserless** | Headless Chrome for automated job applications |
| **Telegram Bot API** | Chat interface for controlling automations |

---

## 🔐 Secure Credentials (Managed in n8n)
- OpenRouter API  
- OpenAI API  
- Google Sheets OAuth2  
- Google Drive OAuth2  
- Pinecone API  
- Telegram Bot Token  
- Browserless Access Token  

---

## 📋 Example User Interaction

**User:**  
> “Find remote frontend developer jobs in Germany.”

**AI Agent Output to Indeed Tool:**
```json
{
  "position": "frontend developer",
  "country": "DE",
  "location": "Remote",
  "maxItems": 5,
  "parseCompanyDetails": false,
  "saveOnlyUniqueItems": true,
  "followApplyRedirects": false
}
