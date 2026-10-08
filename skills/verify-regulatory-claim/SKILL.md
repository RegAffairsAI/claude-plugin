---
name: verify-regulatory-claim
description: Verifies a regulatory statement (from a supplier, a customer, a colleague, a news article or an AI answer) against the official source using the RegAffairs AI tools, and returns a verdict with the exact source passage. Use when the user asks "is this true", "is this still current", or wants a claim about a substance, ingredient, limit or deadline checked.
---

# Verify a regulatory claim

Check one claim against the regulator's own text and say plainly whether it holds.

## Steps

1. **Split the claim into checkable facts.** For example, "BPA is banned in food contact materials in the EU since 2024" holds three facts: the substance, the scope (food contact), the date. Write them out before checking.
2. **Use the most specific tool for each fact:**
   - List status of a chemical: `regaffairs_check_chemical_substance`.
   - Classification or label elements: `regaffairs_get_ghs_hazard_classification`.
   - Exposure limit: `regaffairs_get_workplace_exposure_limits`.
   - Food, cosmetic, pesticide, biocide, textile, drug or device status: the matching `regaffairs_check_*` tool.
   - Wording of an article, annex or guideline: `regaffairs_search_regulations` with a precise query, then pass `document_id` to read more of the same document. For non-English registers, search in that language too.
3. **Read the passage, not just the hit.** Check the scope, conditions, dates and transitional periods in the source text. Many claims are wrong only in scope or date.
4. **Give the verdict** in this format:
   - **Claim:** the user's words.
   - **Verdict:** True, Partly true, Not supported, False, or Outdated.
   - **What the source says:** a short quote with the document title, date and link.
   - **What differs:** one or two lines on the gap (scope, date, threshold, jurisdiction).
5. If the tools return nothing relevant, say "Not found in RegAffairs AI data" and name what was searched. Do not mark a claim false just because no record was found.

## Rules

- Never confirm a claim from memory. The verdict must rest on a returned source.
- Use `regaffairs_ask_question` only if the claim needs reasoning across several sources, and tell the user it counts as one question on their plan.
- End with: "Research, not legal advice. Check the linked sources before acting."
