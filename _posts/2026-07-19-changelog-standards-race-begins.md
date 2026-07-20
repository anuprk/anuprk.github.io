---
title: "Changelog: Standards Race Begins"
description: "The model wars have a new shape, agents are getting their own protocol stack, Google can't ship, and governments want a seat at the table."
categories: [AI]
tags: [ai, llm, agents, mcp, regulation, changelog]
---

A month ago I would have told you the frontier model race was a two-horse contest between Anthropic and OpenAI, with Google trailing and occasionally releasing something impressive on a benchmark nobody cares about. That framing is wrong now, but not in the direction you'd expect.

# The Model Releases: GPT-5.6 and Fable 5

OpenAI shipped GPT-5.6 on July 9 as a three-tier family: Sol (frontier), Terra (mid-range), and Luna (cheap and fast). The interesting bit isn't the benchmarks. It's the architecture of the offering. They've stopped pretending one model fits all use cases and instead ship what amounts to a product line. Sol for research and complex reasoning. Terra for everyday professional work where you want quality but not the cost. Luna for high-volume API calls where latency and price matter more than peak intelligence.

The feature list tells you where OpenAI thinks the money is: Programmatic Tool Calling, persisted reasoning across sessions, multi-agent orchestration in beta, and explicit prompt caching controls. These are all enterprise integration features. The consumer chatbot is the loss leader; the API is the business.

Anthropic's Fable 5 came back globally on July 1 after export control restrictions were lifted. Then demand exceeded compute capacity, and Anthropic scrambled to manage it. Starting July 20, Fable 5 rolls into Max and Team Premium plans at 50% of usage limits. Pro and Team Standard users get $100 in credits. The compute constraint is real — Anthropic signed a $45 billion SpaceX compute deal and is reportedly in talks to lease $10 billion in capacity from Meta.

Two things stand out. First, we've reached the point where model demand outstrips available compute even for the companies building the models. Second, the pricing gymnastics (tiers, credits, usage caps) signal that the "unlimited AI" framing of 2024-2025 is over. AI inference is expensive, and someone has to pay for it.

Meanwhile, Anthropic merged Chat and Cowork into a single interface, added local file reading, and shipped persistent memory across sessions. These feel like the right UX moves for people who actually use Claude for work rather than party tricks.

# Google's Gemini 3.5 Pro: Three Missed Deadlines

Google announced Gemini 3.5 Pro at I/O in May. Sundar Pichai said "give us until next month." June came and went. July 17 was the next target. Missed again. Reports say the rebuilt model still has hallucination and consistency problems that prevent it from clearing basic reliability bars.

This matters less for the model itself and more for what it says about Google's position. They have the compute, the talent, and the distribution (every Android phone, every Chrome browser, Gmail, Docs). What they don't seem to have is the ability to ship frontier models on schedule. Three consecutive misses erodes developer trust. If you're building on Gemini APIs, you're building on a platform whose flagship product keeps slipping.

Separately, Google killed Gemini Code Assist for individual developers on July 17. The free tier that gave generous monthly completions is gone. The message: coding AI is a business now, not a growth hack for API adoption.

# The Agent Protocol Wars

The MCP (Model Context Protocol) spec hit its release candidate on July 28. The final specification introduces a stateless protocol core, an Extensions framework, Tasks, MCP Apps, authorization hardening, and a formal deprecation policy. If you're running MCP servers in production, you have a ten-week migration window.

But the bigger story is fragmentation. We now have MCP, WebMCP, A2A (Agent-to-Agent), and various proprietary agent frameworks all competing for adoption. Google, Microsoft, Salesforce, Snowflake, and ServiceNow are reportedly co-developing a shared "backend" approach for enterprise agent connectivity. Citrix shipped an MCP Gateway for routing and governing agent traffic. Banks are deploying agents faster than their security teams can audit the connections.

The pattern looks familiar: new technology emerges, everyone builds their own version, then the enterprise buyers demand interoperability, and a few winners emerge as standards. We're in the "everyone builds their own version" phase right now. The question is whether MCP's head start is enough to become the TCP/IP of agent connectivity, or whether the enterprise consortium approach fragments it into a WebSocket-vs-SOAP situation.

For practitioners building agents today, I'd bet on MCP for tool connectivity (it has the broadest adoption) and watch A2A for agent-to-agent communication. But pin your dependencies — the spec is still moving.

# AI Coding Tools: The IDE Wars Intensify

85% of developers now use AI coding assistants regularly, according to JetBrains. The market has moved past autocomplete into full workflow ownership. The fight is over who controls the developer experience: the IDE (Cursor, Kiro), the terminal (Claude Code), the browser (Lovable, v0), or the platform (GitHub Copilot).

Google dropping Gemini Code Assist's free tier is one signal. Another: the tools that are winning are the ones that understand your whole repo, not just the file you're editing. Context window size used to be a benchmark curiosity. Now it's a product feature that determines whether your coding assistant can actually help with a 200-file refactor or just suggest one-liners.

I built a 49,000-line app recently using Kiro without typing code directly. The structured spec workflow (requirements, design, tasks) was what made it work. Raw chat-with-AI produces inconsistent results. Structured prompting with review gates produces shipping software. The tooling gap between "AI that writes code" and "AI you can build production systems with" is still large.

# Governance: Everyone Wants a Seat, Nobody Wants to Regulate

The UN held its first all-nations AI governance forum in Geneva in early July. Scientists presented warnings about catastrophic risk from increasingly capable systems. Every country got a seat at the table. Nothing binding came out of it.

The US executive order on AI security opts for voluntary model testing. Companies can present frontier models to the government before release. The keyword is "can," not "must." Demis Hassabis from DeepMind called for a US-led AI standards body. The EU passed its Digital Omnibus on AI Regulation. Ten more countries signed the Pax Silica Declaration.

The US approach and the EU approach are diverging further. The US says: let industry self-regulate, we'll test the models if you want us to. The EU says: mandatory audits, safety frameworks, penalties for noncompliance. Neither approach has proven it can keep up with the pace of model development.

Anthropic is proposing a jailbreak severity scoring framework with Amazon, Microsoft, Google, and other Glasswing partners. This is industry trying to get ahead of regulation by defining the metrics before regulators do. Whether that's responsible self-governance or regulatory capture depends on how cynical you're feeling.

# Open Questions

1. If compute is the bottleneck for frontier models (Anthropic can't meet Fable 5 demand, Google can't get 3.5 Pro reliable enough to ship), does that mean the pace of capability improvement is about to slow down, or does it mean whoever solves the compute problem wins the next round?

2. The agent protocol stack is forming in real time. Will we get one standard (like HTTP) or a fragmented mess (like messaging protocols)? What does it take for MCP to become the default, and who loses if it does?

3. At what point does "85% of developers use AI assistants" change the economics of software teams? We're generating code faster, but are we shipping better software faster? The quality question hasn't been answered yet.
