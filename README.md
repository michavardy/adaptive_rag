# Research Proposal

## Background
Large language models (LLMs) are AI systems that can understand and generate human-like text. They are trained on vast amounts of text from the internet, which allows them to answer questions and complete tasks in many domains. However, they have limitations: they can forget earlier information in long conversations, give outdated or incorrect answers, and struggle with very specific or proprietary knowledge. Researchers are exploring ways to help LLMs use external information effectively so they can work on tasks that require accurate and up-to-date knowledge.

## Terms

**LLM** : A computer program trained to understand and generate human-like text by learning patterns from large amounts of language data.

**Agent** : An AI system or program that can perform tasks, make decisions, or interact with humans using language models.

**Context Window**: The limited amount of text an LLM can "remember" at one time when answering questions or generating text.

**Shortcut Learning**: The model may choose the most common or most repeated answer it has seen during training, instead of the most correct or most up-to-date answer.

**training data contaimination**: Because LLMs are trained on large amounts of internet text, they learn whatever is common online. If wrong or outdated information is repeated many times, the model may treat it as correct.

**Knowlage Staleness**: The model’s knowledge may be outdated. If something has changed over time (like a company policy, software version, or public figure’s role), the model might still answer based on older information.

**anchoring**: The way a question is worded can push the model toward a common but incorrect idea. The model becomes “stuck” on that suggestion and builds its answer around it.

## Motivation for Rag
LLM agents don’t have an infinite memory and can get confused if too much information is given at once. They contain a lot of general information but often lack the very specific or proprietary knowledge needed for certain tasks, like a code base, legal documents, or health policies. Additionally, LLMs can make mistakes because of shortcut learning, outdated knowledge, or being influenced by common phrases online. Retrieval-Augmented Generation (RAG) aims to fix these problems by allowing the LLM to access external, up-to-date information on demand.

## Rag Introduction
Retrieval-Augmented Generation (RAG) is a method that allows a language model to use external information when answering a question. Instead of relying only on what it learned during training, the model first retrieves relevant documents and then generates an answer based on that information.

For example, imagine a student studying for a history exam. A regular LLM might answer using general knowledge about a topic. A RAG system, however, would first look at the student’s actual textbook or class notes and then generate an answer based specifically on those materials.

This helps reduce mistakes from outdated or overly general information and makes responses more accurate and aligned with the exact material being studied.

## Rag Problems

1. **Chunking** - RAG systems usually split documents into smaller pieces (“chunks”) before storing them.  the problem is how to recognize where ideas naturally begin and end so that the chunks or set of chunks will contain complete and useful information without any extra.

2. **Extraction** -  how to select which chunk or set of chunks are relevant to answer the question without introducing extra information.

3. **System Prompt** - given a users request, we might retrieve the correct information but the LLM still answers not according to how the user intended.  the System Prompt captures the intended use, style and form of how the user expects to see the answer.

## Naive Solutions

1. **Chunking** –
Split documents into equal-sized pieces, such as every few paragraphs, and slightly overlap them so nothing is completely cut off. This makes sure all content is stored, but it does not truly understand where a full idea starts or ends. Important information may still be split awkwardly.

2. **Extraction** –
When a question is asked, select the pieces of text that seem most similar to the question and send the top few matches to the model. If we are unsure, we can send more pieces to avoid missing something important. However, this can also include extra or unrelated information, which may confuse the model.

3. **System Prompt** –
Write one fixed instruction that tells the model to use only the retrieved documents, follow a certain writing style, and avoid adding outside knowledge. This creates consistent behavior, but it cannot adjust to different types of questions, users, or goals.

## Static Solutions
there are many proposed solutions to these problems, however most of the solutions I have seen are static meaning that they can't change over time, or they can't be learned by the LLM.

## Proposal: Adaptive RAG with Structured User Feedback

1. **Objective**
The goal of this proposal is to design a RAG system that improves over time through structured user feedback. Instead of relying on fixed rules for chunking, extraction, and prompting, the system would refine these components based on how users evaluate its answers.

The system is intended to adapt across different domains such as coding, legal, educational, medical, and agent-based workflows. Each domain has different expectations for accuracy, structure, tone, and level of detail. A static RAG pipeline typically must be manually tailored to each domain, which can be costly and difficult to maintain.

2. **Core Idea**
The central idea is to treat each answer as a draft that can be reviewed and improved. After the system generates a response, the user provides explicit feedback — similar to reviewing a Merge Request in software development.

This feedback is not only about whether the answer is correct, but also whether:

The right information was retrieved

The answer included too much or too little detail

The format matched expectations

The tone or structure was appropriate

The system then uses this feedback to adjust its internal behavior.

3. **How the Feedback Mechanism Works**
The feedback mechanism operates at three levels:

a) *Chunking Adjustment*
If answers consistently miss context or include unnecessary information, the system can adjust how documents are split.
For example:

In legal domains, it may learn to keep larger sections together to preserve full clauses.

In coding domains, it may learn to isolate functions or classes as separate chunks.

b) *Retrieval Adjustment*
If users often say the answer is incomplete or overloaded with irrelevant details, the system can refine how many chunks it retrieves and which types it prioritizes.
Over time, it learns what “relevant” means within each domain.

c) *System Prompt Adjustment*
If users frequently request changes in style (e.g., “be more concise,” “explain step-by-step,” “cite sources”), the system can update its internal instructions to match those expectations automatically in future interactions.

4. **Domain Adaptation**
Different domains require different response patterns:

Coding: precise, structured, minimal ambiguity

Legal: careful wording, complete context

Education: explanations, examples, progressive clarity

Medical: cautious, clearly sourced, structured

Agent workflows: action-oriented, structured outputs

Instead of manually designing a separate RAG pipeline for each domain, this system would gradually learn these expectations from user feedback.

5. **Expected Outcome**
Over time, the RAG system becomes domain-aware and aligned with user expectations. It learns how to:

Organize information more effectively

Retrieve the right level of detail

Present answers in the expected structure and tone

The result is a RAG system that improves through interaction. Rather than remaining static and rule-based, it evolves to better serve specific domains and individual users while reducing the need for manual customization.
