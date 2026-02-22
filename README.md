# LangChain Tutorial - Weather Agent

A hands-on tutorial notebook that demonstrates building a conversational LangChain agent with custom tools, structured output, and persistent memory. This project creates a **weather forecasting agent** that responds with clever puns and maintains conversation context across multiple interactions.

---

## Table of Contents

- [Architecture & Components](#architecture--components)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installing](#installing)
- [Running the Notebook](#running-the-notebook)
- [Project Structure](#project-structure)

---

## Architecture & Components

The tutorial builds a complete conversational agent with the following architecture:

### Agent Overview
A LangChain agent powered by GPT-4o-mini that:
- **Responds with puns** — enforces a punny weather forecast personality through a system prompt
- **Uses custom tools** — `get_weather_for_location` and `get_user_location` decorated with `@tool`
- **Maintains conversation history** — via LangGraph's `InMemorySaver` checkpointer, keyed by `thread_id`
- **Delivers structured output** — a `ResponseFormat` dataclass with punny responses and weather conditions
- **Handles user context** — a `Context` dataclass carrying `user_id` for personalized tool behavior

```
User Message
    └─► Agent (gpt-4o-mini)
            ├─► get_user_location(context.user_id)
            ├─► get_weather_for_location(city)
            └─► ResponseFormat { punny_response, weather_conditions }
```

### Key Components

| Component | Purpose | Details |
|---|---|---|
| **Model** | Core LLM | GPT-4o-mini with temperature=0.5, max_tokens=1000 |
| **Tools** | Actions the agent can take | `get_weather_for_location(city)`, `get_user_location()` |
| **Context** | Per-user state | Dataclass with `user_id` to personalize responses |
| **Response Format** | Structured output | `punny_response` (str) + `weather_conditions` (optional str) |
| **Memory** | Conversation persistence | `InMemorySaver` checkpointer for multi-turn conversations |
| **System Prompt** | Agent personality | Instructs agent to be a punny weather forecaster |

### Conversation Flow

```
First Message: "What's the weather outside?"
    └─► Agent retrieves user location (Florida for user_id="1")
    └─► Agent gets weather for Florida
    └─► Punny response with weather details

Second Message: "Thank you!"
    └─► Agent remembers previous conversation context
    └─► Responds appropriately to gratitude
```

---

## Getting Started

### Prerequisites

Make sure you have the following installed:

- **Python 3.8+**
- **pip** or a compatible package manager
- A virtual environment tool (e.g., `venv`)
- API key for:
  - [OpenAI](https://platform.openai.com/account/api-keys) — required for GPT-4o-mini

```bash
python --version 
pip --version
```

### Installing

**Step 1 — Clone the repository**

```bash
git clone https://github.com/your-username/langchain-weather-agent.git
cd langchain-weather-agent
```

**Step 2 — Create and activate a virtual environment**

```bash
python -m venv .venv

# On Windows:
.venv\Scripts\activate

# On macOS/Linux:
source .venv/bin/activate
```

**Step 3 — Install dependencies**

```bash
pip install langchain langchain-openai langgraph python-dotenv
```

Or from within the notebook, run the first cell directly:

```python
%pip install langchain langchain-openai langgraph python-dotenv
```

**Step 4 — Configure environment variables**

Create a `.env` file at the root of the project:

```env
OPENAI_API_KEY=sk-...
```

The notebook loads these automatically with `python-dotenv`:

```python
from dotenv import load_dotenv
import os

load_dotenv()
```

---

## Running the Notebook

**Step 5 — Launch Jupyter**

```bash
jupyter notebook langchain.ipynb
# or
jupyter lab langchain.ipynb
```

**Step 6 — Run cells in order**

Execute cells from top to bottom. The notebook is divided into clearly marked sections:

1. **Setup** — installs packages and loads environment variables
2. **System Prompt** — defines the agent's personality and tool usage instructions
3. **Create Tools** — implements custom weather and location tools with context support
4. **Configure Model** — initializes GPT-4o-mini with specific parameters
5. **Response Format** — defines structured output schema
6. **Add Memory** — sets up conversation persistence
7. **Create and Run Agent** — assembles all components and executes test conversations

---

## Project Structure

```
LangchainTutorial/
├── langchain.ipynb            # Main tutorial notebook
├── .env                       # API keys (not committed to version control)
├── .venv/                     # Virtual environment (not committed)
└── README.md                  # This file
```

### Notebook Structure

The notebook is organized into the following sections:

1. **Setup** — Package installation and imports
2. **System Prompt** — Agent personality configuration
3. **Create Tools** — Custom tool definitions with context support
4. **Configure Model** — GPT-4o-mini initialization
5. **Response Format** — Structured output schema
6. **Add Memory** — Conversation persistence setup
7. **Create and Run Agent** — Agent assembly and execution examples
8. **First Execution** — Initial weather query demonstration
9. **Second Execution** — Conversation continuity demonstration

---
### authors
juan felipe rangel rodriguez
