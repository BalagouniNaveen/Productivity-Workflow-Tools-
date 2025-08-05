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
