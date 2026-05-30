# Macro Lab

Free, no-signup calorie + macro calculator that generates a personalized 5/7/14-day meal plan with full nutrition, per-aisle grocery list, and 13 cuisines. Static HTML/CSS/JS in a single file — no backend, no LLM, no tracking.

**Live:** https://sumitdahiya125.github.io/macro-lab/

## What it does

1. Takes your stats (age, sex, height, weight, activity, optional body-fat %) and goal (cut / maintain / bulk).
2. Computes a daily calorie target via **Mifflin–St Jeor** (or **Katch-McArdle** when body-fat % is given) × activity factor ± goal adjustment.
3. Splits calories into protein / carbs / fat (protein scales with bodyweight; fat = 25% of cals; carbs = remainder), and estimates a daily water target.
4. Generates a meal plan filtered by **cuisine**, **diet**, **health filter**, and **allergens**, with full ingredient lists, household measures, and per-meal macros.
5. Computes **fiber, saturated fat, sodium, and sugar** for every meal from an ingredient nutrition table, and shows a daily nutrition panel vs. targets.
6. Aggregates a shopping list grouped by store aisle, with real summed quantities (grams / ml / pieces, auto-promoting to kg / L).

## Filters

- **Cuisine (13)** — Western/International · Italian · Mexican/Latin · Greek & Mediterranean · Middle Eastern · Indian (Mix / North / South) · Thai · Chinese · Japanese · Korean · Vietnamese
- **Diet** — No restrictions · Pescatarian · Eggetarian (veg + eggs) · Vegetarian (strict, no eggs) · Vegan
- **Health filter** — Healthy only (default) · Show all · Diabetes-friendly · Heart-friendly · High-protein only
- **Allergens** — Gluten · Dairy · Tree nuts & peanuts · Soy · Egg · Fish · Shellfish · Sesame
- **Plan length** — 5 / 7 / 14 days

## Meal database

- **249 meals** across breakfast / lunch / dinner / snack, spanning 13 cuisines.
- Each meal carries: name, diet eligibility, cuisine, ingredient list (metric weights **and** household measures — cups / tbsp / katoris / palm-sized for meat / cooked-equivalent for grains), allergen tags, and iron/calcium flags.
- **307 unique ingredients**, every one in the nutrition table → macros for 100% of meals are computed, not hand-typed.
- Indulgent meals (deep-fried or cream/butter-heavy) are flagged and hidden under the default "Healthy only" filter.
- Egg-containing meals are auto-detected and demoted out of the strict-vegetarian bucket (Indian-style vegetarian excludes eggs).

## Accuracy

- Per-100g nutrition table covers calories, protein, carbs, fat **plus** fiber, saturated fat, sodium, and sugar.
- Meal totals are computed from `ingredient grams × per-100g values`, parsed from each quantity string ("80g · 1 cup", "1 medium (~120g)", "½ medium", "250ml", "3 (~30g each)", etc.) with a per-piece weight fallback.
- The daily nutrition panel shows plan averages vs. evidence-based targets: fiber (~14g/1000kcal), saturated fat (<10% of cals), sodium (<2,300mg/day).

## How it picks meals (no AI involved)

For each slot in the plan:

1. Filter the database by `diet ∩ cuisine ∩ healthOk ∩ allergens`, excluding hidden meals.
2. If the pool is thinner than 3 meals: tier-1 fallback relaxes cuisine, tier-2 relaxes the health filter.
3. Sort by absolute distance from the slot's calorie target (breakfast 25% / lunch 30% / dinner 30% / snack 15%).
4. Take the top 5 closest as the "acceptable" pool.
5. Prefer favorited meals, then meals not yet used this week.
6. Random pick from what remains.

## Features

- **Per-meal swap** — re-pick a single meal without regenerating the week.
- **Drag-and-drop** — drag a meal to the same slot on another day to swap.
- **Favorite / hide** meals (persisted) — generator prefers favorites, never serves hidden ones.
- **Dark mode** (auto-detects system preference), **localStorage persistence**, **share-via-URL**, **reset**, and a **print-friendly** stylesheet + Print button.
- Collapsible day cards with one-line summaries; sticky section nav (Targets · Plan · Grocery).

## Honest disclaimer

- BMR formulas are well-established but still estimates.
- Nutrition values are computed from an ingredient table of USDA-style averages — accurate to roughly ±10–15% depending on portion, cut, and cooking method.
- Health-filter presets (Diabetes / Heart) are informational only, not medical advice. Consult a registered dietitian for clinical conditions.

## Tech

- Single `index.html` with inline CSS and JS. Inter font from Google Fonts.
- No build step, no npm, no backend, no tracking.

## Run locally

```sh
python3 -m http.server 8765
# then open http://localhost:8765
```

Or just open `index.html` in a browser.

## Repo layout

```
.
├── index.html      # the entire app
└── README.md       # this file
```
