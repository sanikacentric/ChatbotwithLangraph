# Chatbot with LangGraph — Stateful Conversational AI Agent

A production-ready conversational chatbot built with LangGraph featuring persistent memory, tool use, streaming responses, and human-in-the-loop capabilities. Implements stateful multi-turn conversations using LangGraph's graph-based workflow engine.

## Description

This chatbot goes beyond simple prompt-response patterns by implementing a stateful LangGraph workflow. The graph maintains conversation history, manages tool call results, handles errors gracefully, and supports interrupts for human approval of sensitive actions.

The implementation includes an advanced memory system with short-term conversation buffer and long-term knowledge retrieval, enabling the chatbot to maintain context across sessions and reference past interactions.

## Features

- **Stateful Conversation** — LangGraph state machine maintains full conversation history across turns
- - **Tool Use** — Integrated tools for web search, code execution, calculator, and custom APIs
  - - **Persistent Memory** — SQLite-backed checkpointing for conversation persistence across restarts
    - - **Human-in-the-Loop** — Interrupt nodes for human approval before executing sensitive tool calls
      - - **Streaming** — Real-time token streaming with intermediate thinking steps visible
        - - **Error Handling** — Graceful error recovery with retry logic and fallback responses
          - - **Multi-Modal** — Support for text, image, and file inputs via GPT-4V and Claude Vision
            - - **FastAPI Backend** — RESTful API with WebSocket streaming for frontend integration
             
              - ## Architecture
             
              - ```
                User Message
                     |
                     v
                Message Handler (LangGraph Node)
                     |
                     v
                LLM Call (GPT-4 / Claude)
                     |
                     +--> Tool Calls? --> Tool Executor Node
                     |                          |
                     |                    Tool Results
                     |                          |
                     +<-------------------------+
                     |
                     v
                Response Formatter
                     |
                     v
                Checkpoint (SQLite persistence)
                     |
                     v
                Streamed Response to User
                ```

                ## Tech Stack

                | Component | Technology |
                |-----------|-----------|
                | Agent Framework | LangGraph |
                | LLM | OpenAI GPT-4, Anthropic Claude |
                | Memory | LangGraph SqliteSaver |
                | API | FastAPI + WebSocket |
                | Tools | Tavily Search, Python REPL |
                | Frontend | Streamlit / React |
                | Language | Python 3.10+ |

                ## Setup Instructions

                ### Prerequisites

                - Python 3.10+
                - - OpenAI API key
                 
                  - ### Installation
                 
                  - ```bash
                    git clone https://github.com/sanikacentric/ChatbotwithLangraph.git
                    cd ChatbotwithLangraph
                    pip install langchain langgraph langchain-openai fastapi uvicorn streamlit
                    ```

                    ### Configuration

                    ```bash
                    export OPENAI_API_KEY="your-openai-api-key"
                    export TAVILY_API_KEY="your-tavily-api-key"
                    ```

                    ### Run the Chatbot

                    ```python
                    from langgraph.checkpoint.sqlite import SqliteSaver
                    from graph import build_chatbot_graph

                    memory = SqliteSaver.from_conn_string("chat_history.db")
                    graph = build_chatbot_graph(checkpointer=memory)

                    config = {"configurable": {"thread_id": "user-123"}}
                    result = graph.invoke({"messages": [("user", "Hello! What can you help me with?")]}, config)
                    print(result["messages"][-1].content)
                    ```

                    ### Run FastAPI Server

                    ```bash
                    uvicorn api:app --reload --port 8000
                    ```

                    ### Run Streamlit UI

                    ```bash
                    streamlit run app.py
                    ```

                    ## License

                    MIT License
