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

## Mptivation for Rag
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

## Proposal: Adaptive Rag with Structured user feedback

1. Objective

The goal of this proposal is to design a RAG system that improves over time through structured user feedback. Instead of relying on fixed rules for chunking, extraction, and prompting, the system would refine these components based on how users evaluate its answers.

The system is intended to adapt across different domains such as coding, legal, educational, medical, and agent-based workflows. Each domain has different expectations for accuracy, structure, tone, and level of detail. A static RAG pipeline typically must be manually tailored to each domain, which can be costly and difficult to maintain.

2. Core Idea

The central idea is to treat each LLM answer as a draft that can be reviewed and improved. After the system generates a response, the user provides explicit feedback — similar to a student (LLM) submitting an assignment, the teacher (User) provides explicate feedback.

This feedback is a prestructured template that guides the user to not only feedback about whether the answer is correct, but also whether:

The right information was retrieved

The answer included too much or too little detail

The format matched expectations

The tone or structure was appropriate

The system then uses this feedback to adjust its internal behavior.

3. How the Feedback Mechanism Works

The feedback mechanism operates at three levels:

a) Chunking Adjustment

If answers consistently miss context or include unnecessary information, the system can adjust how documents are split.
For example:

In legal domains, it may learn to keep larger sections together to preserve full clauses.

In coding domains, it may learn to isolate functions or classes as separate chunks.

b) Retrieval Adjustment

If users often say the answer is incomplete or overloaded with irrelevant details, the system can refine how many chunks it retrieves and which types it prioritizes.
Over time, it learns what “relevant” means within each domain.

c) System Prompt Adjustment

If users frequently request changes in style (e.g., “be more concise,” “explain step-by-step,” “cite sources”), the system can update its internal instructions to match those expectations automatically in future interactions.

4. Domain Adaptation

Different domains require different response patterns:

Coding: precise, structured, minimal ambiguity

Legal: careful wording, complete context

Education: explanations, examples, progressive clarity

Medical: cautious, clearly sourced, structured

Agent workflows: action-oriented, structured outputs

Instead of manually designing a separate RAG pipeline for each domain, this system would gradually learn these expectations from user feedback.


You said:
whats the LLM problem where they get outdated or bad information because the prompt references a common thing found in the internet

ChatGPT said:
The phenomenon you’re describing can fall under a few related failure modes in LLM research. The most relevant terms are:

1. Spurious Correlation / Shortcut Learning
In machine learning, models often rely on surface-level statistical patterns rather than underlying causal structure.

If a prompt references something common on the internet (e.g., a popular misconception, outdated meme, or oversimplified explanation), the model may retrieve the most statistically dominant association, not the most correct or current one.

This is often described as:

Shortcut learning

Spurious correlation reliance

Pattern overgeneralization

2. Training Data Contamination / Distribution Bias
LLMs are trained on large-scale web corpora. If:

A misconception is widely repeated online, or

Outdated information is overrepresented,

then the model’s internal distribution will reflect that bias.

This is sometimes framed as:

Web-scale distribution bias

Corpus-induced bias

Popularity bias

3. Knowledge Staleness
If the issue is specifically that the information is outdated (e.g., referencing old APIs, deprecated frameworks, previous political leaders), that’s usually called:

Knowledge cutoff problem

Model staleness

Temporal misalignment

This is especially relevant when prompts mention entities that have changed meaning over time.

4. Anchoring or Context-Induced Retrieval Bias
When the prompt wording itself nudges the model toward common-but-wrong associations, this is often described as:

Anchoring bias in LLMs

Contextual retrieval bias

Prompt-induced hallucination

The model conditions heavily on the prompt tokens, so referencing a common phrase can activate the “wrong attractor basin” in representation space.

5. Semantic Priming Effects
From a cognitive science analogy:
If a prompt contains a highly frequent internet phrase, the model may get “primed” toward the dominant web interpretation.

This is similar to:

Semantic priming

Frequency-based activation bias

If I had to name what you likely mean:
If the problem is:

The prompt references something commonly found online, and the model gives the most common internet answer rather than the most accurate or current one.

The cleanest technical label is:

Popularity bias combined with distributional overfitting to web corpora

Or more informally in LLM research:

Web-distribution anchoring

If you want, describe a concrete example of the failure mode and I can classify it more precisely — especially if you're thinking about this in the context of RAG or chunking research.


You said:
# Research Proposal

## Background

## Terms
**LLM** : 
**Agent** :
**Context Window**: 
**Shortcut Learning**: model may retrieve the most statistically dominant association, not the most correct or current one.
**training data contaimination**: LLMs are trained on large-scale web corpora. If:
A misconception is widely repeated online, or
Outdated information is overrepresented,
then the model’s internal distribution will reflect that bias.

**Knowlage Staleness**: prompts mention entities that have changed meaning over time.

**anchoring**: When the prompt wording itself nudges the model toward common-but-wrong associations

**Priming**: prompt contains a highly frequent internet phrase, the model may get “primed” toward the dominant web interpretation.


## Mptivation for Rag
LLM Agents don't have an infinate context window.  They also get confused with very large context windows.  LLM's are general purpose and so contain vast amounts of unnecissary information while often don't contain propriatary or specific data needed to solve domain specific problems like a code base, specific legal documents, health policies, user database ect.  LLM's suffer from technical problems such as shortcut learning, training data contamination, knowlage staleness, anchoring, priming.

## Rag Introduction

## Rag Problems
1. dynamic semantic chunking
2. dynamic extraction: learning how to extract correctly
3. dynamic system prompt: learning how to system prompt or prompt rewrite

## Current Solution

please fill in background (2-5 sentances)
terms: LLM, Agent, context window

rag introduction (3-10 sentences)
current solutions (2-5 sentences)

please keep explinations very simple and as non technical as possible without over simplifying and not explaining correctly

ChatGPT said:
Here’s a filled-in version of your research proposal draft with clear, simple, and accurate explanations:

Background
Large language models (LLMs) are powerful AI systems that can understand and generate human-like text. They are trained on vast amounts of text from the internet, which allows them to answer questions and complete tasks in many domains. However, they have limitations: they can forget earlier information in long conversations, give outdated or incorrect answers, and struggle with very specific or proprietary knowledge. Researchers are exploring ways to help LLMs use external information effectively so they can work on tasks that require accurate and up-to-date knowledge.

Terms
LLM: A computer program trained to understand and generate human-like text by learning patterns from large amounts of language data.
Agent: An AI system or program that can perform tasks, make decisions, or interact with humans using language models.
Context Window: The limited amount of text an LLM can "remember" at one time when answering questions or generating text.

Motivation for RAG
LLM agents don’t have an infinite memory and can get confused if too much information is given at once. They contain a lot of general information but often lack the very specific or proprietary knowledge needed for certain tasks, like a code base, legal documents, or health policies. Additionally, LLMs can make mistakes because of shortcut learning, outdated knowledge, or being influenced by common phrases online. Retrieval-Augmented Generation (RAG) aims to fix these problems by allowing the LLM to access external, up-to-date information on demand.

RAG Introduction
Retrieval-Augmented Generation (RAG) is a method that helps language models find and use the right information from external sources while generating answers. Instead of relying only on what the model learned during training, it can "look up" documents, databases, or other knowledge in real time. RAG works by first retrieving relevant pieces of information and then letting the LLM generate answers using those pieces as context. This reduces errors from outdated knowledge or common internet biases and helps the model answer very specific questions more accurately. RAG can also make smaller context windows more effective because the model only sees information that is relevant to the current task.

Current Solutions
Current solutions often involve storing knowledge in large databases and retrieving the most relevant information before asking the LLM to generate an answer. Some systems also rewrite prompts or adjust how information is given to the model to reduce mistakes from biases or outdated knowledge. While these methods improve accuracy, there are still challenges with organizing information correctly, deciding which pieces are important, and helping the model use the information effectively.

If you want, I can also rewrite the “RAG Problems” section in the same style so it’s clear and non-technical while still accurate. Do you want me to do that next?


You said:
**Shortcut Learning**: model may retrieve the most statistically dominant association, not the most correct or current one.
**training data contaimination**: LLMs are trained on large-scale web corpora. If:
A misconception is widely repeated online, or
Outdated information is overrepresented,
then the model’s internal distribution will reflect that bias.

**Knowlage Staleness**: prompts mention entities that have changed meaning over time.

**anchoring**: When the prompt wording itself nudges the model toward common-but-wrong associations

**Priming**: prompt contains a highly frequent internet phrase, the model may get “primed” toward the dominant web interpretation.

help me rewrite these definitions, 
please keep explinations very simple and as non technical as possible without over simplifying and not explaining correctly

ChatGPT said:
Here are clearer and simpler versions of your definitions, keeping them accurate but easy to understand:

Shortcut Learning:
The model may choose the most common or most repeated answer it has seen during training, instead of the most correct or most up-to-date answer.

Training Data Contamination:
Because LLMs are trained on large amounts of internet text, they learn whatever is common online. If wrong or outdated information is repeated many times, the model may treat it as correct.

Knowledge Staleness:
The model’s knowledge may be outdated. If something has changed over time (like a company policy, software version, or public figure’s role), the model might still answer based on older information.

Anchoring:
The way a question is worded can push the model toward a common but incorrect idea. The model becomes “stuck” on that suggestion and builds its answer around it.

Priming:
If a prompt includes a very common phrase or idea from the internet, the model may automatically lean toward the most popular interpretation of that phrase, even if it is not the best one for the current situation.

If you'd like, I can also help you group these into categories (for example: data-level issues vs. prompt-level issues) to make your paper structure cleaner.


You said:
## Rag Introduction
Retrieval-Augmented Generation (RAG) is a method that helps language models find and use the right information from external sources while generating answers. Instead of relying only on what the model learned during training, it can "look up" documents, databases, or other knowledge in real time. RAG works by first retrieving relevant pieces of information and then letting the LLM generate answers using those pieces as context. This reduces errors from outdated knowledge or common internet biases and helps the model answer very specific questions more accurately. RAG can also make smaller context windows more effective because the model only sees information that is relevant to the current task.

please rewrite this, 
keep it short, clear, and not so wordy, 
please provide an example and keep it understandable to non technical readers without over simplifying

ChatGPT said:
RAG Introduction
Retrieval-Augmented Generation (RAG) is a method that allows a language model to use external information when answering a question. Instead of relying only on what it learned during training, the model first retrieves relevant documents and then generates an answer based on that information.

For example, if you ask questions about your company’s internal codebase, a normal LLM might guess based on general programming knowledge. A RAG system would first search your actual code or documentation, then answer using those specific files.

This helps reduce mistakes caused by outdated knowledge or common internet assumptions and makes answers more accurate and task-specific.


You said:
please regenerate with an example from education

ChatGPT said:
RAG Introduction
Retrieval-Augmented Generation (RAG) is a method that allows a language model to use external information when answering a question. Instead of relying only on what it learned during training, the model first retrieves relevant documents and then generates an answer based on that information.

For example, imagine a student studying for a history exam. A regular LLM might answer using general knowledge about a topic. A RAG system, however, would first look at the student’s actual textbook or class notes and then generate an answer based specifically on those materials.

This helps reduce mistakes from outdated or overly general information and makes responses more accurate and aligned with the exact material being studied.


You said:
## Rag Problems
1. dynamic semantic chunking
2. dynamic extraction: learning how to extract correctly
3. dynamic system prompt: learning how to system prompt or prompt rewrite

please explain these problems more clearly and simply

ChatGPT said:
RAG Problems
1. Dynamic Semantic Chunking
RAG systems usually split documents into smaller pieces (“chunks”) before storing them. The problem is that fixed-size chunks often break ideas in the wrong place. A definition might be separated from its explanation, or an example might be split from the rule it belongs to.

Dynamic semantic chunking means adjusting how documents are split based on meaning, not just length. The challenge is teaching the system to recognize where ideas naturally begin and end so that each chunk contains complete and useful information.

2. Dynamic Extraction (Learning What to Retrieve Correctly)
When a user asks a question, the system must choose which pieces of information to retrieve. If it retrieves too little, the answer may be incomplete. If it retrieves too much or irrelevant information, the model may become confused.

The problem is deciding what is truly relevant to the question. “Dynamic extraction” means improving how the system selects the right information depending on the specific task and context.

3. Dynamic System Prompt (Learning How to Prompt Correctly)
Even with the right documents, the way instructions are given to the model matters. A poorly written system prompt may cause the model to ignore retrieved information or rely on its own general knowledge instead.

Dynamic system prompting means adapting the instructions given to the model depending on the task, the type of documents retrieved, and the user’s goal. The challenge is learning how to automatically adjust these instructions so the model consistently uses the retrieved information correctly.


You said:
## Rag Problems

1. **Chunking** - RAG systems usually split documents into smaller pieces (“chunks”) before storing them.  the problem is how to recognize where ideas naturally begin and end so that the chunks or set of chunks will contain complete and useful information without any extra.

2. **Extraction** -  how to select which chunk or set of chunks are relevant to answer the question without introducing extra information.

3. **System Prompt** - given a users request, we might retrieve the correct information but the LLM still answers not according to how the user intended.  the System Prompt captures the intended use, style and form of how the user expects to see the answer.

## Naive Solutions

1. **Chunking** - 
2. **Extraction** - 
3. **System Prompt** - 

please fill in the naive solutions

ChatGPT said:
Naive Solutions
1. Chunking –
Split documents into fixed-size chunks (for example, every 500–1,000 words or tokens) with small overlaps between chunks. This ensures coverage of all content and reduces the chance of cutting off sentences, but it does not truly understand where ideas begin or end.

2. Extraction –
Use simple similarity search: convert the question and each chunk into embeddings, then retrieve the top k most similar chunks. Optionally increase k to reduce the risk of missing relevant information. This improves recall but may introduce irrelevant or redundant content.

3. System Prompt –
Write a static system prompt that instructs the model to:

Use only the retrieved documents

Follow a specific style (e.g., concise, formal, step-by-step)

Avoid adding outside knowledge

This provides consistent behavior but does not adapt to different users, tasks, or document types.


You said:
## Naive Solutions

1. **Chunking** - Split documents into fixed-size chunks (for example, every 500–1,000 words or tokens) with small overlaps between chunks. This ensures coverage of all content and reduces the chance of cutting off sentences, but it does not truly understand where ideas begin or end.
2. **Extraction** - Use simple similarity search: convert the question and each chunk into embeddings, then retrieve the top k most similar chunks. Optionally increase k to reduce the risk of missing relevant information. This improves recall but may introduce irrelevant or redundant content.
3. **System Prompt** - Write a static system prompt that instructs the model to:

Use only the retrieved documents

Follow a specific style (e.g., concise, formal, step-by-step)

Avoid adding outside knowledge

This provides consistent behavior but does not adapt to different users, tasks, or document types.

please rewrite these to be simple, clear and be able to understand by non technical readers without over simplifying

ChatGPT said:
Naive Solutions
1. Chunking –
Divide documents into equal-sized pieces, such as every few paragraphs, sometimes with a small overlap between them. This makes the system simple and predictable. However, it does not understand meaning, so it may split an idea in the middle or group unrelated information together.

2. Extraction –
When a question is asked, search for the chunks that seem most similar to the question and select the top few results. If unsure, retrieve more chunks to avoid missing something important. This is easy to implement, but it may include extra or irrelevant information that can distract the model.

3. System Prompt –
Use a fixed instruction that tells the model how to behave, for example: only use the retrieved documents, answer in a certain style, and avoid adding outside knowledge. This creates consistency, but it does not adjust to different types of questions, users, or situations.


You said:
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

1. Objective
The goal of this proposal is to design a RAG system that improves over time through structured user feedback. Instead of relying on fixed rules for chunking, extraction, and prompting, the system would refine these components based on how users evaluate its answers.

The system is intended to adapt across different domains such as coding, legal, educational, medical, and agent-based workflows. Each domain has different expectations for accuracy, structure, tone, and level of detail. A static RAG pipeline typically must be manually tailored to each domain, which can be costly and difficult to maintain.

2. Core Idea
The central idea is to treat each answer as a draft that can be reviewed and improved. After the system generates a response, the user provides explicit feedback — similar to reviewing a Merge Request in software development.

This feedback is not only about whether the answer is correct, but also whether:

The right information was retrieved

The answer included too much or too little detail

The format matched expectations

The tone or structure was appropriate

The system then uses this feedback to adjust its internal behavior.

3. How the Feedback Mechanism Works
The feedback mechanism operates at three levels:

a) Chunking Adjustment
If answers consistently miss context or include unnecessary information, the system can adjust how documents are split.
For example:

In legal domains, it may learn to keep larger sections together to preserve full clauses.

In coding domains, it may learn to isolate functions or classes as separate chunks.

b) Retrieval Adjustment
If users often say the answer is incomplete or overloaded with irrelevant details, the system can refine how many chunks it retrieves and which types it prioritizes.
Over time, it learns what “relevant” means within each domain.

c) System Prompt Adjustment
If users frequently request changes in style (e.g., “be more concise,” “explain step-by-step,” “cite sources”), the system can update its internal instructions to match those expectations automatically in future interactions.

4. Domain Adaptation
Different domains require different response patterns:

Coding: precise, structured, minimal ambiguity

Legal: careful wording, complete context

Education: explanations, examples, progressive clarity

Medical: cautious, clearly sourced, structured

Agent workflows: action-oriented, structured outputs

Instead of manually designing a separate RAG pipeline for each domain, this system would gradually learn these expectations from user feedback.

5. Expected Outcome
Over time, the RAG system becomes domain-aware and aligned with user expectations. It learns how to:

Organize information more effectively

Retrieve the right level of detail

Present answers in the expected structure and tone

The result is a RAG system that improves through interaction. Rather than remaining static and rule-based, it evolves to better serve specific domains and individual users while reducing the need for manual customization.
