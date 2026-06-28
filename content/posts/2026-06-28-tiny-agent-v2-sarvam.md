---
title: "A Tiny Agent, v2: Python and the Sarvam API"
date: 2026-06-28T18:00:00+05:30
draft: false
tags: ["ai agents", "python", "sarvam"]
---

After the Go [tiny coding agent](/blog/posts/2026-06-28-building-a-tiny-agent/),
I rewrote the same thing in Python, this time pointed at the
[Sarvam](https://www.sarvam.ai/) API instead of Gemini. Sarvam is an Indian
company which is trying to work Indian Sovereign. The models are strong on Indian
languages, which made it interesting to target. The code lives in
[`test-agent-v2`](https://github.com/bharathbhushan1/ai-learning/tree/main/test-agent-v2),
managed with [uv](https://docs.astral.sh/uv/).

Functionally it is the same agent: a stdin-driven loop, the same three file
tools (`read_file`, `list_files`, `edit_file`), the same Sudarshana ☸️ agent. I had typed
out most of the v1 agent by hand with as little AI help as possible. I just wanted to
get into the weeds of the mechanics, ser-deser, order of various operations, different
data structures needed etc. This time I felt confident I could relate to the different 
parts of the agent. So I just asked Claude to rewrite it in python.


## OpenAI-compatible means the protocol is different

The Go agent spoke Gemini's `generateContent` dialect — `contents` made of
`parts`, only `user` and `model` roles, tool results sent back inside a
`user` turn as `functionResponse` parts. Sarvam instead exposes an
**OpenAI-compatible** `POST /v1/chat/completions` endpoint, so the whole
conversation is the standard OpenAI chat pattern:

| | Go agent (Gemini) | v2 (Sarvam) |
| --- | --- | --- |
| Endpoint | `…/models/{model}:generateContent` | `…/v1/chat/completions` |
| Auth | `x-goog-api-key` header | `Authorization: Bearer <key>` |
| Conversation | `contents` of `{role, parts}` | `messages` of `{role, content, …}` |
| Roles | `user`, `model` only | `system`, `user`, `assistant`, `tool` |
| Tool result | a `functionResponse` part in a `user` turn | a dedicated `tool` message keyed by `tool_call_id` |

`tool` role makes the plumbing simpler to understand. Every tool result is its own
`tool` message carrying a `tool_call_id` that ties it back to the exact
`tool_calls` entry the assistant emitted.

Claude also nudged me to add a system prompt using the `system` role.
The Gemini version did not have an obvious place to put the system problem.

There is a `DEBUG=1` mode that pretty-prints the actual request body each turn.
There is also a per-call `usage` log.

## A translate tool, and when not to use it

The one genuinely new tool is `translate`. Sarvam has a *dedicated* translation
model (`sarvam-translate:v1`) separate from the chat model, and it exposes knobs
the chat path doesn't: register (formal / colloquial / code-mixed), output
script (native, roman transliteration, spoken form), and so on. It also lives at
a different endpoint and — authenticates with a different header
(`api-subscription-key` instead of `Authorization: Bearer`), so the tool reaches
out to a second API entirely.

Two wrinkles worth recording:

- **No auto-detect.** `sarvam-translate:v1` rejects `"auto"` as a source
  language, so the model has to identify the source itself and pass an explicit
  code (`en-IN`, `kn-IN`, …) (or so it seems). The chat model does this reliably.
- **Native scripts.** Tool results get serialized with `ensure_ascii=False`,
  otherwise Kannada comes back as `\uXXXX` escapes that the model dutifully
  echoes verbatim. You want real characters reaching the model.

But the more interesting decision was to leave it **off by default**. The
dedicated model costs roughly ₹20 per 10K characters, versus about ₹10 per 1M
output tokens for just letting the chat model translate inline — call it a ~100x
premium. And `sarvam-30b` is itself multilingual, so for ordinary requests
inline translation is fine. The expensive purpose-built tool only earns its keep
for low-resource languages or when you actually need its controls. So it's gated
behind `ENABLE_TRANSLATE_TOOL=1`, and the system prompt only steers the model
toward it when it's enabled. A nice reminder that "there's a specialized API for
this" and "you should call the specialized API" are different questions.

## Next

Same as before — more interesting tools. But porting the agent across two
providers was its own lesson: the agentic loop is identical, and basically all
the friction lives in how each vendor models a conversation and a tool call.
