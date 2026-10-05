# Privacy policy: Verified Research

Last updated: 2026-10-06

Verified Research is a Claude plugin made of instructions, reference guides and three optional Python scripts. It has no server, no account system and no MCP connectors of its own.

## What it collects

Nothing. The author does not receive, see or store any data from people who use the plugin, and the plugin contains no analytics or telemetry.

## What it reads

- **Public web pages**, through your Claude client's built-in web search and fetch tools, to answer the questions you ask.
- **Your own connected apps** (for example Drive or email), only when you ask about your own work and only through connectors you have already set up in Claude. It does not put private details into web searches.

## What it sends, and where

- Search queries and page requests go through your Claude client's web search and fetch tools, under your Claude provider's terms.
- The optional `scripts/check_links.py` sends plain HTTP requests from your machine to the URLs cited in a report, to check that they load and contain the quoted text. It sends nothing else.

## What it stores

A Standard or Deep research run writes its working notes and the final report as local files under `research/` in your working folder. They stay on your machine (or in your session's workspace) and you can delete them at any time. Nothing is retained anywhere else by the plugin.

## Children

The plugin is not intended for users under 18.

## Contact

Questions or concerns: open an issue at https://github.com/JalalD/verified-research/issues
