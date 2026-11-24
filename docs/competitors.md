# Competitors and Differentiation

Checked 2026-11-23 against official pricing and docs pages. Prices are for Linux 2 vCPU x64.

## What Each Competitor Offers

| Provider                | $/min              | Region per job?              | Carbon-aware?             | The one thing only they do                              |
| ----------------------- | ------------------ | ---------------------------- | ------------------------- | ------------------------------------------------------- |
| GitHub-hosted           | 0.006              | No                           | No                        | Zero setup, native default                              |
| Blacksmith              | 0.004              | No (set per org)             | No                        | **Flex scheduling:** wait ≤ 24 h for 40% off             |
| Namespace               | 0.002–0.003        | No                           | No                        | Persistent cache volumes, 15 s billing                  |
| Depot                   | 0.006              | US/EU                        | No                        | Per-second tracking, no per-job minimum; fast cache     |
| WarpBuild               | 0.004 (BYOC 0.002) | Enterprise only              | No                        | SSH into failed jobs                                    |
| Ubicloud                | 0.00125–0.002      | No (DE by default)           | No                        | Open source (AGPL), cheapest managed EU runners         |
| RunsOn                  | AWS cost + licence | **Yes** (`region=` label)    | No                        | Per-job AWS region in your own account, spot by default |
| Cirun                   | Your cloud bill    | Per runner config            | No                        | 8 clouds + on-prem                                      |
| Self-hosted / Actuated  | ~0 marginal        | Your sites                   | No                        | Your hardware, your energy                              |
| Carbon-Aware Dispatcher | –                  | –                            | Time only                 | Delays a job until the grid is green                    |
| Eco-CI                  | –                  | –                            | Measures only             | Estimates the energy of each run                        |
| runner-fallback-action  | –                  | –                            | No                        | Self-hosted runner if online, else GitHub               |
| ~~CarbonRunner~~        | "25% < GitHub"     | Automatic                    | **Location**              | Live carbon routing across clouds. **Shut down 2026**   |

No runner provider makes a renewable-energy or carbon claim on its pricing or docs pages.

## Why CarbonRunner Failed and What We Change

CarbonRunner's closure notice gave its reasons as "Nobody cares" (money beats carbon) and "The cost of switching is too high" (a new vendor means due diligence, testing and team buy-in).

| Their problem                     | EcoRouter's answer                                                                    |
| --------------------------------- | ------------------------------------------------------------------------------------- |
| Sold carbon                       | Sells **lower CI bills**. Carbon is the free bonus: the greenest option within a cost tolerance |
| Was a new runner vendor           | Is **not a vendor**. It routes across the runners you already have. Open source, no new contract |
| Cloud regions only, location only | Location **and** time, plus your own hardware                                         |

## What We Take From Each

| Competitor                | Leg we take                                                  | What we add                                            |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| CarbonRunner              | Live, location-based carbon routing                          | Not a vendor; open source; also time-shifts            |
| Blacksmith Flex           | "Can wait" jobs are cheaper                                  | Works across all providers, and waits for a *green* hour |
| RunsOn                    | Per-job region label                                         | Chooses the region from live grid data                 |
| Ubicloud / Namespace      | Cheapest per minute                                          | Used as cheap backends, picked when they win           |
| Carbon-Aware Dispatcher   | Time-shifting                                                | Location-shifting and a deadline fallback              |
| runner-fallback-action    | Multi-backend fallback                                       | Decides on cost, carbon and urgency, not just "online?" |
| Eco-CI                    | Energy per job                                               | Uses it to report savings per repo                     |
| GitHub-hosted             | The safe default                                             | Stays the instant lane for humans and the final fallback |

## Grid Numbers That Matter (Electricity Maps 2025 annual means)

| Zone    | gCO₂eq/kWh | Example runners                    |
| ------- | ---------- | ---------------------------------- |
| Sweden  | 21.3       | AWS eu-north-1 (via RunsOn)        |
| France  | 32         | AWS eu-west-3                      |
| Finland | 66         | Hetzner HEL                        |
| Germany | 335.4      | Frankfurt, Falkenstein (most EU runners) |

- **Location beats time.** Sweden vs. Germany is a ~16× difference.
- **Germany's daily profile** swings between ~239 and ~387 g, so time-shifting still helps for German sites and solar.
- **Rank by the grid (location-based) only.** The Green Software Foundation's SCI specification excludes market-based claims (RECs, PPAs) from grid intensity, so "100% renewable" marketing never changes the score.
