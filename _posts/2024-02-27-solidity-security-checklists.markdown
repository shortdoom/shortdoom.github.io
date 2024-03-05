---
layout: post
title: "Solidity contract security checklists"
subtitle: "AI x Security r&d dump"
date: 2024-02-27 21:52:16 +0100
categories: security
---

Unintended results of running benchmarks with available GPT-as-an-audit tools (GPTScan & GPTLens) were creation of some security checklists to guide LLM towards the better answer. I didn't run it very diligently so there's no actual data to back it up (now), but with or without guided approach GPT was hallucinating still too much to rely on it as an actual productivity boost in security review of smart contract code.

At the same time those lists are pretty useful for human security engineer. Following lists are created through different techniques. They are manually reviewed, but mostly automatically generated. Automatically generated doesn't mean pure GPT output was used. A conversational approach with data samples and appropriate prompt engineering was the guiding principle of generating short and succicint security ruleset. Dataset was mixed, containing

1) Public checklists available

2) Exploit replay databases

3) Academic databases

4) Manually choosen specific attack vectors

Lists are incomplete in scope. Meaning, those don't describe all of the SCV or all of the possible attacks. Priority was put on checklist that LLM can deduce from, so multi-transaction attack like Flash Loans or very specific rounding errors are basically left uncovered.

Might be useful to some, seeing how such lists were very hot topic in 2020-2022 cycle