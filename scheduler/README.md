# AI Agent with n8n

This repository shows how to build a simple **AI agent** on top of **n8n**.

An AI agent here means:
> An LLM-powered workflow that **decides the next step by itself** and **chooses tools** to reach a goal.

---
## Workflow Overview

This workflow is a chat-based AI assistant that can call tools dynamically.

![n8n scheduler flow](./n8n_scheduler.gif)

---

## How It Works

- The agent **dynamically decides the workflow order** at run time.
- It **selects and calls tools** (n8n nodes / APIs) as needed.
- It uses intermediate results to **plan the next action** until it can return an answer.

1. **Chat Trigger – “When Chat Message Received”**  
   Entry point of the workflow. It receives the user’s message from the n8n chat interface.

2. **AI Agent**  
   The “brain” of the system.  
   - Reads the incoming message and conversation context.  
   - Decides whether to answer directly or call one of the tools.  
   - Orchestrates the overall Perceive → Plan → Act → Reflect loop.

3. **LLM / Chat Model Node (e.g., OpenAI / Gemini)**  
   Provides the language understanding and reasoning.  
   The AI Agent uses this model to:  
   - Understand the user request,  
   - Plan the next action,  
   - Generate the final reply.

4. **Memory Node (Conversation / Simple Memory)**  
   Stores recent messages so the agent can:  
   - Keep context across turns,  
   - Handle follow-up questions naturally.

5. **Tool Nodes (Examples)**  
   These are the “hands” of the agent. They are registered as tools in the AI Agent.
   - **Tool 1 – Get Weather**  
     Calls an external API to fetch real-time weather for a given location.
   - **Tool 2 – Get News / RSS**  
     Fetches the latest headlines or articles from an RSS feed or another data source.

6. **Reply back through Chat Trigger**  
   The AI Agent returns a final, user-friendly answer, which is sent back to the chat UI.

---

## When to Use

- Tasks where the **required tools or order of steps change** depending on the situation:
  - AI assistant that manages your **schedule or projects**.
  - Research helper that uses **multiple tools** (web search, docs, APIs) in different combinations.

---

## Pros / Cons

**Pros**
- Flexible: can **reuse your existing tools** and adapt to many types of requests.

**Cons**
- Prone to **hallucinations and unexpected behavior**.
- Less stable and harder to debug than **fixed, deterministic workflows**.
