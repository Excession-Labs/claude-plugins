---
name: llm-app-security-review
description: Review an LLM-powered app, chatbot, RAG system or AI agent against the OWASP Top 10 for LLM Applications 2026, with proof for each finding. Use when asked to review or red-team an LLM app, check prompt injection, excessive agency, system prompt leakage, RAG or tool and MCP security.
---

# LLM app security review

## When to use

You own the AI application or have written authorization to test it. Map its inputs (user text, files, retrieved documents, tool output), its tools and its data first.

## Checklist, highest impact first

OWASP Top 10 for LLM Applications 2026 (published 2026-08-04). The 2025 list is the previous edition and numbers differ.

1. **LLM03 Excessive Agency.** List each tool and what it can change. Trigger an action with injected text. Evidence: the tool call log and the changed state, read back.
2. **LLM01 Prompt Injection.** Put instructions in a document, web page, email or tool result the app reads. Evidence: the transcript showing the app followed them.
3. **LLM02 Sensitive Information Disclosure.** Ask for other users' data, secrets or logs. Evidence: the data in a response, tied to a second account.
4. **LLM09 Vector and Embedding Weaknesses.** Query as tenant B for tenant A's documents. Evidence: A's content in B's answer.
5. **LLM10 Improper Output Handling.** Check whether output renders as HTML or markdown images, or reaches a shell, SQL or URL fetch. Evidence: the rendered payload or outbound request.
6. **LLM08 Hidden Context Exposure.** Try to extract the system prompt. Assume it can be found; check that it holds no secrets. Evidence: the output.
7. **LLM06 Unbounded Consumption.** Check token, rate and spend limits with small bounded tests. Evidence: counts and the limit that applied.
8. **LLM04 Supply Chain, LLM05 Poisoning, LLM07 Misinformation.** Review model and package sources, ingestion paths, and whether people or agents act on unchecked output. Evidence: the config, the source list or the transcript.

## Safe testing

- Own systems or written authorization only.
- Use test accounts, test documents and a test mailbox. No real user data.
- Do not point injected instructions at real recipients. Use a callback you control.
- Cap spend on paid model or tool calls and state the cap.

## Evidence rule

A model saying "done" is not proof. Show the tool log, the changed record read back, or the callback hit. Model output varies, so repeat and state the count.

## Report

Finding, evidence (prompt, transcript, tool log), impact, fix. Add "Controls that held": injections that the app refused or an approval step that blocked, with the transcript.

## Guide

OWASP Top 10 for LLM Applications: https://airedteamingservices.com/guides/owasp-top-10-for-llm-applications/
