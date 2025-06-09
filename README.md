# n8n Workflow: AIRS Agent

**Version:** 2.0 (based on the provided JSON)
**Name in n8n:** Prisma AIRS Demo

## Overview

This n8n workflow implements a conversational AI agent with a focus on security and guided tool setup. It uses a Langchain Agent powered by an Ollama LLM (Llama3.2). Before the agent processes user input and before the agent's response is sent back, the content is scanned by the Palo Alto Networks AIRS (AI-Powered Runtime Security) API to detect and block malicious content, prompt injections, sensitive data, etc.

The agent's primary initial goal is to be able to convert contents to uppercase:
1.  It has "Simple Memory" to the agent to remember the conversation
2.  A toUppercase tool


## Key Features

* **Conversational Interface:** Triggers on an incoming chat message.
* **Security First (AIRS Integration):**
    * **Prompt Scanning:** User input is first sent to the a Node that calls the Palo Alto AIRS API. If deemed malicious or problematic, the workflow stops and informs the user in readable format.
    * **Response Scanning:** The agent's generated response is sent to the same node . If the response is flagged, a block message is sent instead of the agent's original response.
* **Langchain Agent:** Utilizes `@n8n/n8n-nodes-langchain.agent` for conversational logic.
* **LLM Backend:** Uses an Ollama Chat Model (`llama3.2:latest`) for generating responses.
    * *Note: An AWS Bedrock Chat Model node is present but not connected in the main flow.*
* **Memory:** Employs `@n8n/n8n-nodes-langchain.memoryBufferWindow` for short-term conversation memory.
* **toUppercase:**
    * Converts the input to Uppercase
* **Step-by-Step User Guidance:** The agent's system prompt is designed to walk the user through the necessary configuration steps for its memory and tools.
* **Contextual Constraint

## Key Setup requirements
* AIRS API token for the Header Authentication
* change the profile in the AIRS request and response field edits from example-profile to your specific profile

## Design considerations
* The Prisma_AIRS_Node must also be included as it is called by PRISM_AIRS_Example (In Prisma_AIRS node it is a standalone node)
