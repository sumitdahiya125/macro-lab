# Macro Lab

Free, no-signup calorie + macro calculator that also generates a 7-day meal plan and a real shopping list. Static HTML/CSS/JS — no backend, no LLM, no tracking.

**Live:** https://sumitdahiya125.github.io/macro-lab/

## What it does

1. Takes your stats (age, sex, height, weight, activity) and goal (cut / maintain / bulk).
2. Computes daily calorie target via the **Mifflin–St Jeor** BMR formula × activity factor ± goal adjustment.
3. Splits calories into protein / carbs / fat (protein scales with bodyweight; fat = 25% of cals; carbs = remainder).
4. Generates a 7-day meal plan filtered by **cuisine**, **diet**, and **health filter**, with ingredients and per-meal macros.
5. Aggregates a shopping list with real quantities (sums grams / ml / pieces, auto-promotes to kg / L past 1000).

## Filters

- **Cuisine** — Any · Western/International · Indian (Mix / North / South) · Mediterranean / Middle Eastern · Mexican / Latin · East Asian
- **Diet** — No restrictions · Pescatarian · Eggetarian (veg + eggs) · Vegetarian (no eggs) · Vegan
- **Health filter** — Healthy only (default) · Show all · Diabetes-friendly · Heart-friendly · PCOS-friendly · High protein only

## How it picks meals (no AI involved)

For each of the 28 slots in a week:

1. Filter the meal database by `diet ∩ cuisine ∩ healthOk`.
2. If the pool is thinner than 3 meals, tier-1 fallback relaxes cuisine; tier-2 fallback relaxes the health filter as last resort.
3. Sort remaining meals by absolute distance from the slot's calorie target (breakfast 25% / lunch 30% / dinner 30% / snack 15%).
4. Take the top 5 closest as the "acceptable" pool.
5. Prefer meals not yet used this week (tracked via `Set` per slot).
6. Random pick from what remains.

That's the whole algorithm. ~25 lines of JavaScript.

## Meal database

- ~125 meals across breakfast / lunch / dinner / snack.
- Each meal carries: name, diet array, cuisine, total calories + macros, and an ingredient list with both metric weights and household measures (cups / tbsp / katoris / palm-sized for meat / etc.).
- 11 meals are flagged as **indulgent** (deep fried or cream/butter heavy) and hidden under the default "Healthy only" filter — toggle to "Show all" to include them.
- Egg-containing meals are auto-detected from ingredient names and demoted out of the strict-vegetarian bucket.

## UI

- **Sticky section nav** (Targets · Plan · Grocery) at the top of results for fast jumping.
- **Collapsible day cards** with a one-line meal summary; click to expand for ingredients + per-meal macros. First day expanded by default; "Expand all" / "Collapse all" controls available.
- **Color-coded macro bar** with protein (orange) / carbs (yellow) / fat (blue) proportions of the day target.

## Honest disclaimer

- The Mifflin–St Jeor calorie formula is well-established but is still an estimate.
- Per-meal macros are hand-typed approximations (±10–15%). Making them truly accurate would require an ingredient-level nutrition table (per-100g values × quantities). That's not implemented.
- Health-filter presets (Diabetes / Heart / PCOS) are informational only and not medical advice. Consult a registered dietitian for clinical conditions.

## Tech

- Single `index.html` file with inline CSS and JS.
- Inter font from Google Fonts.
- No build step. No npm. No backend. No tracking.

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
