# BrightPath HR System Portal

An integrated HR System Portal that combines employee leave request workflows and an AI-powered HR Chatbot into a single, seamless web interface.

## 🔗 Live Demo
- **HR System Portal:** [https://youssefhelmy45.github.io/hr-system/](https://youssefhelmy45.github.io/hr-system/)

---

## 🚀 Key Features & Architecture

### 1. Main HR Portal (SPA)
- Single Page Application (SPA) providing a unified UI/UX for all HR services.
- Easily scalable structure allowing new features/links to be added seamlessly.

### 2. HR Chatbot (AI & Vector DB Integration)
- Powered by an **n8n AI Workflow**.
- Connected to **Qdrant Vector Database** for storing and retrieving knowledge from the Employee Handbook (v4.2).
- Delivers real-time, context-aware responses and displays source references.

### 3. Leave Request Workflow
- Integrated employee leave request system.
- Connected via **Google Forms & Google Apps Script** to trigger automated n8n notification workflows.

---

## 🛠️ Tech Stack
- **Frontend:** HTML5, CSS3, JavaScript (Vanilla)
- **Automation / Backend Workflows:** n8n Cloud
- **Vector Database:** Qdrant Cloud
- **Integrations:** Google Forms, Google Apps Script
- **Hosting:** GitHub Pages
