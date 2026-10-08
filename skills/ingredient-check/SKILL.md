---
name: ingredient-check
description: Checks whether a food additive or a cosmetic ingredient is permitted, restricted or banned in the EU, GB, US and other markets, with maximum levels, product-type conditions and label warnings, using the RegAffairs AI tools. Use when the user asks about an E-number, a colour additive, a novel food, an INCI ingredient, a preservative or UV filter, or reviews a recipe or formula.
---

# Food additive or cosmetic ingredient check

Give the status and conditions of each ingredient for the product and market the user cares about, with sources.

## Steps

1. **Identify the regime.** Food (additive, colour, novel food, supplement, food contact, contaminant) or cosmetic. If a formula or recipe is pasted, list the ingredients and check each one.
2. **Food ingredients:** call `regaffairs_check_food_additive_or_ingredient` with `ingredient` (name, E-number, INS number, FD&C name or CAS), `jurisdictions`, and `food_category` when the user named the food (for example "flavoured drinks" or EU category 14.1.4). Use `regime` to narrow when clear. For label, allergen or claim questions, call `regaffairs_check_food_label_and_allergens` with the matching `topic`.
3. **Cosmetic ingredients:** call `regaffairs_check_cosmetic_ingredient` with `ingredient` (INCI, common name or CAS), `application` (`leave_on` or `rinse_off`), `product_type` (for example "face cream", "hair dye", "oral care") and `for_children_under_3` if relevant. Use `function` for preservatives, UV filters, colourants and fragrance allergens to narrow to the right annex.
4. **Report per ingredient:** Status (permitted, restricted, banned, not listed), Annex or category entry, Maximum level and unit, Conditions and product types, Required label warning, Source link. For a formula, give one table and put any blocker at the top.
5. **Flag recent or pending changes** the tools return, such as a new entry, a re-evaluation or an SCCS or EFSA opinion, with their dates.

## Rules

- Levels are exact: copy the number, unit and basis (for example "0.5% in the ready-for-use preparation") from the source.
- "Not listed" in a positive list (for example EU food additives or cosmetic preservatives) means not permitted for that function. Say so explicitly.
- Do not use these tools for medicines or general chemical restrictions; switch to the pharma or chemical tools.
- End with: "Research, not legal advice. Check the linked sources before acting."
