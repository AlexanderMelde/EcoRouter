# Competitors and Differentiation

Checked 2026-11-23 against official pricing and docs pages. Prices are for Linux 2 vCPU x64.

## What Each Competitor Offers

| Provider                | $/min                | Region per job?              | Carbon-aware?             | The one thing only they do                              |
| ----------------------- | -------------------- | ---------------------------- | ------------------------- | ------------------------------------------------------- |
| GitHub-hosted           | 0.006¹               | No                           | No                        | Zero setup, native default                              |
| Blacksmith              | 0.004²               | No (set per org)             | No                        | **Flex scheduling:** wait ≤ 24 h for 40% off³            |
| Namespace               | 0.002–0.003⁴         | No                           | No                        | Persistent cache volumes, 15 s billing⁴                 |
| Depot                   | 0.006⁵               | US/EU                        | No                        | Per-second tracking, no per-job minimum⁵ ⁶; fast cache  |
| WarpBuild               | 0.004 (BYOC 0.002)⁷  | Enterprise only              | No                        | SSH into failed jobs                                    |
| Ubicloud                | 0.00125–0.002⁸       | No (DE by default)           | No                        | Open source (AGPL), cheapest managed EU runners         |
| RunsOn                  | AWS cost + licence   | **Yes** (`region=` label)    | No                        | Per-job AWS region in your own account, spot by default |
| Cirun                   | Your cloud bill      | Per runner config            | No                        | 8 clouds + on-prem                                      |
| Self-hosted / Actuated  | ~0 marginal          | Your sites                   | No                        | Your hardware, your energy                              |
| Carbon-Aware Dispatcher | –                    | –                            | Time only                 | Delays a job until the grid is green                    |
| Eco-CI                  | –                    | –                            | Measures only             | Estimates the energy of each run                        |
| runner-fallback-action  | –                    | –                            | No                        | Self-hosted runner if online, else GitHub               |
| ~~CarbonRunner~~        | "25% < GitHub"       | Automatic                    | **Location**              | Live carbon routing across clouds. **Shut down 2026**⁹  |

No runner provider makes a renewable-energy or carbon claim on its pricing or docs pages.

## Why CarbonRunner Failed and What We Change

CarbonRunner's closure notice gave its reasons as "Nobody cares" (money beats carbon) and "The cost of switching is too high" (a new vendor means due diligence, testing and team buy-in).¹⁰

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
| Sweden  | 21.3¹¹     | AWS eu-north-1 (via RunsOn)        |
| France  | 32.0¹²     | AWS eu-west-3                      |
| Finland | 66.4¹³     | Hetzner HEL                        |
| Germany | 335.4¹⁴    | Frankfurt, Falkenstein (most EU runners) |

- **Location beats time.** Sweden vs. Germany is a ~16× difference (335.4 / 21.3 ≈ 15.7).
- **Germany's daily profile** swings between ~239 and ~387 g,¹⁴ so time-shifting still helps for German sites and solar.
- **Rank by the grid (location-based) only.** The Green Software Foundation's SCI specification excludes market-based claims (RECs, PPAs) from grid intensity, so "100% renewable" marketing never changes the score.¹⁵

## Sources

¹ [GitHub Docs - Actions runner pricing](https://docs.github.com/en/billing/reference/actions-runner-pricing)  
² [Blacksmith - Pricing](https://www.blacksmith.sh/pricing)  
³ [Blacksmith Docs - Runners overview (Flex)](https://docs.blacksmith.sh/blacksmith-runners/overview)  
⁴ [Namespace - Pricing](https://namespace.so/pricing)  
⁵ [Depot - Pricing](https://depot.dev/pricing)  
⁶ [Depot Docs - GitHub Actions overview](https://depot.dev/docs/github-actions/overview)  
⁷ [WarpBuild - Pricing](https://www.warpbuild.com/pricing)  
⁸ [Ubicloud Docs - Pricing](https://www.ubicloud.com/docs/about/pricing)  
⁹ [CarbonRunner on GitHub](https://github.com/carbonrunner) (org marked no longer maintained; community repo archived 15 Apr 2026)  
¹⁰ [Fershad Irani - Nobody cares](https://fershad-com.pages.dev/writing/nobody-cares/)  
¹¹ [Electricity Maps - Grid in Review 2025: Sweden](https://www.electricitymaps.com/grid-in-review-2025/sweden)  
¹² [Electricity Maps - Grid in Review 2025: France](https://www.electricitymaps.com/grid-in-review-2025/france)  
¹³ [Electricity Maps - Grid in Review 2025: Finland](https://www.electricitymaps.com/grid-in-review-2025/finland)  
¹⁴ [Electricity Maps - Grid in Review 2025: Germany](https://www.electricitymaps.com/grid-in-review-2025/germany)  
¹⁵ [Green Software Foundation - SCI Specification](https://github.com/Green-Software-Foundation/sci/blob/main/SPEC.md)
