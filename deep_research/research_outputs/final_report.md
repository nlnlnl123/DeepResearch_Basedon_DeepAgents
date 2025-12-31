# Context Engineering Approaches for Building AI Agents

## Overview of Context Engineering

Context engineering is the practice of deliberately constructing and maintaining the information environment that an AI agent uses to perform tasks. It ensures that the underlying large language model (LLM) receives all relevant information while filtering out irrelevant data. As Andrej Karpathy describes it, context engineering is "the delicate art and science of filling the context window with just the right information for the next step" [1].

The LLM context window functions similarly to RAM in a computer system—it has limited capacity, and just as an operating system curates what fits into CPU RAM, context engineering plays a similar role in managing what information the LLM accesses at each step. This practice has become increasingly critical, with Cognition calling it "effectively the #1 job of engineers building AI agents" [1].

Context engineering differs from prompt engineering in important ways. While prompt engineering focuses on crafting system prompts or user inputs to elicit specific responses, context engineering is concerned with shaping what the model knows by managing short-term memory, long-term memory, retrieved documents, schema constraints, and outputs from tools [1].

## Four Main Context Engineering Strategies

### Write Context

Writing context involves saving information outside the context window to help an agent perform tasks. This approach addresses the limitation that context windows have finite capacity and that long-running tasks can generate more information than can fit in memory at once.

**Scratchpads** enable persisting information during task execution. For example, Anthropic's multi-agent researcher saves plans to memory to prevent truncation when tasks exceed the 200,000 token limit [1]. This allows agents to maintain state across operations that would otherwise exceed context capacity.

**Memories** store information across multiple sessions and come in three primary types:
- **Episodic memories**: Few-shot examples of desired behavior that demonstrate how the agent should respond in specific situations
- **Procedural memories**: Instructions that steer behavior and guide the agent's approach to tasks
- **Semantic memories**: Facts and knowledge that provide task-relevant context [1]

These memory types work together to create a rich, persistent knowledge base that agents can draw upon without requiring all information to be present in the immediate context window.

### Select Context

Selecting context involves pulling relevant information into the context window to help an agent perform tasks. This strategy is crucial for ensuring that agents have access to the most pertinent information without overwhelming the context window with irrelevant data.

**Memory selection** uses embeddings and knowledge graphs to retrieve relevant memories from storage. By representing memories as vector embeddings, systems can perform semantic similarity searches to find the most relevant historical information for a given situation [1].

**Tool selection** applies retrieval-augmented generation (RAG) to tool descriptions, fetching only the most relevant tools for a given task. Research has shown this approach can improve tool selection accuracy by threefold compared to naive approaches [1]. This is particularly important in multi-agent systems where agents may have access to dozens or hundreds of tools.

**Knowledge retrieval** employs RAG with advanced techniques including AST parsing, semantic chunking, grep/file search, knowledge graph-based retrieval, and re-ranking steps. Tools like Windsurf implement these sophisticated retrieval strategies to ensure code development agents can efficiently find relevant code in large codebases [1].

### Compress Context

Compressing context involves retaining only the tokens required to perform a task, thereby making more efficient use of limited context window capacity.

**Context summarization** uses recursive or hierarchical summarization to distill relevant information. Claude Code's "auto-compact" feature, for example, automatically summarizes the full trajectory of user-agent interactions when context usage reaches 95% of capacity [1]. This preserves the most important information while freeing up space for new context.

**Context trimming** filters or prunes context using various strategies. Hard-coded heuristics might remove older messages or less relevant information, while trained context pruners like Provence use machine learning models to identify and retain the most contextually important tokens [1]. The challenge lies in balancing compression with retention of critical details that may be needed later.

### Isolate Context

Isolating context involves splitting context up to help an agent perform tasks. This strategy addresses issues like context poisoning, distraction, confusion, and clash that can occur when too much or conflicting information occupies the context window.

**Multi-agent systems** separate concerns across sub-agents, each with specific tools, instructions, and context windows. Anthropic's multi-agent researcher and OpenAI Swarm exemplify this approach, where different agents handle different aspects of a task [1]. While this can use up to 15 times more tokens than single-agent approaches, it often improves performance by preventing context overload.

**Sandboxing** runs code in isolated environments, such as HuggingFace's CodeAgent with E2B sandbox. This isolates token-heavy objects in environment state rather than keeping them in the context window [1].

**State objects** use schema-based state objects to isolate context fields for selective exposure. This allows fine-grained control over what information each agent or component can access [1].

## Best Practices and Methodologies

### Three Pillars of Context Engineering

**Operationalized knowledge** involves subject matter experts (SMEs) mapping real-world processes and configuring AI agents with natural language prompts. Schema builders define document formats and validation rules, while prebuilt templates encode domain logic. For example, insurance applications might have templates that encode 80% of domain logic for claims, loans, and underwriting [2].

**Context infrastructure** provides the technical foundation for effective context management. Multi-agent orchestration maintains structured context across tasks, while embeddings, memory, and vector stores preserve and retrieve relevant memories. Audit trails log confidence scores, decisions, and tool calls for traceability and compliance [2].

**Deployment strategy** ensures context isn't lost to third-party tools through VPC or on-prem deployments. Feedback loops evolve logic via user feedback and usage data, while progress trackers connect AI actions to business standard operating procedures (SOPs) [2].

### Collaborative Configuration Approach

Effective context engineering requires embedding with subject matter experts and IT teams to encode real-world logic into AI workflows. This involves labeling edge cases directly within documents and workflows, defining schema structures and domain-specific rules, and tuning confidence thresholds for automatic approvals or human escalation [2].

Regular review of execution logs helps refine tool use, query formats, and decision logic. This collaborative approach ensures that context engineering reflects actual business processes rather than theoretical idealizations.

### Continuous Evolution

Context engineering is not a one-time setup but an ongoing process. Coaching AI agents through human-in-the-loop feedback mechanisms, conducting A/B testing on context configurations, and using progress trackers to align decisions to SOPs all contribute to continuous improvement [2].

Tracking every step in JSON logs enables detailed analysis and optimization. Context engineering continues through monitoring, retraining, and tuning based on real-world performance data.

## Tools, Frameworks, and Platforms

### LangChain/LangGraph

LangGraph serves as a low-level orchestration framework designed to support all four context engineering strategies. It provides thread-scoped short-term memory with checkpointing for scratchpads, long-term memory for persisting context across sessions, and fine-grained state control at each agent step [1].

Built-in utilities for summarization and trimming help manage context window usage, while support for multi-agent architectures enables separation of concerns. LangGraph also supports sandboxing for context isolation [1].

LangSmith, the accompanying observability and evaluation platform, provides agent tracing and token usage tracking. It enables agent evaluation to test context engineering impact, creating a virtuous feedback loop: identify opportunities, implement changes, test results, and repeat [1].

Additional LangChain ecosystem tools include LangGraph Bigtool for semantic search over tool descriptions and LangMem for memory management abstractions [1].

### AgentFlow

AgentFlow, a multimodal platform, implements structured context layers with a Business Configuration Layer and a Technical Configuration Layer. Built-in tools include schema/workflow builders, confidence scoring, execution logs, and drift detection [2].

The Progress Tracker feature connects AI actions to business SOPs, ensuring that agent behavior aligns with organizational processes and requirements [2].

### Other Tools

OpenAI Swarm provides a multi-agent library for separation of concerns, while E2B offers sandbox environments for code execution. Zep serves as a memory platform powered by embeddings and knowledge graphs, and Neo4j Graphiti provides knowledge graph memory for agents [1].

## Real-World Applications and Examples

### Insurance Underwriting

A multimodal case study in insurance underwriting achieved greater than 95% accuracy through context engineering. The implementation involved SME-guided schema creation tailored for each insurer, with template-based logic embedded for three different insurance companies [2].

The system incorporated a computer vision model to link text fields to visual anchors, and iterative SME feedback tuned performance over time. This example demonstrates how context engineering can achieve production-grade accuracy in complex, regulated industries [2].

### Anthropic's Multi-Agent Researcher

Anthropic's multi-agent researcher uses a LeadResearcher that saves plans to memory to persist context beyond the 200,000 token limit. Multiple sub-agents operate in parallel with isolated context windows, each allocated to narrow sub-tasks [1].

While this approach outperformed single-agent alternatives, it uses up to 15 times more tokens than chat-based approaches. This trade-off between performance and resource consumption is a key consideration in context engineering design [1].

### Claude Code

Claude Code uses "auto-compact" summarization at 95% context window usage, summarizing the full trajectory of user-agent interactions to preserve important information while freeing space for new context. The system also uses CLAUDE.md for procedural memory, storing instructions that guide behavior [1].

### ChatGPT Memory

ChatGPT's memory feature auto-generates long-term memories across sessions, storing and selecting from large collections of user-specific memories. The system uses embeddings for memory indexing, enabling efficient retrieval of relevant information [1].

However, this approach faces challenges, including the potential to retrieve unexpected memories that may not be relevant to the current task. For example, the system might inject location information into image generation tasks where it wasn't explicitly requested [1].

### Code Development Tools

Modern code development tools like Cursor and Windsurf use rules files for procedural memories, storing instructions that guide the agent's behavior during coding tasks. Windsurf implements advanced RAG for code retrieval including AST parsing, semantic chunking, grep/file search, knowledge graph-based retrieval, and re-ranking [1].

HuggingFace's Deep Researcher uses CodeAgent that outputs code instead of JSON tool calls, with code running in a sandbox environment. This isolates token-heavy objects in environment state rather than keeping them in the context window [1].

## Current Research and Developments

### Academic Research

Academic research has made significant contributions to context engineering. The Reflexion paper introduced reflection following each agent turn and re-using self-generated memories, while the Generative Agents research created memories synthesized periodically from collections of past agent feedback [1].

Provence represents a trained context pruner for Question-Answering tasks, demonstrating that machine learning can improve context compression beyond simple heuristics. Research on RAG for tool selection has shown a threefold improvement in accuracy compared to baseline approaches [1].

### Industry Developments

Major AI companies including Anthropic, OpenAI, and Cognition are publishing context engineering methodologies, reflecting the growing importance of this practice. Framework providers like LangChain and HuggingFace are building context engineering capabilities into their platforms, making advanced techniques more accessible to developers [1].

Current industry focus emphasizes production-grade systems with traceability, auditability, and compliance requirements. Recent research papers confirm that context compression, context selection, and context formatting are central to enterprise LLM performance [2].

## Challenges and Limitations

### Context-Related Failures

Several types of context-related failures can occur in AI agent systems. Context poisoning happens when a hallucination makes it into the context, potentially corrupting future decisions. Context distraction occurs when the context overwhelms the model's training, while context confusion arises when superfluous context influences responses inappropriately [1].

Context clash happens when different parts of the context disagree with each other, potentially leading to inconsistent or contradictory behavior. These failure modes highlight the importance of careful context management and validation [1].

### Token and Performance Challenges

Long-running tasks and accumulating tool feedback cause large token usage that can exceed context window size. This balloons cost and latency while degrading agent performance. Multi-agent systems, while often more effective, can use up to 15 times more tokens than single agents, creating significant resource implications [1].

Balancing performance against resource consumption remains a key challenge in context engineering design. Different applications may require different trade-offs depending on their specific requirements and constraints.

### Memory Selection Challenges

Ensuring relevant memories are selected from large collections presents ongoing difficulties. Unexpected or undesired memory retrieval can make users feel the context window "no longer belongs to them," eroding trust in the system [1].

Embedding search becomes unreliable as codebase size grows, necessitating combinations of techniques for reliable retrieval. The challenge is to maintain accuracy and relevance while scaling to ever-larger knowledge bases [1].

### Implementation Challenges

Context engineering requires collaboration between SMEs and IT teams, creating organizational coordination challenges. Continuous monitoring and tuning are required, and careful prompt engineering is needed to plan multi-agent work effectively [2].

Coordination complexity in multi-agent systems can be significant, and balancing context compression with retention of critical information requires sophisticated algorithms and ongoing refinement. These implementation challenges mean that effective context engineering requires both technical expertise and domain knowledge [1].

### Summarization Challenges

Summarization faces the inherent difficulty of capturing specific events or decisions in compressed form. Important details may be lost during compression, potentially affecting agent performance on future tasks that depend on those details [1].

Effective summarization may require fine-tuned models specifically trained for context compression, adding complexity to the implementation. The risk of losing important information during compression must be carefully managed [1].

## Conclusion

Context engineering has emerged as a critical discipline for building effective AI agents. The four main strategies—write, select, compress, and isolate context—provide a comprehensive toolkit for managing the limited context window while ensuring agents have access to relevant information.

Successful context engineering requires a combination of technical infrastructure, domain expertise, and continuous refinement. Tools like LangGraph, AgentFlow, and various memory platforms provide essential capabilities, but effective implementation demands careful attention to specific application requirements and constraints.

As AI agents become more sophisticated and deployed in increasingly complex environments, context engineering will likely continue to evolve. Current research directions include improved context compression algorithms, more sophisticated memory selection mechanisms, and better approaches to multi-agent coordination. Organizations that invest in robust context engineering practices will be well-positioned to leverage AI agents effectively in production environments.

### Sources

[1] LangChain Blog - Context Engineering: https://blog.langchain.com/context-engineering-for-agents/
[2] Multimodal - Context Engineering: The Secret to High-Performing Agentic AI: https://www.multimodal.dev/post/context-engineering