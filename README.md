![RegAffairs AI](assets/regaffairs-icon-512.png)

# RegAffairs AI plugin for Claude

Cited answers on chemical, food, pharma, device, cosmetic, pesticide and biocide rules, inside Claude.

This plugin connects Claude to the [RegAffairs AI](https://regaffairsai.com) MCP server and adds five skills that teach Claude real regulatory affairs workflows. Every result links to the source document it came from, so each claim can be checked.

Built for regulatory affairs, product stewardship, SDS authoring, REACH Only Representatives and regulatory consultancies.

## What you get

**Connector:** the RegAffairs AI MCP server at `https://mcp.regaffairsai.com/mcp`. This is the same server as the RegAffairs AI connector in the Claude directory, so you see one set of tools. It covers more than 1,400 regulator datasets in 49 jurisdictions: ECHA REACH and CLP lists, UK REACH, US TSCA, California Prop 65, Turkey KKDIK, EU and US food law, EU cosmetics annexes, pesticide approvals and MRLs, biocides, EU GMP and ICH, FDA and EMA drug data, the EU MDR and IVDR, device registers and textile restricted substance lists.

**Skills:**

| Skill | What it does |
| --- | --- |
| `substance-regulatory-screen` | Screens a list of CAS or EC numbers, or a bill of materials, against SVHC, REACH Annex XIV and XVII, UK REACH, TSCA, Prop 65 and KKDIK, and returns one cited status table with actions. |
| `sds-section-2-15-check` | Checks SDS section 2 (classification, H-statements, pictograms, signal word) and section 15 (regulatory listings) against the CLP Annex VI entry and REACH lists, including pending CLH changes. |
| `market-entry-requirements` | Builds a cited checklist of what a product needs before it is sold in a given market: registrations, ingredient limits, labels, notifications and who is responsible. |
| `verify-regulatory-claim` | Checks a statement from a supplier, customer, article or AI answer against the official text and gives a verdict with the exact passage. |
| `ingredient-check` | Checks food additives (E-numbers, colours, novel foods, food contact) and cosmetic ingredients (INCI, annex entries, maximum levels, warnings) per market and product type. |

## Install

In Claude, find **RegAffairs AI** in the plugin directory and install it. Then open the plugin's Connectors tab, connect RegAffairs AI and sign in with your RegAffairs AI account.

In Claude Code, after the plugin is installed, run `/mcp` and authenticate the `regaffairs` server.

New to RegAffairs AI? Create a free account at [regaffairsai.com](https://regaffairsai.com). Setup guide: [regaffairsai.com/docs/mcp](https://regaffairsai.com/docs/mcp).

## Example prompts

- Screen these for REACH, SVHC and TSCA status: 80-05-7, 117-81-7, 50-00-0, 1333-86-4.
- Here is section 2 and 15 of our SDS for formaldehyde. Is it current with CLP and REACH?
- What do I need to sell a leave-on face cream with salicylic acid in the EU and GB?
- A supplier says titanium dioxide (E171) is still allowed in EU food. Is that true?
- Can I use phenoxyethanol at 1% in a rinse-off shampoo in the EU?

## What this plugin runs, sends and fetches

- It runs no local code. It has no scripts, hooks, commands or binaries.
- It contains Markdown skills and one MCP server reference (`.mcp.json`) of type `http` pointing at `https://mcp.regaffairsai.com/mcp`.
- When Claude calls a RegAffairs AI tool, the tool arguments (for example substance names, CAS numbers, ingredient names or your question) are sent to that server over HTTPS, authenticated with OAuth 2.1 against `https://regaffairsai.com`. The server returns regulatory data with links to stored source documents on regaffairsai.com.
- The lookup tools are read-only. The `regaffairs_ask_question` tool runs the RegAffairs AI research agent, saves the question and answer to your RegAffairs AI history and counts as one question on your plan.
- The plugin stores no credentials. Claude manages the OAuth tokens.

Privacy policy: [regaffairsai.com/privacypolicy](https://regaffairsai.com/privacypolicy). Terms: [regaffairsai.com/termsofservice](https://regaffairsai.com/termsofservice).

## Support

Email [hello@regaffairsai.com](mailto:hello@regaffairsai.com) or use [regaffairsai.com/contact](https://regaffairsai.com/contact).

Answers are research, not legal advice. Check the linked sources before acting.

## License

MIT. See [LICENSE](LICENSE).
