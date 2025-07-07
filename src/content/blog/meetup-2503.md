---
title: "Meetup2503- Dive, the opensource MCP host application"
pubDatetime: 2025-03-29
tags:
  - meetup
  - dive
  - mcp
  
description: "- Dive: OpenAgentPlatform Biggo 高雄在地最強新創 最新open source AI Agent 平台
   - MCP： model context protocpl
- aichat: all-in-one cli ai tools
- Iroh P2P network library
   - umbpipe - p2p unix sockect
   - zeco - zellij session sharing helper
- Steno keyboard - 22-key keyboard"
youtube: YvnpwDVrJ2k

---

[![hackmd-github-sync-badge](https://hackmd.io/9LzIkboFTManbksZzdDGfA/badge)](https://hackmd.io/9LzIkboFTManbksZzdDGfA)

共筆歡迎編輯
Please feel free to help us update the content.

- tyson's note https://www.facebook.com/share/142AP36xo3/

# MCP

## mcp

- 技術難點: llm 的token window limitation
  - 裝100個工具, 打個hi, 就要送100 次，所以裝越多， token 會成等比級數消耗
  - Content Window 不只是 MCP 的技術難點. 也是 RAG 的. 也是 ChatHistory 的. 講起來LLM不論走那個方向都有這個難題.

## two types of MCP connections:

### MCP-stdio

### MCP-SSE - server-sent events

- MCP 目前沒有 auth跟 authorization, 也沒有 package management 。credentials 是pre-configured

# Dive

- AI PC 支援比android 支援更有機會，因爲AI PC 沒有應用。

- 引述一兩個 Reddit 上有人說：實際上基本上沒人在乎MCP，- Google 基本上不在乎MCP (反串要註明)
- 即便 Microsoft Windows 不做。電子五哥最後也可能透過ISV方式做pre-load 把MCP 做進去, 就像Ubuntu Linux 一樣
- MCP host 只需要一個, 不會裝很多個. 是 MCP 的 Tools 裝的越多. tools desc 就越多, 就算沒有呼叫到的工具, token用量也會倍增.

## Why do Dive

- Dive 有 6000 使用者。
- Dive 開源跨平台，透過很多MCP host, 能做到比perplexity 更好的體驗，例如直接用playwright 開browser 找資料
- 不缺資金，但在台灣人很難找人做Dive。
- 現在做AI 跟以前做search engine 在做的事很像
- 越來越少人用 Google (so am I)

## Dive backend (aka, Dived)

## quick thought

- Tyson - 要支援 human in the loop 的 EUDI wallet auth/authorization 要改 dived 。

## Monthly Tip sharing

### steno keyboard

### aichat

### zeco: zellij session sharing helper
