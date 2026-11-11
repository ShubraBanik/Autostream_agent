# AutoStream Social-to-Lead Agent

An AI-powered conversational agent for **AutoStream**, a fictional SaaS video editing platform for content creators. 

---

## What It Does

- Detects user intent — greeting, product inquiry, or high-intent to buy
- Answers pricing and policy questions using a local JSON knowledge base (RAG)
- Collects lead details (Name, Email, Platform) progressively when high intent is detected
- Triggers a mock lead capture function only after all three fields are collected and validated
- Retains full conversation memory across multiple turns using LangGraph state

---

## Tech Stack

- **Language:** Python 3.9+
- **Framework:** LangGraph (LangChain)
- **LLM:** LLaMA 3.1 8B Instant via Groq API
- **Knowledge Base:** Local JSON file (RAG)
- **Environment:** Google Colab

---

## Example Conversation

```
You: Hi there!
Agent: Hey! Welcome to AutoStream...

You: What plans do you offer?
Agent: We have two plans — Basic at $29/month and Pro at $79/month...

You: What is included in the Pro plan?
Agent: The Pro plan includes unlimited videos, 4K resolution, AI captions...

You: I want to sign up for the Pro plan for my YouTube channel
Agent: Great choice! Could I start with your name?

You: Aman
Agent: Nice to meet you, Aman! What is your email address?

You: aman@gmail.com
Agent: Almost there! Which creator platform do you mainly publish on?

You: YouTube
Agent: You are all set, Aman! Welcome to AutoStream!

✅ Lead captured successfully: Aman, aman@gmail.com, YouTube
```

---

## How to Run

1. Open the notebook in Google Colab
2. Upload `knowledge_base.json` to the Colab file panel
3. Get a free API key from [console.groq.com](https://console.groq.com)
4. Set your API key in Cell 2
5. Run all cells top to bottom
6. Start chatting when prompted with `You:`

---

## Why LangGraph

LangGraph was chosen because it provides explicit, inspectable state machines where each node has a clearly defined role — intent detection, response generation, and lead collection. This makes the workflow easy to control, debug, and extend. AutoGen was not chosen as its multi-agent model adds unnecessary complexity for a single-agent linear workflow like this.

---

## State Management

The agent uses a TypedDict-based state that persists across every conversation turn. It tracks the full message history, current intent, collected lead fields, which field is currently being asked for, and whether the lead has already been captured. This ensures the lead capture tool is never triggered prematurely and the conversation always resumes from the correct step.

---

## LLM Note

The agent was built using LLaMA 3.1 via Groq instead of the suggested models due to API credit limitations. The agent is fully LLM-agnostic and can be switched to GPT-4o-mini, Gemini 1.5 Flash, or Claude 3 Haiku by changing only the `get_llm()` function.

---

## WhatsApp Deployment

To deploy on WhatsApp, the agent would be wrapped in a FastAPI webhook server connected to the Meta WhatsApp Business Cloud API. Each incoming message would be routed to the agent using the user's WhatsApp ID as a session key. Conversation state would be stored in Redis with a TTL so each user maintains their own thread. The agent's reply would be sent back via the WhatsApp Cloud API after each turn.
