---
name: substance-regulatory-screen
description: Screens a list of chemical substances (names, CAS or EC numbers, or a bill of materials) against REACH, SVHC, CLP, UK REACH, TSCA, Prop 65 and KKDIK using the RegAffairs AI tools, and returns one cited status table. Use when the user pastes several substances, a BOM or a supplier declaration and asks which are listed, restricted, banned or need action.
---

# Substance regulatory screen

Turn a list of substances into one table: identity, list status per jurisdiction, and what it means for the user, with every status linked to its source.

## Steps

1. **Collect the list.** Accept names, CAS (e.g. 80-05-7) or EC numbers (e.g. 201-245-8) from text, a table or a file. Keep the user's row order and any concentration column. Ask which markets matter only if the user has not said; otherwise check all.
2. **Resolve identity first when names are messy.** For trade names, salts, hydrates or group entries, call `regaffairs_check_chemical_substance` with `identity_only: true`. Flag any row that resolves to more than one CAS or to none, and ask before guessing.
3. **Screen in batches of up to 50.** Call `regaffairs_check_chemical_substance` with `substances` (and `jurisdictions` if the user named markets, for example `["EU","GB","US","US-CA","TR"]`).
4. **Add classification only if asked or if it changes the answer.** For CMR or SVHC hits where the user needs label or SDS impact, call `regaffairs_get_ghs_hazard_classification` per substance.
5. **Build the table.** Columns: Substance, CAS, EC, SVHC (date added), Annex XIV (sunset date), Annex XVII (entry number and condition), UK REACH, TSCA, Prop 65, KKDIK, Notes. Use "not listed" only when the tool says the substance was checked and is absent. Use "not checked" when a list was out of scope.
6. **Summarise actions** under the table, highest impact first. Examples: SVHC above 0.1% w/w in an article triggers REACH Article 33 communication and a SCIP notification; an Annex XIV substance past its sunset date needs an authorisation; an Annex XVII entry restricts the listed uses only, so quote the condition.

## Rules

- Cite every status with the source the tool returned (list name, date, link). Do not state a status the tools did not return.
- Do not use `regaffairs_ask_question` for single-substance list status. Use it only for a cross-cutting follow-up, such as "what must my SDS and SCIP notification say for these hits", and note that each call counts as one question on the user's plan.
- If a substance is a group entry (for example PFAS or lead compounds), say the listing is group-based and that membership needs the user's own check.
- End with: "Research, not legal advice. Check the linked sources before acting."
