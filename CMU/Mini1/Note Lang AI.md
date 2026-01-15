# Jan 15 Thu
![[Lecture_2_Prompt_Engineering.pdf]]
## #SLM  - small language model
- 1 - 7 B
- cheaper for specific job and similar performance for those 70B
- don't use SLM **individually** for complex reason
- examples: Phi-2, Phi-3, Gemma 2B, Gemma 7B.

## #LAM  - large Action models
- use for robotic control, workflow, automation and heavy-resource agent
- LAM don't just communicate with you and they take action in real world.
- avoid using them without safety guardrails. ==A mistake may lead to hardware failure==

## #HLM - hierarchical Language Models (Agent design pattern)
- hlm are system-level orchestrations of language models
- HLMS use 2 tier architecture to handle complex task to decomposing them
- High tier for planing and task decomposition
- lower tier using for execute tasks
- generally slow carries the compound bias of two llms.
- examples: hierachical workflows in LangGraph, AutoGPT BabyAGI.

## #LCM - large concept models
- LCM operate over abstract concept and structured knowledge representations rather than raw text
- analogy: LLM thinks in tokens while LCM thinks in idea??
- future of LLM? 
- LCM aim to reason at the conceptual level not the word level
- ex: NSCL
## #VLM - visual LM

## #MoE -Mixture of Experts 
A Mixture of Experts (MoE) model is ==an AI architecture that uses multiple specialized neural networks (experts) and a gating network to efficiently process complex inputs==, activating only relevant experts for each task, making models larger and more capable while remaining computationally efficient, unlike traditional "dense" models that use all parameters for every input. Popular examples include Mixtral and Grok-1

---
## LLM limitation
- not work well with tabular mathe cal
- length of input and output 
- knowledge cutoff: llm ara limited to the trating state
- bias and toxicity
- hallucinations

## Overcoming LLM Limitations: Prompt Engineering
- bad prompt -> bad output
- need guide the LM towards more accurate and relevant response

## AI Prompt Components: Required Elements
- task
- format
- topic
- tone or style - how it should sound serious casual professional friendly ???
-  context
-  requirements of constraints

## AI Prompt Components: Optional Elements
- goal: tell the ai what outcome you want, what should the reader to expect

Pormpting Strategies
- Chain of thought - most popular -
	- if all bear are mammals and some mammals can swim, can we condlude that all bear can swim? ==Provide step by step reasoning,==
- self - consistency prompting (majority voting on COT )
	- CoT improves reasoning but can be inconsistent. Repeated prompts may yield different answers due to sampling temperature.
	- Self-consistency prompting asks the model to generate multiple independent reasoning paths. • The most frequent final answer is selected (majority vote). 
	- The method evaluates agreement among answers, not the quality of reasoning.
- tree of thought (TOT)
	- TOT varies the reasoning steps themselves.
	- at each step llm explores mutile possible reasoning directions
	- TOT often outperforms basic CoT
	- #Tree-of-Thoughts Prompt Example 
		1. Generate at least three different possible approaches. 
		2. Evaluate each approach. 
		3. Determine which approach makes the most sense. 
		4. Proceed to perform the task using the chosen approach. 

## Summary
- Use structured prompting when prompting LLMs. 
- include essential, and ideally optional, prompt components to your prompt. 
- Prompt LLMs in JSON or Markdown-like format. 
- Prefer Chain-of-Thought prompting for low-cost reasoning, Self-Consistency for problems with multiple plausible solutions, and Tree-of-Thoughts for complex tasks that require exploring alternative reasoning paths.

---
