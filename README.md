### A pentester's walkthrough — and what these labs actually teach about agent security

I work as a penetration tester and I've been specialising into AI agent and MCP security. PortSwigger's Web LLM Attacks track is the best free hands-on introduction to this space I've found, so I worked through all eight labs and wrote up my reasoning rather than just the answers.

The thing that struck me most: **almost none of this is new.** It's command injection, SQL injection, SSRF, XSS and confused deputy — bug classes with two decades of playbooks — reappearing because we've inserted a language model between the attacker's input and the system's actions. The model is not the vulnerability. The model is the *delivery mechanism*.

If you're a pentester wondering whether your skills transfer to AI security, this write-up is my answer: they do, almost entirely.

> **Scope.** These are PortSwigger's deliberately vulnerable training labs, with officially published solutions. Nothing here is a live vulnerability. Lab objectives are paraphrased — see [PortSwigger's Web Security Academy](https://portswigger.net/web-security/llm-attacks) for the originals.

---
