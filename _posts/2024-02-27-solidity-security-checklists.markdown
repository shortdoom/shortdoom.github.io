---
layout: post
title: "solidity contract security checklists"
subtitle: "ai x security r&d dump"
date: 2024-02-27 21:52:16 +0100
categories: security
---

Unintended results of running benchmarks with available GPT-as-an-audit tools (GPTScan & GPTLens) were creation of some security checklists to guide LLM towards the better answer. I didn't run it very diligently so there's no actual data to back it up (now), but with or without guided approach GPT was hallucinating still too much to rely on it as an actual productivity boost in security review of smart contract code.

At the same time those lists are pretty useful for a human security engineer. Following lists are created using different techniques. They are manually reviewed, but mostly automatically generated. Automatically generated doesn't mean pure GPT output was used. A conversational approach with data samples and appropriate prompt engineering was the guiding principle of generating short and succicint security ruleset. Dataset was mixed, containing

1) Public checklists available

2) Exploit replay databases

3) Academic databases

4) Manually choosen specific attack vectors

Here's the more detailed table of sources used:

| Repository or Article                                                    | Type          | Focus    | Real World Examples | Priority | Data in Repository |
|--------------------------------------------------------------------------|---------------|----------|---------------------|----------|--------------------|
| [ComposableSecurity/SCSVS](https://github.com/ComposableSecurity/SCSVS)  | Checklist     | Bug, Dev | No                  | 10       | Yes                |
| [sirhashalot/SCV-List](https://github.com/sirhashalot/SCV-List)          | Attack Vectors| Bug      | Yes                 | 7        | Yes                |
| [transmissions11/solcurity](https://github.com/transmissions11/solcurity)| Checklist     | Dev, Bug | No                  | 5        | Yes                |
| [sigp/solidity-security-blog](https://github.com/sigp/solidity-security-blog) | Attack Vectors| Bug | Yes | 5 | Yes                |
| [harendra-shakya/smart-contract-attack-vectors](https://github.com/harendra-shakya/smart-contract-attack-vectors) | Attack Vectors | Bug | No | 5 | No            |
| [ZhangZhuoSJTU/Web3Bugs](https://github.com/ZhangZhuoSJTU/Web3Bugs/blob/main/results/bugs.csv) | Checklist | Dev, Bug | Yes | 5 | No              |
| [security-pitfalls-and-best-practices-201](https://secureum.substack.com/p/security-pitfalls-and-best-practices-201?s=r) | Checklist | Dev, Bug | No | 5 | Yes             |
| [sayan011/Immunefi-bug-bounty-writeups-list](https://github.com/sayan011/Immunefi-bug-bounty-writeups-list) | Attack Vectors | Bug | Yes | 4 | No              |
| [security-pitfalls-and-best-practices-101](https://secureum.substack.com/p/security-pitfalls-and-best-practices-101?s=r) | Checklist | Dev, Bug | No | 4 | Yes             |
| [Decurity/audit-checklists](https://github.com/Decurity/audit-checklists) | Checklist | Bug | No | 1 | Yes              |
| [defi-slippage-attacks](https://dacian.me/defi-slippage-attacks) | Attack Vectors | Dev, Bug | Yes | 1 | Yes           |
| [Smart-Contract-Auditor-Tools-and-Techniques](https://github.com/shanzson/Smart-Contract-Auditor-Tools-and-Techniques#smart-contract-security-techniques-and-best-practices-refer-defivulnlabs-) | Database | Dev, Bug | Yes | 1 | No              |


Lists are incomplete in scope. Meaning, those don't describe all of the SCV or all of the possible attacks. Priority was put on checklist that LLM can deduce from, so multi-transaction attack like Flash Loans or very specific rounding errors are basically left uncovered.

Might be useful to some, seeing how such lists were very hot topic in 2020-2022 cycle and are (most likely) re-used in 2024 by so-called AI-audit projects. In the end, I had a fairly complex pipeline for generating prompt-based scans, but it simply wasn't delivering any worthwhile result. That being said, I do think it's fully doable to automate a large part of security reviews maybe even utilizing list as below in the process, but for today - there's something still missing, my suspicion is that generalized LLMs are not the best LLM for the job. 

And here's the detailed list used to GPT-audit:

| Description | Level | Type |
|-------------|-------|------|
| Verify that access control checks are consistently applied across all sensitive functions. | Contract-lvl | Access Control |
| Verify that function has correct visibility preventing unintended unauthorized access | Function-lvl | Access Control |
| Verify that changes to access control settings are properly authenticated | Contract-lvl | Access Control |
| Verify that access control is not missing in the function if appropriate | Function-lvl | Access Control |
| Verify that extreme input values would appropriately influence the logic flow in the contract | Function-lvl | Arithmetic |
| Verify that function multiplies before dividing | Function-lvl | Arithmetic |
| Verify that arithmetic operations are protected against integer overflow and underflow vulnerabilities | Function-lvl | Arithmetic |
| Verify that functions correctly handle edge cases for zero or extremely large numerical inputs | Function-lvl | Arithmetic |
| Verify the use of fixed-point arithmetic to mitigate risks associated with floating-point precision errors | Function-lvl | Arithmetic |
| Verify that division operations within the function include checks against division by zero | Function-lvl | Arithmetic |
| Verify that the implementation of percentage calculations is accurate and free from precision loss | Function-lvl | Arithmetic |
| Verify that array accesses are within bounds to prevent runtime exceptions. | Function-lvl | Array |
| Verify that deleting an element from a dynamic array by swapping with the last element properly removes the last element and the array size is adjusted accordingly | Function-lvl | Array |
| Verify that using the delete keyword to remove an element from a dynamic array results in space reclamation and proper array size adjustment | Function-lvl | Array |
| Verify that nested loops iterating over arrays do not contain premature exit statements that could disrupt data addition | Function-lvl | Array |
| Verify that there are no off-by one errors, especially with >, <, >=, <= and length – 1 | Function-lvl | Bug |
| Verify that loop termination conditions prevent off-by-one errors. | Function-lvl | Bug |
| Verify correct initialization and incrementation of counters in loops to prevent logic errors. | Function-lvl | Bug |
| Verify correctness of updating and accessing values in loops to prevent logic errors | Function-lvl | Bug |
| Verify the robustness of calculations against potential external contracts manipulation | Function-lvl | Calculation |
| Verify that mathematical operations account for the fixed precision of Solidity to prevent rounding errors | Function-lvl | Calculation |
| Verify that return values of mathematical operations are within expected range to prevent logic errors | Function-lvl | Calculation |
| Verify that compound arithmetic operations (like a*b/c) are correctly ordered to prevent precision loss | Function-lvl | Calculation |
| Verify that calculations involving token amounts are not susceptible to rounding errors leading to token loss or gain | Function-lvl | Calculation |
| Verify that mathematical operations are not influenced by user-controlled variables leading to unexpected results | Function-lvl | Calculation |
| Verify that calculations involving time or block numbers handle edge cases correctly | Function-lvl | Calculation |
| Verify functional symmetry of implemented functions, such as corresponding deposit and withdraw or mint and burn effects | Contract-lvl | Logic |
| Verify that calling a function once with value XY would give a same result as calling it X times with value Y | Function-lvl | Logic |
| Verify that logical operators (`==`, `!=`, `&&`, `\|`, `!`) are used correctly especially to prevent off-by-one errors | Function-lvl | Logic |
| Verify that expressions passed to logical/comparison operators (`&&`, `\|`, `>=`, `==`, etc) do not have unintended side-effects | Function-lvl | Logic |
| Verify that external contract calls returned data can’t be manipulated | Function-lvl | Logic |
| Verify that function designed behavior can’t be manipulated by arbitrary input values | Function-lvl | Logic |
| Verify that the business logic flows of smart contract proceed in a sequential step order, and it is not possible to skip any part of it or to do it in a different order than designed | Function-lvl & Contract-lvl | Logic |
| Verify simplicity and clarity in modifier logic, avoiding untrusted external contract calls | Modifier-lvl | Logic |
| Verify that logical conditions in loops and if-statements accurately account for boundary cases to avoid off-by-one errors | Function-lvl | Logic |
| Verify that function inputs are thoroughly validated to prevent manipulation of business logic | Function-lvl | Logic |
| Verify that all edge cases are handled in conditional statements to prevent logic bypass | Function-lvl | Logic |
| Verify that the function does not improperly trust user input for critical logic decisions | Function-lvl | Logic |
| Verify that array indexes and loop bounds are correctly calculated to avoid off-by-one errors | Function-lvl | Logic |
| Verify that functions handling external data use proper validation to prevent logic manipulation | Function-lvl | Logic |
| Verify that conditional checks do not rely on assumptions about external contract states or returns | Function-lvl | Logic |
| Verify that all return paths in a function maintain the intended logic flow and data integrity | Function-lvl | Logic |
| Verify that complex logical expressions do not have unintended side effects or bypass checks | Function-lvl | Logic |
| Verify that contract state changes are consistent with business logic across all functions | Contract-lvl | Logic |
| Verify that function does not permit unexpected behavior when receiving extreme or boundary input values | Function-lvl | Logic |
| Verify that fallback functions are not susceptible to logic manipulation or unexpected behavior | Contract-lvl | Logic |
| Verify that function has reentrancy guards in place to prevent recursive calls | Function-lvl | Reentrancy |
| Verify that function handles state changes before external calls to prevent reentrancy attacks | Function-lvl | Reentrancy |
| Verify the use of mutexes or equivalent mechanisms to prevent reentrancy in functions where external calls are made | Function-lvl | Reentrancy |
