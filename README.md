# n8n Workflow: AIRS Agent

**Version:** 2.0 (based on the provided JSON)
**Name in n8n:** Prisma AIRS Demo

## Overview

This n8n workflow implements a conversational AI agent with a focus on security and guided tool setup. It uses a Langchain Agent powered by an Google Vertex AI (but you can change that out). 

Before the agent processes user input and before the agent's response is sent back, the content is scanned by the Palo Alto Networks AIRS (AI-Powered Runtime Security) API to detect and block malicious content, prompt injections, sensitive data, etc.

### Prisma AIRS

We start with a basic AI Agent whose primary initial goal is to be able to convert contents to uppercase:
1.  It has "Simple Memory" to the agent to remember the conversation
2.  A `toUppercase` tool

This is what it looks like :

![Basic Agent](images/Basic%20Agent.png)

To add security we add the AI Agent with Prisma AIRS. Protecting both the input and the output of the agent (createing a sandwitch effect).


This is what it looks like with Security surrounding the Agent:

![n8n layout of Prisma AIRS standalone example](images/Prisma%20AIRS.png)

## Key Features

* **Conversational Interface:** Triggers on an incoming chat message.
* **Security First (AIRS Integration):**
    * **Prompt Scanning:** User input is first sent to the a Node that calls the Palo Alto AIRS API. If deemed malicious or problematic, the workflow stops and informs the user in readable format.
    * **Response Scanning:** The agent's generated response is sent to the same node . If the response is flagged, a block message is sent instead of the agent's original response.
* **Langchain Agent:** Utilizes `@n8n/n8n-nodes-langchain.agent` for conversational logic.
* **LLM Backend:** Uses an Google Vertex AL Chat Model (`gemini-2.0-flash-lite-001`) for generating responses - This can be changed out to Ollama if you want to run it fully local.
* **Memory:** Employs `@n8n/n8n-nodes-langchain.memoryBufferWindow` for short-term conversation memory.
* **toUppercase:** Converts the input to Uppercase


## Key Setup requirements
* AIRS API token for the Header Authentication
* change the profile in the AIRS request and response field edits from example-profile to your specific profile

# Advanced Example

For more Advanced Users, the workflow is split from the Example. It also allows combination of events (e.g. DLP + URL Filtering + Prompt Injection)
### Prisma AIRS Node 
* This is the reusable node. It returns presentable responses (you could call the API yourself and handle it seperately - this is done in Prisma AIRS Node)
![n8n layout of Prisma AIRS Node](images/Prisma%20AIRS%20Node.png)

### Prisma AIRS Example
* This node calls Prisma AIRS Node. 
![n8n layout of Prisma AIRS Example](images/Prisma%20AIRS%20Example.png)

## Design considerations
* The Prisma_AIRS_Node must also be included as it is called by PRISM_AIRS_Example (In Prisma_AIRS node it is a standalone node)

