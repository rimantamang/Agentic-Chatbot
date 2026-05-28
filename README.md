# 🤖 Agentic-ChatBot

An autonomous, graph-based multi-agent AI workspace built with **LangGraph** and powered by high-speed inference through **Groq LLMs**. The application features an interactive web interface built with **Streamlit**, enabling users to switch between standalone chat, web-grounded search, and automated research workflows.

---

## 🏗️ Project Architecture

The workspace is organized into a clean, modular package structure to separate graph routing logic, model orchestration, and front-end interface layers:

```text
AgenticChatBot/
│
├── AINews/                      # Automated production outputs
│   └── weekly_summary.md        # Compiled weekly AI research logs
│
├── src/
│   └── langgraphagenticai/      # Core package root
│       ├── graph/               # Graph orchestration
│       │   └── graph_builder.py # State machine, edges, and compilation
│       │
│       ├── LLMS/                # Inference management
│       │   └── groqllm.py       # Groq client initializing open-weights models
│       │
│       ├── nodes/               # Graph processing nodes (Specialized Workers)
│       │   ├── ai_news_node.py  # Aggregates, structures, and saves newsletters
│       │   ├── basic_chatbot_node.py # Fallback standard conversational layer
│       │   └── chatbot_with_Tool_node.py # Router agent determining tool dependencies
│       │
│       ├── state/               # Thread execution state
│       │   └── state.py         # Memory schemas and continuous state channels
│       │
│       ├── tools/               # Agent execution utilities
│       │   └── search_tool.py   # Web search grounding interface
│       │
│       └── ui/                  # Web interface layer
│           ├── streamlitui/     # Streamlit templates and layouts
│           ├── uiconfigfile.ini # Hardcoded UI text settings and configuration
│           └── uiconfigfile.py  # Python configuration parser
│
├── app.py                       # Main application runtime file
├── main.py                      # Core engine runner and test scripts
├── requirements.txt             # Project software dependencies
└── .env                         # Local runtime credentials (Git ignored)
```

---

## 🌟 Key Features

* **LangGraph State Management:** Implements cyclic state transitions to allow agents to reflect, refine, and recurse until tasks are solved successfully.
* **On-Demand Tool Execution:** Automatically branches chat operations to execute real-time web lookups via `search_tool.py` when questions require outer-world grounding.
* **Groq Inference Engine:** Configured to leverage ultra-fast response streams using open architectures like `llama-3.1-8b-instant`.
* **Flexible Runtime Execution:** A dynamic control panel sidebar allows the user to swap between distinct graph execution paradigms on the fly:
  * **Basic Chatbot:** Standard conversational agent using memory state variables.
  * **Chatbot With Web:** Augmented search-and-reason graph utilizing conditional edges.
  * **AI News:** Isolated compilation flow tracking technology shifts into physical logs (`weekly_summary.md`).

---

## 🚀 Getting Started

### 1. Prerequisites
Ensure you have **Python 3.10** or greater installed locally.

### 2. Installation
Clone the repository and install all required package dependencies:
```bash
git clone <your-repository-url>
cd AgenticChatBot
pip install -r requirements.txt
```

### 3. Environment Setup
Create a `.env` file in the root directory of your workspace to store secrets:
```env
GROQ_API_KEY=your_groq_api_key_here
TAVILY_API_KEY=your_search_tool_api_key_here
```

### 4. Running the Web UI
Launch the application using Streamlit:
```bash
streamlit run app.py
```
Once initialized, navigate your local browser to:
```text
http://localhost:8501
```

---

## 🧩 Deep-Dive Execution Flow

1. **User Action:** The user picks a use-case mode (e.g., *Chatbot With Web*) and submits a query inside the Streamlit frontend.
2. **Graph Ingestion:** Configuration inputs pass down to `graph_builder.py` to route execution.
3. **Intent Branching:** `chatbot_with_Tool_node.py` inspects the context. If data gaps exist, it signals a conditional transition to the `search_tool.py` script.
4. **Answer Construction:** Collected web context or internal weights compile down inside the Groq node loop and streams output characters directly into your browser display.
