# Automated Gmail AI Processing Workflow (n8n)

An automated n8n workflow that fetches messages from Gmail, processes and transforms the payload using custom JavaScript, generates dynamic responses or analysis using a local **Ollama Chat Model** connected via LangChain's **Basic LLM Chain**, and automatically sends response emails back through Gmail.

## 📌 Workflow Architecture

The workflow operates in a sequential pipeline consisting of trigger, ingestion, transformation, AI processing, and dispatch stages.

![n8n Workflow Screenshot](screenshot.png)

## 🚀 Workflow Overview & Step-by-Step Breakdown

1. **Trigger: When clicking 'Execute workflow'**
   * **Type:** Manual Trigger (`On clicking 'Execute workflow'`)
   * **Purpose:** Starts the workflow execution manually on demand.

2. **Fetch Emails: Get many messages**
   * **Node Type:** Gmail Node (`getAll: message`)
   * **Purpose:** Connects to the configured Gmail account and retrieves multiple email messages based on specified search filters/query constraints.

3. **Pre-processing: Code in JavaScript**
   * **Node Type:** Code Node (`JavaScript`)
   * **Purpose:** Cleans, formats, and structures the incoming Gmail raw data (e.g., extracting subject, sender, body text, or sanitizing HTML content) into a clean format suitable for LLM prompt context.

4. **AI Generation: Basic LLM Chain + Ollama Chat Model**
   * **Node Type:** LangChain Chain (`Basic LLM Chain`) + Model Sub-node (`Ollama Chat Model`)
   * **Purpose:**
     * Receives structured text from the pre-processing step.
     * Passes the prompt context to a locally or remotely hosted **Ollama Chat Model** (e.g., Llama 3, Mistral, Gemma).
     * Processes the request (e.g., generating an automated reply, summarizing email content, or classifying customer intent).

5. **Post-processing: Code in JavaScript1**
   * **Node Type:** Code Node (`JavaScript`)
   * **Purpose:** Formats the generated output from the LLM, preparing email parameters such as `to`, `subject`, and `body` payload for dispatch.

6. **Send Email: Send a message**
   * **Node Type:** Gmail Node (`send: message`)
   * **Purpose:** Sends out the processed AI response or summary as an email via Gmail to the target recipient.

## 🛠️ Prerequisites

To run this n8n workflow, ensure you have the following set up:

1. **n8n Instance:** Running locally or self-hosted via Docker / n8n Cloud.
2. **Gmail Credentials:** OAuth2 / API credentials configured in n8n for Gmail read/write access.
3. **Ollama Instance:**
   * Ollama installed and running locally or accessible via network endpoint (`http://localhost:11434` or custom URL).
   * Desired model pulled (e.g., `ollama pull llama3`).

## ⚙️ Configuration & Setup

1. **Import Workflow:**
   * Copy the JSON configuration of your workflow.
   * Open n8n, navigate to **Workflows > Import from File / JSON**, and paste the code.

2. **Setup Gmail Integration:**
   * Authenticate your Gmail credential in both `Get many messages` and `Send a message` nodes.

3. **Configure Ollama Model:**
   * Click on the `Ollama Chat Model` node.
   * Specify the base URL (e.g., `http://localhost:11434` or host machine address).
   * Specify the target model name (e.g., `llama3`, `mistral`, `phi3`).

4. **Execute:**
   * Click **Execute Workflow** to test the full loop from message retrieval to AI processing and dispatch.

## 📂 File Structure

```
.
├── README.md        # Documentation for the n8n workflow
└── screenshot.png   # Visual overview of the workflow
```#
