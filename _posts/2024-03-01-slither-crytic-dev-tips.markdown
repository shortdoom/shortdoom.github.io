---
layout: post
title: "Slither & Crytic-compile dev notes"
subtitle: "Non official tips & tricks for static analyzer and compile helper library"
date: 2024-03-01 21:52:16 +0100
categories: slither
---

Unintended results of running benchmarks with available GPT-as-an-audit tools (GPTScan & GPTLens) were creation of some security checklists to guide LLM towards the better answer. I didn't run it very diligently so there's no actual data to back it up (now), but with or without guided approach GPT was hallucinating still too much to rely on it as an actual productivity boost in security review of smart contract code.

At the same time those lists are pretty useful for human security engineer. Following lists are created through different techniques. They are manually reviewed, but mostly automatically generated. Automatically generated doesn't mean pure GPT output was used. A conversational approach with data samples and appropriate prompt engineering was the guiding principle of generating short and succicint security ruleset. Dataset was mixed, containing