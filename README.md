# n8n Workflow: AIRS Agent

**Version:** 3.1 
**Name in n8n:** Prisma AIRS

## Overview

This n8n workflow implements a conversational AI agent with a focus on security and guided tool setup. It uses a Langchain Agent powered by an Google Vertex AI (but you can change that out). 

Before the agent processes user input and before the agent's response is sent back, the content is scanned by the Palo Alto Networks AIRS (AI-Powered Runtime Security) API to detect and block malicious content, prompt injections, sensitive data, etc.

### Prisma AIRS

We start with a basic AI Agent whose primary initial goal is to be able to convert contents to uppercase:
1.  It has "Simple Memory" to the agent to remember the conversation
2.  A `toUppercase` tool

Here is the layout of the basic agent:

![Basic Agent](images/Basic%20Agent.png)

To add security we add the AI Agent with Prisma AIRS. Protecting both the input and the output of the agent (createing a sandwitch effect).


Here is the layout with Security surrounding the Agent:

![n8n layout of Prisma AIRS standalone example](images/Prisma%20AIRS.png)

## Key Features

* **Conversational Interface:** Triggers on an incoming chat message. There is also a web hook for an API call.
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

### Prisma AIRS Example
This node has 2 parts, 
* The standard workflow with the AI Agent sandwitched by the Prisma AIRS functionality (blue/purple blocks).
* The node that can be called repeatibly (yellow block - confirm you are referencing this in your standard block). This will call the Prisma AIRS API and replaces the json responses to plain english responses. It also will stack responses if there are multiple triggers.
* If you enable masking, it will still return the response with the masked fields.

![n8n layout of Prisma AIRS Example](images/Prisma%20AIRS%20Example.png)

### Prisma AIRS MCP Server

This is a N8N replica of the Python SDK MCP Server.

![MCP Server Example](images/Prisma%20AIRS%20MCP%20Server.png)

You attach to the MCP Server which is built off your n8n Server (e.g. as my n8n server is on my laptop I connect to the localhost: http://localhost:5678/mcp/airs-mcp/sse). For safety I am using a bearer token.

The Server (the yellow block) advertises is functions to the client (in the green block) and you can run scans and get results and reports. 

In this example I have a test agent (green block) which can test the results. 

I have seperated the function of the server from the execution via workflow call (blue block)


