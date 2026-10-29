# Project Delivery Assistant – Powered by AI

**MUJ–Deloitte | Gen AI for Business | Capstone Topic 02: Consulting or Project Delivery Assistant**

## 1. Project Overview

This project uses **n8n** and **Google Gemini** to analyze a fictional software project plan and meeting transcript. It compares planned milestones with meeting updates, drafts meeting minutes, identifies action items and delivery risks, and produces a project status report. A Project Manager reviews the AI-generated report and selects **Approved** or **Needs Changes** before the workflow prepares downloadable reports.

The sample project is **Enterprise Customer Portal Development (ECP-2026-01)**. All sample project details are fictional.

## 2. Business Problem and Objective

Project Managers and Team Leads spend time consolidating project plans, meeting discussions, actions, and delivery risks. This proof of concept demonstrates how GenAI can draft these outputs consistently, while keeping a human reviewer responsible for checking accuracy and approving the result.

### Note: Please have look on very short video showing full work flow: `06_Screenshots/Workflow_Video.mp4`

## 3. Tools and Technologies

- **n8n Cloud** – form submission, PDF/TXT extraction, workflow orchestration, human review, and file generation.
- **Google Gemini API** – meeting and project analysis using the included prompt.
- **Markdown / HTML / JSON** – downloadable report and review-record formats.

No additional `.env.example` file is needed for this capstone. API credentials must be configured **inside n8n**, not placed in this README or exported JSON.

## 4. Project Files

```text
Project_Delivery_Assistant/
├── 01_Project_Overview/
│   └── Capstone_Overview.pdf
├── 02_Workflow/
│   └── Project_Delivery_Workflow.json
├── 03_Input_Documents/
│   ├── Project_Plan.pdf
│   ├── Meeting_Transcript_01.txt
│   ├── Meeting_Transcript_02.txt
│   └── README                  # Notes supplied with sample data
├── 04_AI_Prompts/
│   └── Gemini_Prompt.txt
├── 05_Generated_Reports/
│   ├── Project_Status_Report_In_Markdown.md
│   ├── Project_Status_Report_In_Html.html
│   └── Project_Review_Result_In_Json.json
├── 06_Screenshots/
│   ├── Workflow.png
│   ├── AI_Report.png
│   ├── Manager_Review.png
│   └── Final_Outputs.png
└── README.md
```

## 5. Setup and Import Instructions

1. Sign in to an **n8n** instance with access to the **Form Trigger**, **Extract From File**, **Merge**, **Markdown**, **Edit Fields**, **Google Gemini**, **Form**, and **Convert to File** nodes.
2. Create a new workflow and choose **Import from File** (menu wording may vary by n8n version).
3. Import `02_Workflow/Project_Delivery_Workflow.json`.
4. Create or select a **Google Gemini API credential** in n8n. Obtain your own key from [Google AI Studio](https://aistudio.google.com/api-keys) and enter it only in the credential form. Do **not** include the key in shared files or screenshots.
5. Open the **Message a model** node, select that Gemini credential and a model available to your account.
6. Inspect the imported nodes for missing credentials or required settings, then save the workflow.
7. Use the **Test URL** of the **On form submission** node for a manual test. For production use, publish the workflow and use its Production URL, if applicable.

**Note:** Cloud accounts, API keys, hosted services, and running instances are not included in the ZIP. An evaluator will need their own n8n instance and Gemini API access. Model availability and costs may vary.

## 6. How to Run the Demonstration

1. In n8n, start a test execution from the workflow editor.
2. Open the **On form submission** test form.
3. Upload `03_Input_Documents/Project_Plan.pdf` in the **Project Plan** field.
4. Upload `03_Input_Documents/Meeting_Transcript_01.txt` in the **Meeting Transcript** field and submit.
5. n8n extracts text from both files, prepares `project_plan_text` and `meeting_text`, and combines them with the **Merge** node.
6. **Gemini – Message a model** analyzes the combined content and generates a draft meeting summary, action items, risks/blockers, and project status report.
7. The **Convert Markdown to HTML** node formats the draft for the **Project Manager Review** form.
8. In the review form, enter the reviewer name, select **Approved** or **Needs Changes**, add comments, and submit.
9. **Prepare Final Report** includes the reviewer decision and comments in the report.
10. The final branches generate Markdown, HTML, and JSON files. Open the relevant **Convert to File** node's **Output → Binary** panel and choose **Download**.

To test a different meeting, repeat the process with `Meeting_Transcript_02.txt`. It describes later delays and revised *working targets*; the original approved project-plan baseline must not be silently overwritten.

**Important:** Each test execution can generate a new temporary review-form URL. Avoid reusing an old `/form-waiting/` URL. If a review form fails to open, inspect n8n's **Executions** panel and start a fresh test rather than reusing the expired link.

## 7. Expected Outputs

- **Meeting Summary (MoM):** main discussion points and decisions.
- **Action Items:** tasks, responsible owners, due dates, and reported status.
- **Risks and Blockers:** dependencies, potential delays, and suggested mitigation.
- **Project Status:** planned versus reported milestone progress and items needing attention.
- **Human Review:** reviewer name, approval decision, and comments.
- **Downloads:** `.md`, `.html`, and `.json` files containing the report/review result.

When the decision is **Needs Changes**, the generated file is a **draft requiring revision**, not an approved deliverable. This workflow records feedback; it does not automatically revise and reapprove the content.

## 8. Verification and Evidence

- `06_Screenshots/Workflow.png` – complete n8n workflow.
- `06_Screenshots/AI_Report.png` – generated AI analysis.
- `06_Screenshots/Manager_Review.png` – reviewer interface and decision.
- `06_Screenshots/Final_Outputs.png` – report-generation results.
- `05_Generated_Reports/` – example downloadable files.
- `01_Project_Overview/Capstone_Overview.pdf` – 3-page project explanation and business value.

For a complete verification, use your own API credentials, upload the sample documents, confirm the AI output, submit a review, and inspect the three final file outputs.

## 9. Limitations and Safety

- AI-generated information may be incomplete or inaccurate; the Project Manager must validate the results.
- The MVP uses uploaded sample files, not live integrations with Jira, Teams, or Confluence.
- The comparison is prompt-based; it does not implement a deterministic task-matching engine or RAG database.
- Sample content is fictional. Do not upload confidential employer documents without authorization.
- Never commit API keys, secrets, or credential exports to a public repository.
