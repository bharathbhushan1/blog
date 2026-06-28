---
title: "Building a Tiny Coding Agent in Go"
date: 2026-06-28T09:00:00+05:30
draft: false
tags: ["ai agents", "go"]
---

I built a small CLI coding agent in Go, following Thorsten Ball's
[How to Build an Agent](https://ampcode.com/notes/how-to-build-an-agent). It
runs against the free tier of Gemini (`gemini-2.5-flash-lite`), so the whole
thing costs nothing to play with. The code lives in
[`test-agent`](https://github.com/bharathbhushan1/ai-learning/tree/main/test-agent).

The _Agent_ abstraction has a method to get user input, an api key to call the
LLM and a list of tools. The Agentic loop gets textual user input, sends it
to the LLM, and displays the output back to the user.

A separate _conversation_ data abstraction captures all "persistent" aspects of
the chat. Basically anything that needs to be sent to the LLM and the LLM
responses.

A _Tool_ abstraction represents some functionality that the client can invoke.
Currently `read_file`, `list_files` and `edit_file` are implemented.

The agent harness is a simple Go program. There is no system prompt.

The blog post has some good examples of what you can do with this simple agent.

One thing I learned from this inteaction pattern - the agent can make a series
of LLM calls to supply all the tool outputs to the LLM without going back to the
user. In this case, only one LLM call is made to send all tool outputs in one shot.

Another learning was that Gemini's REST API only accepts the `user` and `model`
roles — there's no dedicated `function` role — so tool results have to be sent
back inside a `user` turn, each part carrying a `functionResponse`.

The Pro models require billing; the Flash variants are the ones you can hit
for free.

The agent is named Sudarshana ☸️. Next steps are giving it more interesting
tools and a proper system prompt.
