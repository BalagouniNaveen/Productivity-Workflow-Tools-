Absolutely! Here's a **point-by-point, detailed step-by-step process** to **build a GitHub PR Assistant** project for the Kiro hackathon under the **Productivity & Workflow Tools** category.

---

# 🚀 Project: GitHub PR Assistant

> A tool that analyzes GitHub pull requests and suggests AI-generated code review comments.

---

## 🎯 GOAL

Use **Kiro** to build a working app that:

* Reads a GitHub pull request
* Analyzes the diff using an LLM (like OpenAI)
* Suggests code review comments
* Optionally posts those suggestions to the PR

---

## ✅ Step-by-Step Project Plan

---

### **1️⃣ Ideation and Requirements**

* 🧠 Project Name: `GitHub PR Assistant`
* 📌 Goal: Reduce time spent on code reviews by suggesting helpful comments automatically.
* 🛠️ Tech stack:

  * **Backend:** Python (FastAPI)
  * **AI Integration:** OpenAI GPT API
  * **External API:** GitHub REST API
  * **Frontend (optional):** React (or just Postman/cURL for testing)
  * **Kiro components:** `.kiro/specs/`, `.kiro/hooks/`

---

### **2️⃣ Set up Project Folder**

```bash
mkdir github-pr-assistant
cd github-pr-assistant
git init
```

Create the following structure:

```
github-pr-assistant/
├── backend/
├── frontend/ (optional)
└── .kiro/
    ├── specs/
    └── hooks/
```

---

### **3️⃣ Create a Kiro Spec**

**Path:** `.kiro/specs/github-pr-assistant.yaml`

```yaml
name: github-pr-assistant
description: AI-powered assistant that analyzes GitHub pull requests and suggests review comments.
features:
  - Accept GitHub PR URL input
  - Fetch PR diff and metadata
  - Use LLM to generate code review suggestions
  - Display or post suggestions to GitHub
technologies:
  - backend: Python (FastAPI)
  - AI: OpenAI API
  - Integration: GitHub API
```

---

### **4️⃣ Set Up Backend with Kiro**

> **Prompt Kiro:**
> *"Generate a FastAPI backend with a POST endpoint `/analyze-pr` that accepts a JSON payload with a GitHub PR URL."*

💡 This generates:

* `main.py`
* Basic FastAPI server
* `/analyze-pr` endpoint

---

### **5️⃣ GitHub PR Fetcher Logic**

> **Prompt Kiro:**
> *"Generate Python code to extract repo owner, name, and PR number from a GitHub PR URL and fetch its diff using GitHub API."*

📄 Example Code (add to `backend/github_fetcher.py`):

```python
import re
import requests

def extract_pr_details(pr_url):
    match = re.match(r"https://github.com/(.+)/(.+)/pull/(\d+)", pr_url)
    if not match:
        raise ValueError("Invalid PR URL")
    return match.group(1), match.group(2), match.group(3)

def fetch_pr_diff(owner, repo, pr_number, token):
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3.diff"
    }
    url = f"https://api.github.com/repos/{owner}/{repo}/pulls/{pr_number}"
    response = requests.get(url, headers=headers)
    return response.text
```

---

### **6️⃣ AI-Powered Suggestions**

> **Prompt Kiro:**
> *"Use OpenAI API to analyze a code diff and return AI-generated review suggestions."*

📄 Example Code (add to `backend/suggestion_engine.py`):

```python
import openai
import os

openai.api_key = os.getenv("OPENAI_API_KEY")

def get_suggestions(diff_text):
    prompt = f"""
You are a senior developer. Analyze the following GitHub PR diff and suggest code review comments for readability, performance, and correctness.

Diff:
{diff_text}
"""
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message["content"]
```

---

### **7️⃣ Wire It Together in FastAPI**

📄 `backend/main.py`

```python
from fastapi import FastAPI, Request
from pydantic import BaseModel
import os

from github_fetcher import extract_pr_details, fetch_pr_diff
from suggestion_engine import get_suggestions

app = FastAPI()

class PRRequest(BaseModel):
    pr_url: str

@app.post("/analyze-pr")
def analyze_pr(req: PRRequest):
    owner, repo, pr_number = extract_pr_details(req.pr_url)
    token = os.getenv("GITHUB_TOKEN")
    diff = fetch_pr_diff(owner, repo, pr_number, token)
    suggestions = get_suggestions(diff)
    return {"suggestions": suggestions}
```

---

### **8️⃣ Create `.kiro/hooks/` (Optional)**

📄 `.kiro/hooks/on-analyze-pr.py`

```python
def onAnalyzePR(pr_url):
    # Optional hook called when analyze-pr is triggered
    # This can automate saving logs, metrics, or testing
    pass
```

---

### **9️⃣ Test API Locally**

1. Add `.env` with your tokens:

```
OPENAI_API_KEY=your_openai_key
GITHUB_TOKEN=your_github_token
```

2. Install dependencies:

```bash
pip install fastapi uvicorn openai requests python-dotenv
```

3. Run server:

```bash
uvicorn main:app --reload
```

4. Test using cURL or Postman:

```bash
curl -X POST http://127.0.0.1:8000/analyze-pr \
  -H "Content-Type: application/json" \
  -d '{"pr_url":"https://github.com/user/repo/pull/42"}'
```

---

### **🔟 (Optional) Add React Frontend**

> Prompt Kiro:
> *"Generate a React page with a form to input PR URL and display response from /analyze-pr endpoint."*

* Add loading state
* Show AI review comments below the form

---

### **1️⃣1️⃣ Record a 3-Minute Demo Video**

**Include answers:**

* How did you use Kiro to build this?
* What was the most helpful code Kiro generated?
* Show `.kiro/specs`, FastAPI working, sample output

📺 Upload to YouTube, Vimeo, or Facebook Video

---

### **1️⃣2️⃣ Final Submission Checklist**

✅ `.kiro/` folder included
✅ Public GitHub repo
✅ Open source license (MIT)
✅ README with:

* Project overview
* How Kiro was used
* Tech stack
* Demo video link
  ✅ Category: Productivity & Workflow Tools

---

## ✅ Sample README Outline

```markdown
# GitHub PR Assistant

AI-powered assistant that analyzes GitHub pull requests and suggests helpful code review comments.

## ✨ Features
- PR diff analysis using GitHub API
- AI-generated review comments (via OpenAI)
- Optional frontend form

## 🤖 Kiro Usage
- Used `.kiro/specs/` to define the project
- Generated backend boilerplate with FastAPI
- Created GitHub fetcher and LLM code with Kiro prompts
- Used `.kiro/hooks/` for automation

## 📽 Demo Video
[Watch here](https://youtube.com/...)

## 🛠️ Stack
- FastAPI + Python
- GitHub API
- OpenAI GPT
- Kiro IDE
```

---

Would you like me to generate the full starter code ZIP for this project or push it to a GitHub template?

Let me know!






Awesome! Let's go **step-by-step to build the "GitHub PR Assistant"** project using **Kiro** for the **Productivity & Workflow Tools** category of the hackathon.

---

# 🚀 Project: GitHub PR Assistant

> Analyze pull requests and auto-suggest review comments using AI
> Built with: **Kiro** (specs, hooks, codegen) + GitHub API + OpenAI (or local LLM)

---

## 🧠 High-Level Features

| Feature             | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| 🧠 Analyze PRs      | Fetch code diffs and metadata from GitHub PRs                          |
| 💬 Suggest Comments | AI suggests review comments for code quality, performance, readability |
| 📨 Post to GitHub   | Posts comments back to the PR                                          |
| 🛠️ UI Dashboard    | (Optional) Web UI to paste PR links and view suggestions               |

---

## ✅ Step-by-Step Project Plan

---

## **STEP 1: Create Project Folder & Setup Git**

```bash
mkdir github-pr-assistant
cd github-pr-assistant
git init
```

---

## **STEP 2: Create Kiro Spec (describe your idea)**

Make a folder `.kiro/specs/` and create:

📄 `.kiro/specs/github-pr-assistant.yaml`

```yaml
name: github-pr-assistant
description: AI-powered assistant that reviews GitHub PRs and suggests code review comments.
features:
  - Fetch PR data (diffs, titles, descriptions)
  - Use LLM to suggest review comments
  - Post suggestions to GitHub using API
technologies:
  - backend: Python (FastAPI)
  - integration: GitHub API, OpenAI API
```

---

## **STEP 3: Use Kiro to Generate Backend Boilerplate**

Prompt Kiro:

> “Generate a FastAPI backend with an endpoint `/analyze-pr` that accepts a GitHub PR URL and returns suggested comments.”

Kiro will create:

* `main.py` with FastAPI routes
* `/analyze-pr` logic placeholder

---

## **STEP 4: Add GitHub PR Fetcher (with Kiro’s help)**

Prompt Kiro:

> “Generate Python code that uses GitHub API to fetch the diff and metadata of a pull request given the PR URL.”

You may need:

* GitHub token via `.env` or secret manager

Sample code:

```python
import requests
import re

def get_pr_diff(pr_url: str, github_token: str):
    pattern = r"https://github.com/(.+)/pull/(\d+)"
    match = re.match(pattern, pr_url)
    if not match:
        raise ValueError("Invalid PR URL")

    repo, pr_number = match.groups()
    headers = {
        "Authorization": f"token {github_token}",
        "Accept": "application/vnd.github.v3.diff"
    }
    url = f"https://api.github.com/repos/{repo}/pulls/{pr_number}"
    diff_url = f"{url}"
    response = requests.get(diff_url, headers=headers)
    return response.text
```

---

## **STEP 5: Integrate LLM for Review Suggestions**

Prompt Kiro:

> “Use OpenAI API to analyze the code diff and return suggestions for code review comments.”

Sample Python code:

```python
import openai

def suggest_review_comments(diff_text: str) -> list:
    prompt = f"""
    You are a senior code reviewer. Suggest detailed, helpful code review comments for the following Git diff:
    {diff_text}
    """
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3
    )
    return response.choices[0].message['content']
```

---

## **STEP 6: Post Comments to GitHub (Optional)**

Prompt Kiro:

> “Generate Python code to post a comment to a PR using GitHub API.”

Example:

```python
def post_comment(repo: str, pr_number: int, comment: str, github_token: str):
    url = f"https://api.github.com/repos/{repo}/issues/{pr_number}/comments"
    headers = {
        "Authorization": f"token {github_token}",
        "Accept": "application/vnd.github.v3+json"
    }
    payload = {"body": comment}
    response = requests.post(url, json=payload, headers=headers)
    return response.status_code == 201
```

---

## **STEP 7: Add Kiro Agent Hook (Optional)**

Inside `.kiro/hooks/` create:

📄 `.kiro/hooks/on-analyze-pr.py`

```python
# Kiro hook to run when analyze-pr endpoint is called
def onAnalyzePR(pr_url: str):
    diff = get_pr_diff(pr_url, GITHUB_TOKEN)
    suggestions = suggest_review_comments(diff)
    return suggestions
```

---

## **STEP 8: Test Your App**

Create a simple test script or use Postman to send a POST request:

```bash
curl -X POST http://localhost:8000/analyze-pr \
  -H "Content-Type: application/json" \
  -d '{"pr_url": "https://github.com/yourname/repo/pull/42"}'
```

---

## **STEP 9: (Optional) Frontend UI with Kiro**

Prompt Kiro:

> “Generate a simple React form to enter a GitHub PR URL and view AI-suggested review comments.”

Kiro will scaffold:

* React + Tailwind
* POST request to backend
* Render suggestions

---

## **STEP 10: Record 3-Minute Demo Video**

Video must answer:

* How did you structure your conversations with Kiro?
* What was the most impressive code generation Kiro helped with?

🛠 Use tools like:

* OBS Studio / Loom
* Show `.kiro/specs/`, hook files, app running live

---

## **STEP 11: Make GitHub Repo Public**

Create a repo:

```bash
git remote add origin https://github.com/yourusername/github-pr-assistant
git push -u origin main
```

Add:

* ✅ LICENSE (MIT recommended)
* ✅ `.kiro/` directory with specs and hooks
* ✅ README.md (with Kiro usage write-up)

---

## ✅ Submission Checklist

| Requirement                                      | Status |
| ------------------------------------------------ | ------ |
| `.kiro/` folder in root                          | ✅      |
| Public repo with OSI license                     | ✅      |
| 3-min demo video                                 | ✅      |
| README with Kiro write-up                        | ✅      |
| Identify category: Productivity & Workflow Tools | ✅      |

---

## 🚀 Want to Build It Together?

I can now:

* ✅ Auto-generate the full `.kiro/specs` and `main.py` for you
* ✅ Help set up the React UI if needed
* ✅ Help you test or polish your demo video script

Would you like me to generate the code structure for you now?
