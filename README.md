# Agent Development Kit (ADK)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://www.python.org/)
[![Google Cloud](https://img.shields.io/badge/Google%20Cloud-Vertex%20AI-blue)](https://cloud.google.com/vertex-ai)
[![Gemini](https://img.shields.io/badge/Gemini-2.0-green)](https://ai.google.dev/gemini-api/docs/models)

A flexible and modular framework for developing and deploying AI agents.

## Overview

Agent Development Kit (ADK) is designed to make agent development feel more like software development, making it easier for developers to create, deploy, and orchestrate agentic architectures that range from simple tasks to complex workflows.

Key features:
- **Model-agnostic**: While optimized for Gemini and Google ecosystem, ADK works with various LLM providers
- **Deployment-agnostic**: Deploy agents locally or to cloud environments
- **Compatible with other frameworks**: Built for interoperability with existing agent frameworks

## Getting Started

### Prerequisites

- Python environment (see setup in [crash course repo](https://github.com/bhancockio/agentdevelopment-kit-crash-course))
- API keys for language models (Gemini, OpenAI, etc.)

### Basic Structure

```
parent_folder/
├── agent_folder/         # Agent package directory
│   ├── __init__.py       # Must import agent.py
│   ├── agent.py          # Must define root_agent
│   └── .env              # Environment variables
```

### Root Agent Setup

Every ADK project requires at least one root agent with these properties:

```python
root_agent = Agent(
    name="RootAgent",              # Must match folder name
    model="gemini-2.0-flash",      # Model selection
    description="Root Agent",      # Helpful for multi-agent setups
    instructions="Task description",
    tools=[...]                    # Optional capabilities
)
```

### Running Your Agent

ADK provides several commands:

```bash
adk api_server  # Starts a FastAPI server for agents
adk create      # Creates a new app with prepopulated structure
adk deploy      # Deploys agents to hosted environments
adk eval        # Evaluates agent given eval sets
adk run         # Runs interactive CLI for a specific agent
adk web         # Starts FastAPI server with Web UI
```

## Tools

ADK supports several types of tools to extend agent capabilities:

1. **Function Tools**: Custom tools for specific application needs
   - Functions/Methods: Standard synchronous Python functions
   - Agents-as-Tools: Using specialized agents as tools
   - Long Running Function Tools: For asynchronous operations

2. **Built-in Tools**: Ready-to-use tools for common tasks
   - Google Search, Code Execution, RAG
   - Note: Only one built-in tool is supported per root agent

3. **Third-party Tools**: Integration with external libraries
   - LangChain tools, CrewAI tools, etc.

### Tool Example

```python
def get_current_time(format: str) -> dict:
    """Get the current time in the specified format"""
    return {
        "current_time": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }
```

## State Management

ADK provides robust state management through sessions:

```python
session_service = InMemorySessionService()
session = session_service.create_session(
    app_name="my_app",
    user_id="example_user",
    state={"initial_key": "initial_value"}
)
```

Session types:
- `InMemorySessionService`: Volatile, lost when application closes
- `DatabaseSessionService`: Persistent across application restarts
- `VertexAISessionService`: Stored in Google Cloud

## Multi-Agent Architecture

ADK supports sophisticated multi-agent patterns:

1. **Sequential Agents**: Execute in fixed order, passing data between agents
2. **Parallel Agents**: Execute concurrently for independent tasks
3. **Loop Agents**: Repeatedly execute agents until exit conditions are met

## Cloud Deployment

Deploy to Google Cloud using Vertex AI Agent Engine:

```python
# Wrap agent in AdkApp
app = reasoning_engines.AdkApp(
    agent=root_agent,
    enable_tracing=True
)

# Deploy to Agent Engine
remote_app = agent_engines.create(
    agent_engine=app,
    requirements=["google-cloud-aiplatform[ask,agent_engines]"],
    extra_packages=["./your_agent_package"]
)
```

## Using Other Language Models

ADK supports other language models via:

- **LiteLLM**: Free Python library to handle various models (OpenAI, Claude, Llama, etc.)
- **OpenRouter**: Tool for purchasing tokens usable with any model

## Resources

- [ADK Documentation](https://google.github.io/adk-docs/)
- [Crash Course Tutorial](https://github.com/bhancockio/agentdevelopment-kit-crash-course)
- [Gemini API Documentation](https://ai.google.dev/gemini-api/docs/models)
- [Vertex AI Documentation](https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/overview)

---

This README provides an overview of the Agent Development Kit. For detailed implementation examples and tutorials, please refer to the linked resources.
