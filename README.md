# Shared Context Reduction (SCR)

**The systematic elimination of information already mutually known between communicating agents.**

---

## What Is SCR?

When two parties share prior knowledge, most transmitted information is redundant. SCR is the principle of removing that redundancy — not by compressing data, but by not sending what both sides already know.

- An LLM that knows Python syntax doesn't need you to explain it.
- A robot that has mapped a room doesn't need you to describe the furniture.
- A voice assistant that knows your routine doesn't need you to spell it out.
- A car's AI that has driven a route 200 times doesn't need turn-by-turn directions.

SCR is not compression. Compression encodes the same information in fewer bits. SCR transmits less information, because the missing portion already exists at the receiver.

---

## Five Principles for Safe Reduction

Removing shared context only works if both sides agree on what may be inferred from the gaps. Without constraints, the receiver fills gaps with guesses — and guesses, at scale, become failures.

These five principles form a decision hierarchy, from broadest to strictest.

### 1. Boundary of Omission

The receiver may only infer universally established facts: programming language syntax, framework conventions, mathematical constants, physical laws.

It must not infer: culture-dependent knowledge, time-sensitive information, personal experience, or emotional nuance.

*Example:* A Django project that omits `language:python` — Python can be safely inferred. A project that omits its status — "everything is fine" cannot be inferred. Project health is not a universal fact.

*Theoretical basis:* Grice's Cooperative Principle (1975).

### 2. Prohibition of Implicature

The receiver must not generate implied meaning beyond what is explicitly stated.

`test:pass` means "tests passed." It does not mean "code quality is high," "ready to deploy," or "no bugs remain."

*Theoretical basis:* Grice's theory of conversational implicature. Implicature is culture- and context-dependent. Automated implicature has no guarantee of matching sender intent.

### 3. Verification of Presupposition

When the receiver encounters uncertain presuppositions, it must pause inference and request clarification rather than guess.

`bug:auth-again` — "again" presupposes a prior occurrence. Without a record of a previous auth bug, the receiver interprets this only as "auth-related bug" and does not infer recurrence.

*Theoretical basis:* Presupposition theory in formal semantics.

### 4. Safe Interpretation Selection

When input permits multiple interpretations, the receiver must select the lowest-risk interpretation.

`status:debugging` could mean a status report (low risk), a help request (medium risk), or a warning (high risk). The receiver selects the status report. If the sender meant otherwise, they can clarify.

*Theoretical basis:* Austin and Searle's Speech Act Theory (1962/1969).

### 5. Precision Guarantee

Numbers, negations, conditions, and exceptions are never inferred. If a value requires precision, it must be explicitly stated.

- `timeout:long` → must not be resolved to a number. Must ask.
- `auth:no-password` → scope of negation must be confirmed.
- `deploy:staging` → conditions and constraints must be verified.

*Theoretical basis:* Scalar implicature and negation scope theory.

---

## Principle Hierarchy

These principles are not equal. They form a decision chain where each level tightens the constraint:

```
Principle 1 (highest priority):
  → What CAN be inferred? Draw the boundary.

Principle 2:
  → Within that boundary, generate NO implied meaning.

Principle 3:
  → If a presupposition is uncertain, STOP and ask.

Principle 4:
  → If multiple interpretations remain, choose the SAFEST.

Principle 5 (strictest):
  → If precision is needed, NEVER guess. Always ask.
```

---

## Relationship to LNDF

SCR is the general principle. [LNDF](https://github.com/moncface/lndf) (LLM-Native Data Format) is the first formal implementation of SCR, applied to structured data transmission between humans and LLMs.

```
SCR    = What to remove    (the principle)
LNDF   = What to write     (the format)
5 Principles = What to infer (the safety constraints)
```

- **SCR** defines the process: eliminate mutually known information.
- **LNDF** defines the sender's discipline: six principles for writing minimal, intent-focused data.
- **The Five Principles** define the receiver's constraints: what may and may not be reconstructed from omitted information.

---

## Applicability

SCR is medium-independent and agent-independent. It applies wherever information is transmitted between parties who share prior context:

- LLM communication (text, API, agent frameworks)
- Robotics and physical AI (sensor data, status reporting, command interpretation)
- Voice interfaces (conversational assistants, in-car systems)
- IoT and edge computing (device-to-cloud telemetry, firmware updates)
- Multi-agent systems (agent-to-agent coordination)

---

## Theoretical Foundations

The Five Principles draw from established work in linguistics and philosophy of language:

- Grice, H.P. (1975). *Logic and Conversation.* — Cooperative Principle, conversational implicature
- Austin, J.L. (1962). *How to Do Things with Words.* — Speech act theory
- Searle, J.R. (1969). *Speech Acts.* — Illocutionary force, indirect speech acts
- Brown, P. & Levinson, S.C. (1987). *Politeness: Some Universals in Language Usage.* — Face-threatening acts in communication
- Sperber, D. & Wilson, D. (1986). *Relevance: Communication and Cognition.* — Relevance theory, optimal inference

---

## Specification Governance

This specification is maintained by Hiroaki Tachibana ([moncface](https://github.com/moncface)).

- Change proposals: Submit via GitHub Issues
- Final decision authority: The maintainer (BDFL model)
- Versioning: Semantic versioning (current: v0.1)
- Canonical URL: This README is the authoritative specification

---

## License

Specification and documentation: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

The concepts described are freely available for use, adaptation, and extension with attribution.

---

*Hiroaki Tachibana / [moncface](https://github.com/moncface)*
*April 2026*
