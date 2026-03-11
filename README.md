# 📰 AI News Summarizer Automation

## 📌 Overview
The **AI News Summarizer** is an automated workflow that collects the latest news from multiple RSS feeds, summarizes the content using **Google Gemini AI**, and sends a **daily email newsletter**.

Instead of browsing multiple websites every morning, this system delivers a **concise AI-powered news summary directly to your inbox**.

The automation is built using **n8n workflow automation**, enabling scheduled execution, automated content aggregation, AI summarization, and email delivery.

---

# 🚨 Problem
Every morning professionals spend time:

- Opening **10+ tech websites**
- Scrolling through **hundreds of articles**
- Filtering what is important
- Trying to remember key insights

This process usually takes **30–45 minutes daily**.

---

# 💡 Solution
The **AI News Summarizer** automates the entire process by:

- Fetching the latest news from RSS feeds
- Collecting articles from multiple sources
- Summarizing content using **Gemini AI**
- Delivering a **personalized daily newsletter**

### Result
⏱ **30 minutes of browsing → 2-minute email summary**

---

# ⚙️ Workflow Architecture

```
Schedule Trigger
       ↓
RSS Feed Read (AI News)
       ↓
RSS Feed Read (Technology News)
       ↓
Merge Node
       ↓
Aggregate Node
       ↓
Gemini AI Summarization
       ↓
Gmail Send Node
       ↓
Daily Email Newsletter
```

---

# 🧰 Prerequisites

Before building this workflow you need:

- **n8n Account**
- **Google Account**
- **RSS Feed URLs**
- **Google Cloud Project (for Gmail API)**

---

# 📰 Fetching News Using RSS Feeds

## What is RSS?
RSS is a **news delivery system**.

Instead of visiting multiple websites manually, RSS allows you to **receive updates automatically** whenever a website publishes new content.

Think of it like **subscribing to a newspaper online**.

---

## Finding RSS Feeds

You can locate RSS feeds in several ways:

- Look for the **RSS icon** on websites
- Add `/rss` or `/feed` to the website URL
- Search for RSS feeds online

### Example RSS Feeds

```
https://aibusiness.com/rss.xml
https://techcrunch.com/category/artificial-intelligence/feed/
https://www.reddit.com/r/technology/.rss
https://techcrunch.com/feed/
```

Helpful RSS feed repositories:

```
https://github.com/foorilla/allainews_sources
https://github.com/vishalshar/awesome_ML_AI_RSS_feed
https://github.com/RSS-Renaissance/awesome-AI-feeds
```

---

# ⚙️ Workflow Steps

---

# 1️⃣ Schedule Trigger

The **Schedule Trigger Node** automatically starts the workflow at a specific time.

### Why Schedule Trigger?
Instead of manually running the workflow every morning, this node runs it **automatically in the background**.

### Configuration

- Add **Schedule Trigger Node**
- Set interval → **Every Day**
- Set time → **10:00 AM**

This will fetch and summarize the news **daily at 10 AM**.

---

# 2️⃣ Fetch News from RSS Feeds

Use the **RSS Feed Read Node** to fetch articles.

### RSS Feed Node Purpose

- Retrieves latest articles
- Detects new updates automatically
- Converts feed into structured data

### Steps

1. Connect **RSS Feed Read Node** to the Schedule Trigger
2. Paste RSS Feed URL

Example:

```
https://aibusiness.com/rss.xml
```

3. Execute the node to test.

---

# 3️⃣ Fetch News from Multiple Sources

You can collect news from **different categories**.

Example:

### AI News Feed
```
https://aibusiness.com/rss.xml
```

### Technology News Feed
```
https://techcrunch.com/feed/
```

Each RSS feed provides separate items.

---

# 4️⃣ Merge Multiple News Sources

Use the **Merge Node** to combine multiple RSS feeds.

### Merge Node Function
- Combines items from different sources
- Keeps each article separate
- Outputs a unified data stream

### Configuration

- Add **Merge Node**
- Set **Number of Inputs → 2**
- Connect both RSS nodes

---

# 5️⃣ Aggregate All Content

The **Aggregate Node** combines all individual items into a single dataset.

### Difference Between Merge and Aggregate

**Merge**
- Combines multiple sources
- Keeps items separate

**Aggregate**
- Combines all items into one object
- Creates a single dataset

### Configuration

- Add **Aggregate Node**
- Set to **Aggregate All Item Data**

---

# 6️⃣ Summarize News Using Gemini AI

Add **Basic LLM Chain Node**.

Connect the **Google Gemini Chat Model**.

Recommended Model:

```
gemini-2.5-flash
```

### AI Prompt

```
You are an AI news summarizer assistant.

Analyze the news articles and create an email summary.

For each article:
Create a headline in ALL CAPS
Write a 2-3 sentence summary
Include the article link

Format as:

Subject: Today's AI News
```

---

# 7️⃣ Send Email Using Gmail

Use the **Gmail Send Node** to deliver the summary.

---

# 🔐 Gmail API Setup

### Step 1: Enable Gmail API

Go to:

```
Google Cloud Console → APIs & Services → Library
```

Search for:

```
Gmail API
```

Click **Enable**.

---

### Step 2: Configure OAuth Consent Screen

Fill details:

- App Name → `AI-news-summarizer`
- User Support Email → your email
- Audience → External
- Developer Contact → your email

Accept the **Google User Data Policy**.

---

### Step 3: Create OAuth Credentials

Go to:

```
APIs & Services → Credentials
```

Create:

```
OAuth Client ID
```

Select:

```
Web Application
```

Paste **n8n OAuth Redirect URL**.

Copy:

```
Client ID
Client Secret
```

Paste them inside **n8n Gmail credentials**.

---

# 📧 Gmail Node Configuration

Inside the **Gmail Send Node**:

```
To: your-email@gmail.com
Subject: Today's AI News for you
Email Type: Text
Message: {{ $json.text }}
```

---

# 🧪 Testing the Workflow

Test the workflow manually before activating.

### Testing Checklist

- Execute workflow manually
- Verify RSS feeds fetch correctly
- Check Merge and Aggregate nodes
- Confirm AI summary generation
- Verify email delivery

---

# 🚀 Final Workflow

```
Schedule Trigger
        ↓
RSS Feed Read (AI News)
        ↓
RSS Feed Read (Tech News)
        ↓
Merge Node
        ↓
Aggregate Node
        ↓
Gemini AI Summarization
        ↓
Gmail Send Node
```

---

# 🛠 Technologies Used

- **n8n** – Workflow Automation
- **Google Gemini AI** – AI Summarization
- **RSS Feeds** – News Data Source
- **Gmail API** – Email Delivery
- **Google Cloud** – OAuth Authentication

---

# 🎯 Key Benefits

✔ Automated news monitoring  
✔ AI-powered article summarization  
✔ Multi-source news aggregation  
✔ Daily email newsletter delivery  
✔ Saves **30+ minutes every day**

---

# 🚀 Future Improvements

- Slack or Telegram notifications
- AI personalized news filtering
- Multi-language summaries
- News categorization using AI
- Weekly or monthly digest generation
