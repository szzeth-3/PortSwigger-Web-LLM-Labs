## A pentester's walkthrough and what these labs actually teach about agent security

I work as a penetration tester and I've been specialising into AI agent and MCP security. PortSwigger's Web LLM Attacks track is the best free hands-on introduction to this space I've found, so I worked through all eight labs and wrote up my reasoning rather than just the answers.

The thing that struck me most: **almost none of this is new.** It's command injection, SQL injection, SSRF, XSS and confused deputy, bug classes with two decades of playbooks, reappearing because we've inserted a language model between the attacker's input and the system's actions. The model is not the vulnerability. The model is the *delivery mechanism*.

If you're a pentester wondering whether your skills transfer to AI security, this write-up is my answer: they do, almost entirely.

> **Scope.** These are PortSwigger's deliberately vulnerable training labs, with officially published solutions. Nothing here is a live vulnerability. Lab objectives are paraphrased - see [PortSwigger's Web Security Academy](https://portswigger.net/web-security/llm-attacks) for the originals.

---

## The methodology that emerged

By the third lab I'd settled into a pattern that held for the rest:

1. **Enumerate the tool surface.** Ask the model what functions it can call. It usually just tells you.
2. **Ask it to explain each one in detail.** Parameters, behaviour, what it returns. This is free reconnaissance and it's where the attack path becomes obvious.
3. **Identify excessive agency.** Which tool does more than this product needs?
4. **Find the injection channel.** Where does attacker-controlled text enter the model's context?
5. **Exploit the gap** between what the tool can do and what the application intended.

Steps 1 and 2 are just enumeration. The tool list *is* the attack surface, exactly like a parameter list in a web app.

---
