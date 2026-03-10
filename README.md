# n8n-agent-orchestrator

A personal experiment to stop forgetting things.

## The problem

I kept missing tasks and calendar events. I wanted to capture them 
quickly in plain language without opening five different apps. 
So I built this.

## What it does

You give it a text input. It figures out what you want and does 
one of two things:

- Save it as a task
- Add it to the calendar

That's it. Simple on purpose.

## How it works

A local LLM (Llama 3.1 via Ollama) reads the input and decides 
which action to trigger. n8n handles the routing and execution. 
I added JavaScript Code nodes between steps to normalize the LLM 
output — without them, the responses were too inconsistent to 
work with reliably.
```
Input → LLM Router (Ollama/Llama 3.1) → Intent classifier
            ├── "add to list"     → Task node
            └── "add to calendar" → Calendar node
```

## Stack

- **n8n** — orchestration
- **Ollama** — local LLM runtime
- **Llama 3.1** — natural language routing
- **JavaScript Code nodes** — output normalization

## What I learned

Keeping the scope tight was the right call. One agent, two actions, 
no distractions.

The hardest part wasn't the routing — it was getting consistent output 
from the LLM. Dates especially. Llama 3.1 would hallucinate formats 
constantly, so I ended up writing normalization logic in JS to sanitize 
everything coming out of the model before passing it downstream.

I also added sub-agents at some point. Removed them. They added 
complexity without adding value at this scale.

## What I'd do differently

Llama 3.1 was the main bottleneck — too many hallucinations for 
something that needs to be reliable. Next time I'd start with a 
stronger model or use the Anthropic API directly.

I'd also skip n8n for the orchestration layer and build on top of 
**OpenClaw** or **LangGraph** from the start. More control, 
better tooling for agents specifically.

## Status

Working local prototype. Rebuilding with LangGraph next.