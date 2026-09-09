# Breaking LLM Applications: All 8 PortSwigger Web LLM Labs

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
# What these labs add up to

Six patterns worth carrying into real assessments.

**1. The tool list is the attack surface.** Enumeration works. Ask what functions exist and how they work - the model will usually tell you in detail, and the attack path is normally obvious from the answer.

**2. Excessive agency is the root cause more often than injection.** Lab 1 needed no injection at all. A debug SQL console was exposed to a public chatbot. Injection is how you *reach* excessive agency; the agency is the actual bug.

**3. The model can't separate data from instructions.** Retrieved content and operator instructions arrive as text in the same context. Every mitigation is an attempt to rebuild a boundary the architecture lacks.

**4. Agents are confused deputies by construction.** They hold credentials you don't and process content you control. Nothing needs to be bypassed - the attacker supplies intent and the agent supplies authority.

**5. The best injections wear the agent's own uniform.** Direct commands fail against defended agents. Tasks framed in the agent's purpose succeed. Labs 5, 6, 7 and 8 are all variations on this.

**6. Detection is not prevention.** In Lab 4 the model explicitly flagged the payload as suspicious - and the payload still executed in the victim's browser.

## Mitigations that actually help

Roughly in order of how much blast radius they remove:

- **Least privilege on tools.** Don't expose `debug_sql` to a customer chatbot. Most of these labs die here.
- **Scope agent credentials to the task**, not to the user's full entitlements. An agent reviewing a blog comment doesn't need account deletion rights.
- **Human approval for actions with external side effects** - deletions, payments, outbound posts.
- **Treat model output as untrusted on the way out.** Encode it. Lab 4 is a rendering bug, not an AI bug.
- **Fix the underlying vulnerability.** Lab 2 was an OS command injection and Lab 7 was an SSRF. The LLM was a new route to old bugs.
- **Log tool invocations.** In Labs 5–8 the agent's own actions are the only forensic trail.
- **Don't rely on injection filters alone.** Lab 8 shows what happens when the filter checks shape rather than intent.

---
*Write-up by https://x.com/szzeth Corrections welcome - if I've misread something, I'd rather know.*
