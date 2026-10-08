---
name: market-entry-requirements
description: Maps the regulatory requirements for placing a product on a given market (EU, GB, US, Turkey, Canada, Brazil and others) across chemicals, cosmetics, food, pesticides, biocides, textiles, pharma and devices using the RegAffairs AI tools, and returns a cited checklist. Use when the user asks what they need to sell, import or launch a product in a country.
---

# Market-entry requirements

Produce a short, cited checklist of what must be true before a product can be placed on one market.

## Steps

1. **Pin down the product and market.** Get the product type, the target country, the user's role (manufacturer, importer, distributor, Only Representative) and the key ingredients or substances with CAS numbers. Ask for missing items in one message.
2. **Confirm coverage.** Call `regaffairs_list_covered_regulations` with the industry and `jurisdictions` to see which registers are held for that market. Tell the user if a key register is not covered.
3. **Check the ingredients with the matching industry tool:**
   - Industrial chemicals and mixtures: `regaffairs_check_chemical_substance`, then `regaffairs_get_ghs_hazard_classification` for labelling.
   - Cosmetics: `regaffairs_check_cosmetic_ingredient` with `product_type` and `application`.
   - Food: `regaffairs_check_food_additive_or_ingredient` and `regaffairs_check_food_label_and_allergens`.
   - Plant protection products: `regaffairs_check_pesticide_approval_and_mrl`.
   - Biocides and disinfectants: `regaffairs_check_biocide_active_substance` with the product type (PT1 to PT22).
   - Textiles, footwear, leather: `regaffairs_check_textile_restricted_substances`.
   - Medicines and devices: `regaffairs_check_drug_or_device_approval`, and `regaffairs_find_pharma_gmp_and_ich_guidance` for GMP.
4. **Get the obligations, not just the lists.** For registration, notification, labelling or responsible-person duties, use `regaffairs_search_regulations` to pull the article text (for example REACH Article 6 registration, Cosmetics Regulation Article 13 CPNP notification, KKDIK registration). If the question spans several regimes or jurisdictions, use `regaffairs_ask_question` once and tell the user it counts as one question on their plan. If it returns `status: "running"`, poll `regaffairs_get_answer_status` with the `answer_id`.
5. **Write the checklist** grouped as: Registration or authorisation, Ingredient restrictions, Labelling and documents, Notifications, Who is responsible. One line per item, each with its legal reference and source link. Mark each item "required", "required if" (state the condition) or "check" (data not conclusive).

## Rules

- Only list obligations the tools returned or the article text supports. If something is out of coverage, say so rather than filling the gap from memory.
- Clinical trial design and FDA submission strategy are out of scope.
- End with: "Research, not legal advice. Check the linked sources before acting."
