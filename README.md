# BrightPath HR System

An HR portal combining leave management, an AI handbook assistant, job position management, and automated CV screening.

Built by **Youssef Helmy** using **n8n**, Google Workspace, and AI.

## Project Links

- [Live HR Portal](https://youssefhelmy45.github.io/hr-system/)
- [GitHub Repository](https://github.com/youssefhelmy45/hr-system)
- [Downloadable n8n Workflows](https://github.com/youssefhelmy45/hr-system/tree/main/workflows)

## Features

### Employee Leave Management

Employees submit leave requests through Google Forms. The n8n workflow checks their leave balance and required attachments, requests manager approval, and updates the request status in Google Sheets.

Approved requests deduct the requested days from the appropriate leave balance.

### AI HR Handbook Assistant

A retrieval-augmented generation (RAG) chatbot answers questions using the BrightPath Employee Handbook.

The workflow retrieves relevant content from Qdrant, generates an answer using an AI model, and returns handbook section references alongside the response.

### Job Position Management

HR can open and close positions through one Google Form without editing n8n.

When a position opens, the system:

- Generates a unique Job ID.
- Saves the position details in Google Sheets.
- Creates application and referral links pre-filled with the Job ID.
- Emails the Job ID and both links to HR.

When a position closes, its status changes to `Closed`. Requests to close an unknown Job ID produce an email notification without modifying position records.

### Automated CV Screening

Applications arrive through two Google Forms:

- **Job Application:** external applicants.
- **Employee Referral:** employees referring candidates.

Both sources use the same screening workflow. The system:

1. Checks whether the requested position exists and is open.
2. Saves a copy of the CV in a job-specific and candidate-specific Google Drive folder.
3. Extracts text from the PDF.
4. Uses AI to extract candidate information.
5. Compares documented skills and experience with the position requirements.
6. Calculates priority and saves the application in Google Sheets.
7. Emails HR when the candidate has High priority.

Applications for closed or unknown positions receive an email notification and do not proceed to CV analysis.

### Dynamic Open Positions

The portal fetches open positions from an n8n webhook and displays:

- Job title.
- Department.
- Job ID.
- A **Refer a Candidate** button with the correct pre-filled referral link.

The list refreshes on page load, through a manual Refresh button, and every minute while the portal is visible. Opening or closing a position requires no website code changes.

## Screening and Priority

The AI assessment uses this rubric:

| Criterion | Points |
|---|---:|
| Documented required skills | 60 |
| Experience against the minimum requirement | 25 |
| Documented nice-to-have skills | 15 |

If no nice-to-have skills are specified, their 15 points are allocated to required skills.

| Score | Priority | Action |
|---|---|---|
| 75–100 | High | Save the application and notify HR |
| 50–74 | Medium | Save the application |
| 0–49 | Low | Save the application |

External and referred candidates receive the same assessment criteria, with no referral bonus.

Scores support preliminary HR review. They do not trigger automatic hiring or rejection decisions. Missing CV information is treated as undocumented evidence.

## Included n8n Workflows

The `workflows/` folder contains the three workflows added for the recruitment extension:

| Workflow | Purpose |
|---|---|
| HR - Position Management | Open and close positions, generate links, and notify HR |
| HR - CV Screening | Process external applications and referrals through shared screening logic |
| HR - Open Positions API | Return open positions for the portal |

The portal also provides access to the existing leave management and HR chatbot services.

## Technology Stack

- **Automation:** n8n
- **Frontend:** HTML, CSS, and JavaScript
- **Hosting:** GitHub Pages
- **Forms:** Google Forms
- **Data storage:** Google Sheets
- **CV storage:** Google Drive
- **Notifications:** Gmail
- **CV analysis:** AI language models
- **Handbook retrieval:** Qdrant and OpenAI embeddings

## Setup on Another Account

1. Download the JSON files from `workflows/`.
2. Import each file into n8n using **Import from File**.
3. Configure Google Sheets, Google Drive, Gmail, and AI model credentials.
4. Replace spreadsheet IDs, folder IDs, form links, pre-filled field IDs, and notification recipients with your own configuration.
5. Verify that the forms and spreadsheet column names match the workflow mappings.
6. Update the frontend webhook URLs and configure CORS for your website origin.
7. Publish the workflows and test them with sample applications.

Workflow exports do not provide working service connections on another account; credentials and resource settings must be configured.

## Validated Scenarios

- Opening a position and receiving its application and referral links.
- Generating different Job IDs for multiple positions in one execution.
- Closing an existing position.
- Attempting to close an unknown Job ID.
- Processing an external application automatically.
- Processing an employee referral automatically.
- Saving a Low-priority candidate.
- Generating a High-priority assessment from a suitable test CV and notifying HR.
- Stopping CV processing for closed or unknown positions.
- Displaying newly opened positions on the portal.
- Removing closed positions from the portal.

## Current Scope

CV uploads are limited to PDF files. The current extraction flow expects readable text in the PDF; scanned, image-only CVs would require an OCR extension.

Google API rate limits can temporarily interrupt processing. These errors require delayed retries.

The live demo uses test applications and fictional CV data for demonstration.
