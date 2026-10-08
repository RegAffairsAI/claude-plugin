---
name: sds-section-2-15-check
description: Checks Safety Data Sheet sections 2 (hazard identification) and 15 (regulatory information) for a substance against the CLP Annex VI harmonised classification and REACH lists using the RegAffairs AI tools. Use when the user shares an SDS, an SDS extract or a substance and asks whether the classification, label elements or regulatory listings are correct or current.
---

# SDS section 2 and 15 check

Compare what an SDS says with what the regulator currently publishes, line by line, and list the gaps with sources.

## Steps

1. **Read the SDS.** Extract from section 1 or 3 the substance name, CAS and EC. From section 2 take the classification, H-statements, pictograms, signal word and P-statements. From section 15 take every regulation and list it mentions. Note the SDS revision date. If the SDS covers a mixture, say that mixture classification is the user's own calculation and check each listed component instead.
2. **Get the harmonised entry.** Call `regaffairs_get_ghs_hazard_classification` with the CAS and `include_pending: true` so open CLH intentions and RAC opinions show up. Add `jurisdictions: ["EU","GB"]` if the SDS is used in Great Britain. Add `include_notified: true` only when there is no harmonised entry.
3. **Get the list status.** Call `regaffairs_check_chemical_substance` for the same substance(s) to cover SVHC, Annex XIV, Annex XVII, POPs, UK REACH and any other markets the SDS targets.
4. **Exposure limits if section 8 is in scope.** If the user asks, call `regaffairs_get_workplace_exposure_limits` with the countries the SDS is sold in.
5. **Report the gaps** as a table: Item, SDS says, Source says, Status (match, missing, outdated, wrong), Source link. Cover:
   - Section 2: every hazard class and category, H-statement, pictogram, signal word, specific concentration limits and M-factors. A harmonised classification is binding for the hazard classes it covers; self-classification is still needed for hazard classes it does not cover.
   - Section 15: SVHC listing and date, Annex XIV entry and sunset date, Annex XVII entry number, and any national lists the tools returned.
   - Pending changes: an open CLH intention or RAC opinion means the next ATP may change section 2. Name it and its date.
6. **Give the fix list**, one line per change, in the order a stewardship team would apply it.

## Rules

- Quote the ATP or list version the tool returned. Never cite a classification from memory.
- Do not rewrite the whole SDS unless asked. Flag, explain, cite.
- End with: "Research, not legal advice. Check the linked sources before acting."
