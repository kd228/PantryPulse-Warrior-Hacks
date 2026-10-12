# PantryPulse

**Same food, right place, in time.** PantryPulse matches surplus food from grocers, restaurants and school cafeterias to the food pantries that need it most, ranking pantries by need, distance and safety so good food reaches families before it spoils.

Built for **WarriorHacks 2.0** (Ideathon track). Theme: solve an issue in your community.

## The problem

USDA estimates that 30–40% of the US food supply is wasted. At the same time, pantries run short, often of fresh produce and dairy, the foods that spoil fastest. Donors want to give but cannot spend hours calling around, and pantries often receive what was donated instead of what they need.

## How it works

1. **Post.** A donor logs the food type, amount in pounds, and how many hours it stays safe.
2. **Match.** Pantries are ruled out if they:
   - lack cold storage when the food needs it,
   - cannot be reached before the food is unsafe (drive time at 25 mph plus 30 minutes handling),
   - close before a driver could arrive, or
   - have no space left.
3. **Score.** Each remaining pantry gets a score out of 100:

   `S = 0.40·Need + 0.25·Distance + 0.20·TimeToSpare + 0.15·Space`

   - **Need**: the pantry's reported need (0–100) for that food type
   - **Distance**: `100 · max(0, 1 − miles / 15)`
   - **Time to spare**: `100 · clamp(slack_minutes / 120, 0, 1)`, where slack is the safe window minus drive and handling time
   - **Space**: share of the load the pantry has room for
4. **Dispatch.** The best pantry is filled first and the rest goes to the next. Large donations split across pantries.

Meals are counted as pounds ÷ 1.2, the convention Feeding America uses.

## Demo features

- Ranked matches with the reasoning behind every score, plus the pantries ruled out and why
- To-scale route map (distance from the donor is to scale, direction is illustrative)
- Priority modes: Balanced, Neediest first, Fastest delivery
- Scenario buttons (grocery produce, restaurant surplus, bakery closing, cafeteria dairy)
- Live need board: click a cell to flag a shortage and watch the ranking update
- Dispatch log, impact counters, and a copyable pickup plan
- **Decision assistant**
  - *Quick questions* (why is #1 first, why were pantries ruled out, should I split it, what would change the top pick, compare priorities) are answered instantly from the matching engine.
  - *Typed questions* go to an AI assistant that sees the current donation and rankings. It advises and the coordinator decides. It never rules on food safety.

## Run it

It is a single static file with no build step.

- **Locally:** open `index.html` in a browser (Google Fonts load when online).
- **GitHub Pages:** Settings → Pages → deploy from branch `main`, folder `/ (root)`.

**Note on the assistant:** typed questions are available when the page is opened inside Claude. Anywhere else, such as GitHub Pages or a local file, the typed box is hidden and the page shows a beta notice ("under beta and will be updated soon"). The quick questions work everywhere because they run entirely in the page.

## Data

All pantries, donors, distances and need levels in the demo are **sample data** invented for illustration. A real pilot would use partner pantries and live need reports.

## Repository layout

| Path | What it is |
| --- | --- |
| `index.html` | The working prototype (HTML, CSS and JavaScript in one file) |
| `docs/pitch-deck-outline.md` | Slide-by-slide outline of the 12-slide pitch deck |
| `docs/video-script.md` | Timed 2:50 presentation script |

## Roadmap

- **Weeks 1–6, pilot:** three pantries and five donors, with a web form and text alerts
- **Weeks 7–14, routing:** volunteer drivers, batched routes and live inventory from pantries
- **Month 4 on, county scale:** every pantry and school cafeteria, with a shared public dashboard

## What we would measure

Pounds rerouted, meals provided, time from post to confirmed pickup, and how often food goes to a pantry that reported high need.

## Risks and how we handle them

| Risk | Plan |
| --- | --- |
| Food safety | Safety filters run first. Partners confirm local rules. |
| Stale need data | One-tap updates. Levels expire after 24 hours. |
| Adoption | Web form and text alerts. Nothing to install. |
| Privacy | Only donor and pantry details. Never family data. |
| Assistant errors | Sees only on-screen data. Never rules on food safety. |

## Sources

- [USDA, Food Loss and Waste FAQ](https://www.usda.gov/foodwaste/faqs)
- [Feeding America, About Our Claims](https://feedingamerica.org/ways-to-give/faq/about-our-claims)

## License

MIT, see `LICENSE`.
