# Kaggle-AgenticAi-DAY-3
Day 3 (Context Engineering: Sessions &amp; Memory) 

# 🧠 Context Engineering & Memory Management for AI Agents
This repository documents the advanced patterns for building stateful, intelligent AI agents using the Google Agent Development Kit (ADK). It moves beyond simple prompt engineering to explore Context Engineering—the art of dynamically managing an LLM's finite context window to enable persistence, personalization, and long-running conversations.

# 📚 Theoretical Core Concepts
 ## Context Engineering
Large Language Models are stateless by default. Context Engineering is the process of dynamically assembling the perfect "payload" for every conversation turn.

The Metaphor: It is the mise en place for an agent—preparing all ingredients (history, facts, tools) before the "cooking" (generation) begins.

The Components:

Instructions: System prompts and behavioral guidelines.

Events (History): The raw, chronological log of what just happened.

State (Scratchpad): The current status variables (e.g., cart_is_full=True).

Knowledge (RAG/Memory): External facts or long-term user details.


# 🛠️ Hands-On Implementation Summary
We built a Stateful Agent that evolved from a simple prototype to a production-ready system.

## Phase 1: Session Management
Challenge: Agents have "goldfish memory" (RAM) and forget everything on restart.

Solution: We moved from InMemorySessionService (Volatile) to DatabaseSessionService (Persistent).

Key Insight: The Runner acts as the "Assistant," orchestrating the flow between the User, the Agent (Brain), and the SessionService (Filing Cabinet).

## Phase 2: Context Compaction (Handling Long Conversations)
Challenge: As conversations grow, they hit the "Context Window" limit, increasing cost and latency.

Solution: We implemented Automatic Compaction using the ADK's App configuration.

Code Pattern:

Python

events_compaction_config=EventsCompactionConfig(
    compaction_interval=3,  # Summarize every 3 turns
    overlap_size=1          # Keep the last turn verbatim for immediate context
)
Result: The agent automatically runs a background task to summarize old messages, keeping the context lightweight without losing the gist of the conversation.

## Phase 3: Explicit State Management (ToolContext)
Challenge: Relying on chat history for facts (like names) is inefficient and unreliable.

Solution: We created Custom Tools to explicitly read and write variables to the Session State.

The "Magic" Component: ToolContext.

Writer Tool: tool_context.state["user:name"] = "Sam"

Reader Tool: return tool_context.state.get("user:name")

#### Result: The agent now possesses precise, structured memory that persists even after the chat history is compacted.
