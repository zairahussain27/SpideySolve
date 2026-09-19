# LangChain

LangChain is a framework for building applications powered by Large Language Models (LLMs). It provides reusable abstractions and components for connecting LLMs with prompts, external data, memory, tools, and decision-making workflows.

---

## 1. Abstraction

**Abstraction = pre-built functionality that hides complex implementation details.**

LangChain provides ready-made components so that we don't have to implement everything from scratch.

For example:

* LLM integrations
* Prompt templates
* Document loaders
* Retrievers
* Chains
* Agents
* Memory
* Tools

Instead of manually writing all the integration code, we can use LangChain's existing components.

---

## 2. LLM Support

LangChain supports multiple LLM providers through a common interface.

Examples:

* OpenAI
* Anthropic
* Google Gemini
* Groq
* Ollama
* Hugging Face

Instead of completely rewriting the application when changing providers, we can use the corresponding LangChain integration.

### Basic idea

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-4o-mini",
    api_key="YOUR_API_KEY"
)
```

**Important:** API keys should never be hardcoded or committed to GitHub.

Use environment variables instead:

```env
OPENAI_API_KEY=your_api_key
```

---

# 3. Prompts

A prompt tells the LLM what we want it to do.

LangChain provides **Prompt Templates** for creating reusable and dynamic prompts.

### Example

```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_template(
    "Explain {topic} in simple words."
)

result = prompt.invoke({
    "topic": "RAG"
})
```

Instead of manually constructing a new string every time, we can reuse the template with different values.

### Why use Prompt Templates?

* Reusable prompts
* Dynamic inputs
* Cleaner code
* Easier prompt management
* Better separation between application logic and prompt design

---

# 4. Chains

A **chain** connects multiple steps together to create a workflow.

For example:

```text
User Input
    ↓
Prompt Template
    ↓
LLM
    ↓
Output Parser
    ↓
Final Response
```

A chain can combine operations such as:

* Formatting a prompt
* Calling an LLM
* Processing the output
* Passing the result to another component

### Example

```python
chain = prompt | llm
```

Then:

```python
response = chain.invoke({
    "topic": "LangChain"
})
```

The output of one component becomes the input of the next component.

---

# 5. Indexes

**Indexes help an LLM application work with external data.**

An LLM's knowledge is limited to what it was trained on. If we want the application to answer questions using our own documents or data, we need a way to load, process, store, and retrieve that information.

A typical workflow is:

```text
Documents
    ↓
Document Loader
    ↓
Text Splitting
    ↓
Embeddings
    ↓
Vector Store
    ↓
Retriever
    ↓
Relevant Documents
    ↓
LLM
```

This concept is commonly used when building **RAG (Retrieval-Augmented Generation)** applications.

### Example external data

* PDFs
* Websites
* Databases
* Text files
* Company documentation
* Knowledge bases

---

# 6. Memory

**Memory allows an application to maintain relevant information from previous interactions.**

Without memory:

```text
User: My name is Zaira.
AI: Nice to meet you!

User: What is my name?
AI: I don't know.
```

With conversational history:

```text
User: My name is Zaira.
AI: Nice to meet you, Zaira!

User: What is my name?
AI: Your name is Zaira.
```

Memory can be implemented using conversation history or persistent storage, depending on the application's requirements.

### Important distinction

Memory is not the same as an LLM's training data.

It is application-level information that is supplied to the model when needed.

---

# 7. Agents

An **agent is an LLM-powered system that can decide what action or tool to use next based on the task.**

A normal chain follows a predefined workflow:

```text
Input
 ↓
Step 1
 ↓
Step 2
 ↓
Step 3
 ↓
Output
```

An agent can dynamically decide:

```text
User Request
     ↓
    LLM
     ↓
Decide what to do
     ↓
 ┌───────────────┐
 ↓       ↓       ↓
Search  Tool   Calculator
 └───────────────┘
     ↓
Observe result
     ↓
Decide next action
     ↓
Final Answer
```

For example, if a user asks:

> "What is the weather in Delhi and convert the temperature to Fahrenheit?"

An agent could decide to:

1. Call a weather tool.
2. Get the temperature.
3. Use a calculation tool.
4. Return the result.

### Key idea

**Chain = predefined sequence**

**Agent = dynamic decision-making**

---

# 8. Tools

Tools allow an LLM application to interact with external systems.

Examples:

* Search engines
* Calculators
* APIs
* Databases
* Python
* File systems
* Custom business APIs

An agent can select the appropriate tool based on the user's request.

```text
User
 ↓
Agent
 ↓
Choose Tool
 ↓
Tool Execution
 ↓
Result
 ↓
Agent
 ↓
Final Answer
```

---

# 9. Retrievers

A **retriever finds relevant information from a knowledge source.**

For example:

```text
User Question
      ↓
   Retriever
      ↓
Relevant Documents
      ↓
     LLM
      ↓
   Answer
```

Retrievers are an important part of RAG systems.

---

# 10. Document Loaders

Document loaders bring external data into the application.

Examples:

```text
PDF
Website
CSV
TXT
Database
      ↓
Document Loader
      ↓
Documents
```

After loading, the documents can be split, embedded, indexed, and retrieved.

---

# 11. Output Parsers

LLMs usually return text.

An **output parser** converts the model's response into a more useful format.

For example:

```text
LLM Output
    ↓
Output Parser
    ↓
JSON / List / Structured Object
```

This is useful when the application needs structured data instead of plain text.

---

# 12. LangChain Application Architecture

A typical LangChain application can look like:

```text
                 User
                   ↓
                Prompt
                   ↓
                 LLM
              ↙    ↓    ↘
          Memory  Tools  Retriever
                    ↓       ↓
                 External  Vector
                  APIs     Store
                    ↓       ↓
                    └──→ LLM
                          ↓
                    Output Parser
                          ↓
                    Final Response
```

---

# Quick Revision

| Component             | Main Purpose                                        |
| --------------------- | --------------------------------------------------- |
| **Abstraction**       | Provides reusable building blocks                   |
| **LLM**               | Generates or processes language                     |
| **Prompt Template**   | Creates reusable prompts                            |
| **Chain**             | Connects predefined steps                           |
| **Index / Retrieval** | Makes external data searchable                      |
| **Retriever**         | Finds relevant information                          |
| **Memory**            | Maintains conversational/application context        |
| **Agent**             | Dynamically decides what action to take             |
| **Tool**              | Lets the application interact with external systems |
| **Document Loader**   | Loads external documents/data                       |
| **Output Parser**     | Converts LLM output into structured data            |

---

# The Most Important Difference

### Chain

> **"I know the steps beforehand."**

```text
Prompt → LLM → Parser → Output
```

### Agent

> **"I need to decide what to do based on the situation."**

```text
User → LLM → Decide Action → Tool → Observe → Decide Again → Output
```

### RAG

> **"I need to retrieve relevant external information before generating the answer."**

```text
Question → Retrieve Data → LLM → Answer
```

### Memory

> **"I need to provide relevant information from previous interactions."**

```text
Previous Conversation
        ↓
      Memory
        ↓
     Current LLM Call
```
