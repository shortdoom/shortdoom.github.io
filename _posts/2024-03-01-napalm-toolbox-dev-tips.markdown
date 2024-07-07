---
layout: post
title: "napalm-toolbox dev notes"
subtitle: "info dump on using napalm-toolbox"
date: 2024-03-01 21:52:16 +0100
categories: tools
---

Some notes on working with [Napalm](https://github.com/ConsenSysDiligence/napalm/)

it's important to understand difference between collection and workflow. collections are rules to execute. workflows are set of collections to execute together.

install additional packages: `napalm-slither` (for slither/detectors collection) and `napalm-core` (for 3 base workflows initialized with different rules)

`napalm collections list`

`napalm collections show napalm-core/detectors` - show rules in collection (requires installed `napalm-core` package)

`napalm workflows list` (notice workflow**s** different from `workflow`)

with installed `napalm-core` package you should have 3 workflows initialized, however those seem empty, use

`napalm workflows create test` - if you want to create a new (empty) workflow

`napalm workflow test add slither/detectors` - notice `workflow` not workflow**s**.

then you can run it on target contract, best to do it from contracts directory. for better chances of hassle free compilation pre-install solc versions you will be using with `solc-select install <ver>` and add `slither.config.json` file if remapping or other evm related data is required.

run with:

`napam run test <targer.sol>` - where `test` is name of the workflow specified earlier