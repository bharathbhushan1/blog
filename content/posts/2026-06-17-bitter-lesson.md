---
title: "The Bitter Lesson"
date: 2026-06-17T10:00:00
draft: false
tags: ["machine learning", "reinforcement learning"]
---

This post is a short summary of Rich Sutton's [Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) essay. 

### Context
- Cost of computation reduces exponentially with time.
- "Brute force" approaches which use compute win over human-aided or human-like problem solving over a medium/long time horizon.
- Search is one such computation technique. This helped in Deep Blue winning chess.
- Learning is another. Learning with self-play is yet another. This helped in AlphaGo winning in Go.
- A Hidden Markov model which models sequential processes is yet another computation technique. This helped in speech recognition.
- Deep learning for speech recognition is another step in this direction of using computation instead of human-modeled features.
- Convolutional Neural networks solved image recognition and other vision problems.

### Bitter Lesson
- The bitter lesson is that building systems based on our understanding of
  human knowledge and specifics of problem solving does not work in the long
  run. AI researchers build knowledge into their agents and this helps in the
  short term. In the long term, progress plateaus and even slows down. A
  breakthrough happens by using search/learning along with scaled compute.

### Learnings
- Learning 1: General methods like `search` and `learning` are very powerful and highly scalable with compute.
- Learning 2: The world is very complex. The content of our minds reflects this
  extreme complexity. But the mental processes and meta-methods that we use to
  capture and represent this arbitrary complexity should we built into AI
  systems. We want AI agents to use our discovering process and not need the
  knowledge we have discovered.
