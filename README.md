# n8n Workflow: AIRS Agent

**Version:** 1.0 (based on the provided JSON)
**Name in n8n:** AIRS Agent

## Overview

This n8n workflow implements a conversational AI agent with a focus on security and guided tool setup. It uses a Langchain Agent powered by an Ollama LLM (Llama3.2). Before the agent processes user input and before the agent's response is sent back, the content is scanned by the Palo Alto Networks AIRS (AI-Powered Runtime Security) API to detect and block malicious content, prompt injections, sensitive data, etc.

The agent's primary initial goal is to guide the user through configuring its own tools, specifically:
1.  Adding "Simple Memory" to the agent.
2.  Adding and configuring a "Google Calendar" tool (credentials, resource type, operation, calendar selection, and AI-defined fields for time).

Once configured, the agent can interact with the Google Calendar (e.g., to fetch events for the next day).

## Key Features

* **Conversational Interface:** Triggers on an incoming chat message.
* **Security First (AIRS Integration):**
    * **Prompt Scanning:** User input is first sent to the Palo Alto AIRS API. If deemed malicious or problematic, the workflow stops and informs the user.
    * **Response Scanning:** The agent's generated response is also sent to the AIRS API. If the response is flagged, a block message is sent instead of the agent's original response.
* **Langchain Agent:** Utilizes `@n8n/n8n-nodes-langchain.agent` for conversational logic.
* **LLM Backend:** Uses an Ollama Chat Model (`llama3.2:latest`) for generating responses.
    * *Note: An AWS Bedrock Chat Model node is present but not connected in the main flow.*
* **Memory:** Employs `@n8n/n8n-nodes-langchain.memoryBufferWindow` for short-term conversation memory.
* **Tool Integration (Google Calendar):**
    * Guides the user through setting up the `n8n-nodes-base.googleCalendarTool`.
    * Once set up, it can perform operations like "Get Many" events.
* **Step-by-Step User Guidance:** The agent's system prompt is designed to walk the user through the necessary configuration steps for its memory and tools.
* **Contextual Constraint

## Key Setup requirements
* AIRS API token for the Header Authentication
* change the profile in the AIRS request and response field edits from example-profile to your specific profile
* Provide your gmail calendar integration, or provide a different tool and adjust the adjent.
