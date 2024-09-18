# Agentic RAG using LangChain and LangGraph

## Project Overview

This project demonstrates the implementation of an Agentic Retrieval-Augmented Generation (RAG) system using LangChain and LangGraph. The system combines a local knowledge base with the ability to use external tools, creating a powerful AI agent capable of answering complex queries.

## Key Components

1. LangChain: A framework for developing applications powered by language models.
2. LCEL (LangChain Expression Language): A declarative way to compose chains and agents.
3. LangGraph: A library for building stateful, multi-actor applications with LLMs.
4. OpenAI's GPT models: Used for natural language processing and generation.

## Detailed Implementation Steps

### 1. Setting Up the Environment

- Install required libraries: langchain, langchain_openai, langchain-community, langgraph, arxiv, duckduckgo-search, faiss-cpu, pymupdf.
- Set up environment variables for OpenAI API key and LangSmith tracing.

### 2. Implementing Basic RAG

#### a. Retrieval System
- Use ArxivLoader to fetch documents about Retrieval Augmented Generation.
- Implement text splitting using RecursiveCharacterTextSplitter.
- Create a FAISS vector store with the split documents.
- Set up a retriever using the FAISS vector store.

#### b. Prompt Template
- Create a RAG prompt template for question-answering.

#### c. Language Model
- Initialize OpenAI's ChatOpenAI model (gpt-3.5-turbo).

#### d. RAG Chain
- Construct an LCEL chain that combines retrieval, prompt formatting, and model invocation.

### 3. Building the LangGraph Agent

#### a. Tool Setup
- Implement DuckDuckGo search and Arxiv query tools.
- Create a ToolExecutor for managing tool usage.

#### b. Agent State
- Define an AgentState class to manage the conversation state.

#### c. Graph Nodes
- Implement nodes for calling the model, executing tools, and running the RAG chain.

#### d. Graph Construction
- Create a StateGraph for the agent.
- Add nodes for the agent, action execution, and initial RAG processing.
- Set the entry point to the RAG chain.

#### e. Conditional Edges
- Implement logic to determine if the RAG response fully answers the query.
- Add conditional edges to either end the conversation or continue with tool usage.

#### f. Compilation
- Compile the graph into an executable application.

### 4. Advanced Features

#### a. Dynamic Tool Selection
- Implement a system for the agent to choose appropriate tools based on the query.

#### b. Iterative Refinement
- Allow the agent to refine its answers through multiple cycles if necessary.

#### c. Context Management
- Maintain conversation context across multiple turns.

## Key Innovations

1. Combining RAG with dynamic tool usage.
2. Implementing a stateful, cyclic conversation flow using LangGraph.
3. Using conditional logic to determine when to use external tools vs. local knowledge.

## Detailed Explanation of the Agentic RAG System

### RAG Implementation
The RAG system is implemented using LangChain's LCEL (LangChain Expression Language). It consists of:
1. A retriever based on FAISS vector store, populated with Arxiv papers on RAG.
2. A prompt template for question-answering.
3. An OpenAI language model (gpt-3.5-turbo).
4. An LCEL chain that combines these components.

### LangGraph Integration
LangGraph is used to create a stateful, cyclic graph that represents the agent's decision-making process:

1. State Management:
   - An `AgentState` class is defined to manage the conversation state.
   - The state object contains a sequence of messages that are updated as the conversation progresses.

2. Graph Nodes:
   - `first_action`: The entry point, which runs the RAG chain.
   - `agent`: Calls the language model for decision-making.
   - `action`: Executes tools when needed.

3. Conditional Edges:
   - After the RAG chain, a conditional edge determines if the answer is sufficient or if tools are needed.
   - After the agent node, another conditional edge decides whether to use a tool or end the conversation.

4. Cyclic Behavior:
   - The graph allows for cycles, enabling the agent to use tools and refine answers iteratively.

### Decision-Making Process
1. The query is first processed by the RAG system.
2. A GPT model determines if the RAG answer is sufficient.
3. If not sufficient, the agent node decides which tool to use.
4. The tool is executed, and the result is fed back to the agent.
5. This cycle continues until a satisfactory answer is generated.

### Tool Integration
- DuckDuckGo search and Arxiv query tools are integrated.
- A `ToolExecutor` manages the execution of these tools.

### Advantages of This Approach
1. Flexibility: The system can leverage both local knowledge (RAG) and external tools.
2. Iterative Refinement: The cyclic nature allows for multiple rounds of information gathering and answer refinement.
3. Stateful Conversations: The `AgentState` enables context retention across turns.
4. Conditional Logic: The system can make dynamic decisions based on the current state of the conversation.

## Usage

The final agent can be invoked with a question, and it will:
1. Attempt to answer using the local RAG system.
2. If the answer is incomplete, it will use external tools to gather more information.
3. Refine the answer based on all available information.

## Detailed Workflow

1. Initial RAG Processing:
   - The user's query is first processed by the RAG system (`first_action` node).
   - This leverages the local knowledge base (Arxiv papers on RAG) to generate an initial response.

2. Answer Sufficiency Check:
   - A conditional edge (`is_fully_answered` function) determines if the RAG response is sufficient.
   - This uses GPT-4 to evaluate the completeness of the answer.

3. Tool Usage Decision:
   - If the answer is insufficient, the flow continues to the `agent` node.
   - The agent (GPT-3.5-turbo) decides whether to use a tool or end the conversation.

4. Tool Execution:
   - If a tool is needed, the `action` node executes the selected tool (DuckDuckGo search or Arxiv query).
   - The tool's output is added to the conversation state.

5. Answer Refinement:
   - The flow returns to the `agent` node to refine the answer based on the new information.
   - This cycle can repeat multiple times if necessary.

6. Conversation End:
   - The conversation ends when either the RAG response is sufficient or the agent determines no more tools are needed.

## Implementation Details

1. State Management:
   - The `AgentState` class uses a `TypedDict` to store the conversation history.
   - Messages are appended to the state as the conversation progresses.

2. RAG Chain:
   - Implemented using LCEL, combining retrieval, prompt formatting, and model invocation.
   - Converted to a LangGraph node using pre/post-processing functions.

3. Conditional Logic:
   - `is_fully_answered` function uses GPT-4 to evaluate answer completeness.
   - `should_continue` function checks for the presence of a function call to determine if tool usage is needed.

4. Tool Integration:
   - Tools are wrapped in a `ToolExecutor` for easy management and execution.
   - The `call_tool` function handles tool invocation and result processing.

5. Graph Construction:
   - Nodes and edges are added to the `StateGraph` to define the conversation flow.
   - Conditional edges allow for dynamic decision-making based on the current state.

## Conclusion

This project demonstrates a sophisticated approach to building AI agents that can leverage both local knowledge and external tools. It showcases the power of combining RAG with dynamic tool usage, all orchestrated through LangChain and LangGraph's flexible frameworks. The resulting system is capable of handling complex queries by iteratively refining its answers and seeking additional information when needed.

The Agentic RAG system represents a significant advancement in AI-powered question-answering systems, offering a more flexible and context-aware approach to information retrieval and generation. By combining the strengths of retrieval-based and generation-based methods, along with the ability to use external tools, this system can provide more accurate and comprehensive responses to a wide range of queries.