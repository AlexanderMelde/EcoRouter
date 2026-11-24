# EcoRouter — Carbon-Aware CI Routing

**Each CI job goes to the cheapest runner that meets the jobs deadline, and to the greenest region among near-equals (configurable to your companies eco goals).**

## Motivation
Due to the increased number of AI-generated code and therefore the need for reliable quality control mechanisms, the demand for CI-jobs has increased drastically. Recent studies by GitHub¹ show, that the yearly number of GitHub Actions Jobs have increased fivefold in the last 2 years.

This has both a financial and an environmental cost.
A representative study² (of 2.2M GitHub Actions CI-runs across 18,000+ repositories) recommends two levers to meet eco goals: run CI-jobs in low-carbon regions, and schedule runs for cleaner hours.

We found no existing tool to pull either lever for you, across the runners you already use.

The cost and carbon emission savings can be huge, if the right providers are chosen:

- **Cost:** $0.006/min on GitHub, ~$0.001–0.002 on Ubicloud or Namespace, close to 0 on your own hardware. (>6x saving possible)
- **Carbon:** 335 gCO₂/kWh in Germany vs. 21 in Sweden (16x saving possible)

Furthermore, we observed that running multiple AI agents in parallel can cause compute-bottlenecks on local development workstations, as oftentimes each agent step is followed by a full CI-run quality gate.

## Our Solution

EcoRouter is a drop-in Router that picks **where and when** each CI-job runs, based on each tasks deadline and available compute:

- **Human waiting / High Priority** → the fastest runner now (default & fallback: GitHub-hosted).
- **Nobody waiting / Time-insensitive** (agents, drafts, nightly builds, ...) → the cheapest* runner that meets the deadline. Among near-equally priced options, the cleanest grid wins. Jobs may also wait for a greener hour.

We don't want just a simple "price-per-minute" comparison, but a rather complex multi-dimensional optimization. 

To give you some ideas, here are some possible rules a user might want to configure:

- "Route all jobs to my home server / local machine, unless its RAM/CPU/GPU usage is > 60% during the day, or > 80% at night. In those cases, chose the cheapest cloud-runner. Except for high-priority builds, those should always run on the fast GitHub instances"
- "When the sun shines, use my home lab, else I want you to check for the greenest regions worldwide and always use runners from a local dynamically-priced compute-provider of these regions."
- "I tag my CI jobs with speed, cost, eco and sovereignty requirement labels, for each job decide which solution wins the tradeoff."

```
              PR / push / schedule
                       │
               ┌───────▼────────┐
               │  route CI-job  │  urgency · expected duration · € · gCO₂/kWh now & forecast · cpu load
               └───────┬────────┘
     ┌──────────┬──────┴─────┬──────────────┬───────────────┐   different providers
     ▼          ▼            ▼              ▼               ▼   and their advantages
 GitHub-    Ubicloud /   RunsOn          Blacksmith      Own sites
 hosted     Namespace    eu-north-1      (Flex)          (office PV)
 instant    cheapest     21 g Sweden     cheap if wait   ~free, solar
```

The routing can be done locally or in a short-lived default/GitHub runner (e.g. via a simple docker container). The CI-job itself, or a full CI pipeline, is then executed on the server/provider determined by the router.

## Market Research

Carbon-aware cloud computing is a well-known and trending hype topic across enterprise architects and hyperscalers, proving there is a massive real market.

There are several proprietary cloud ecosystems (vendor lock-in, e.g. Google Cloud Carbon-Aware Scheduling, Azure KEDA, Cloudflare Green Compute), developer SDKs (GSF Carbon Aware SDK), or tools for retrospective measuring (CodeCarbon, Kepler). The only carbon-aware CI service we could find, CarbonRunner³, shut down in 2026. Its closure notice said "Nobody cares" (money beats carbon) and "The cost of switching is too high".

**Why We Win Where CarbonRunner Failed:**

1. **We sell savings, not carbon.** Cost decides first. Carbon breaks ties for free. Optionally, self-imposed eco goals can be configured to be part of the routing.
2. **We are not a vendor.** EcoRouter routes across the runners you already have. Open source, no contract, no migration.
3. **Time and location, cloud and on-prem.** CarbonRunner did only cloud location.

**Why do we focus on CI only?:**

First, we thought of pitching a more generic Follow-the-Sun Routing that dynamically routes latency-tolerant asynchronous compute- and electricity-intensive jobs / workloads to the "greenest" regions with peak surplus renewable energy (=lowest gCO₂eq/kWh). We didn't think of the fact that most jobs require caching or have dependencies in certain isolated networks and therefore cannot easily be re-routed.

With CI jobs, this is a bit different. Especially for open source software, it doesn't matter where a CI job is run.


## Deliverables

- **EcoRouter Docker Container:** Dynamic routing for ci-jobs
- **Dashboard**: Visualizes real-time traffic rerouting (clients, requests, energy generation & regions) on a map and compares the real-time CO₂ savings side-by-side (_Green Routing Ratio_).
- Add-On: **Visual Rule Builder:** UI to create routing criteria
- Add-On: **MCP Server Interface:** Exposes metrics (`ecoroutee://stats`) for AI agents
- Add-On: **Integrations System:** Allows to add further ci-service-providers or inputs for the rules

## Physical Hardware Live Demo

We plan to show 3–4 Raspberry Pis representing distributed cloud regions (Nordics, Central Europe, Southern Europe).
Each Pi is equipped with a mini solar panel and an I2C light sensor and mounted on a 3D-printed diorama.

Once we shine a flashlight onto a solar panel, we will simulate a solar surplus and lower the illuminated region's carbon intensity.

During the demo, ci jobs will be generated automatically in realistic burst patterns.
The proxy will route to the _greenest_ Pi, which will then execute the ci job.
(e.g. running unit tests, end-to-end testing, visual regression tests,...)

The effectiveness of our routing will be shown by the developed dashboard and the Pis CPU load.

## About the Project Leads

EcoRouter is led by **Alexander Melde** and **Milan Ferus-Comelo**, combining enterprise software development with award-winning IoT prototyping.

Alexander (M.Sc. in Computer Science) contributes deep expertise in full-stack development, TypeScript, and AI systems, paired with energy-sector domain knowledge building grid-sensor solutions.

Milan is an edge-AI and IoT developer with an electrical engineering background whose work has been awarded by the KI Bundesverband, the United Nations and the German Federal Ministry of Research, Technology and Space. He even won three awards at CloudFest Hackathon 2026.


## Summaries for the project submission:

**Pitch / Description:**
EcoRouter is a carbon-aware routing layer that optimizes ci-job runner selection. By tracking real-time grid emissions, system load, costs and task urgency, it dynamically forwards ci-workloads to ideal runners. It acts as a minimal-overhead open-source proxy, meaning you don't need to rewrite any code to use it. Users can balance their ci load, optimize cost and slash CO₂ emissions in custom profiles, so EcoRouter adapts to their needs.

**Goal / MVP by the End of the Hackathon:**
Our MVP is a working proxy that routes ci jobs based on live carbon data, paired with a dashboard tracking real-time CO₂ and cost-savings via a "Green Routing Ratio". To show exactly how it works, we’re bringing a tactile hardware demo: a cluster of Raspberry Pis hooked up to mini solar panels and light sensors. Shine a flashlight on a sensor to simulate a solar surplus, and watch EcoRouter instantly redirect the workload to that "green" node. We can extend the system by developing an MCP server so AI agents can monitor their own sustainability stats.

**Long-Term Viability Post-Hackathon:**
Green cloud computing is a massive enterprise trend, but it usually means getting locked into a specific cloud provider or dealing with complex code rewrites. EcoRouter solves this by being open-source, multi-cloud, and entirely frictionless. After the hackathon, we plan to scale it to more providers and create a easy to add-on integration system. Because EcoRouter offers a powerful dual incentive by helping companies hit strict ESG climate goals while actively lowering electricity bills, it has huge commercial potential as a standard, drop-in DevOps tool.


## Sources:
¹ [Github - Octoverse: Commits, pushes, reach new highs in 2026 as developers build with agents](https://github.blog/news-insights/octoverse/octoverse-commits-pushes-reach-new-highs-in-2026-as-developers-build-with-agents/)  
² [Saavedra et.al. - Environmental Impact of CI/CD Pipelines](https://arxiv.org/abs/2510.26413)  
³ [CarbonRunner](https://github.com/carbonrunner)


----------

# Further Details and Ideas, Partly unverified AI:

## Inputs

- **Emissions Data** from real grid: Electricity Maps API, WattTime
- **Geofencing & Constraints:**
  Restricts target regions via `config` files, environment variables, region-specific subdomains,
  URL paths or request headers (`eu.ecoroute.io`, `X-EcoRoute-Zone: EU-only`, `ALLOWED_REGIONS=[...]`)

## Tech Stack

- **Option A: TypeScript / Node.js (Fastify):** Rapid development, official MCP TypeScript SDK support, WebSocket integration.
- **Option B: Rust (Axum / Actix-web):** Minimal memory footprint and native bare-metal performance at the edge.

TypeScript: a route action plus a small router service (Fastify, SQLite queue). A site agent on each Pi. A dashboard with the decision log, prices, grid map and savings.

## Competitors

Full competitor comparison: [docs/competitors.md](docs/competitors.md).

### What We Take From Each Competitor

| From                    | We take                       | We add                                       |
| ----------------------- | ----------------------------- | -------------------------------------------- |
| CarbonRunner            | Live carbon-aware location    | Not a vendor; open source; time-shifting too |
| Blacksmith Flex         | "Can wait" jobs are cheaper   | Across all providers; waits for a green hour |
| RunsOn                  | Per-job `region=` label       | Region chosen from live grid data            |
| Ubicloud / Namespace    | Lowest price per minute       | Picked automatically when they win           |
| Carbon-Aware Dispatcher | Time-shifting                 | Location-shifting + deadline fallback        |
| runner-fallback-action  | Multi-backend fallback        | Decides on cost, carbon and urgency          |
| Eco-CI                  | Energy per job                | Savings report per repo                      |

## How It Works

1. **Classify:** A rules file looks at the actor (human, bot or agent), draft state, event and labels, and returns *urgent* or *deferrable + deadline*. A `ci:now` label promotes a job to urgent.
2. **Score each backend:** its price per minute, the grid intensity of its location now and forecast (location-based, per the Green Software Foundation's SCI spec, so "100% renewable" claims don't count), queue health, and warm cache.
3. **Route:** A `route` job outputs the label, and workflows use `runs-on: ${{ needs.route.outputs.runner }}`. Only backends from an allow-list are chosen, so a job never waits on a label nobody serves.
4. **Shift in time:** If a much greener hour is forecast before the deadline, the job waits for it. This helps in Germany, where the daily profile swings between ~239 and ~387 g, and at solar sites.
5. **Report:** € saved, kgCO₂e avoided, and human time-to-green, per repo. The CO₂ figures are ready for carbon reporting.

**Security:** Fork PRs never go to self-hosted backends.

## Runtime Estimation

Every score depends on how long a job will run on each backend. EcoRouter learns this from the job's own history.

1. **Collect:** After each run, read start time, end time and runner labels from GitHub's jobs API. Key them by `repo / workflow / job (matrix values) / backend`.
2. **Estimate:** Use the median runtime for cost and CO₂, and the 90th-percentile runtime for the deadline check. Recent runs are weighted more heavily, so estimates follow a growing test suite.
3. **Transfer:** Each backend has a speed factor relative to GitHub-hosted. It starts from vCPU count and a short benchmark, then is refined from jobs that ran on both backends.
   `runtime(B) = baseline × speed(B) + boot(B) + cold-cache penalty`
4. **Cold start:** Jobs with no history use similar jobs in the same workflow, then the median for their job-name pattern (`lint`, `e2e`, …), then a conservative default. A job's first runs go to GitHub-hosted to set a baseline. About 5% of deferrable jobs go to other backends so speed factors keep learning.
5. **Score:**
   - **Cost** = billed time × price, using each provider's rounding (GitHub rounds up per minute, Depot bills per second).
   - **Energy** = runtime × an estimated power draw for that machine size (Cloud Carbon Footprint per-vCPU coefficients; Eco-CI for calibration).
   - **CO₂** = energy × grid intensity averaged over the predicted run window.
   - **Deadline check:** queue + boot + 90th-percentile runtime + margin must finish in time. Backends that fail it are excluded.
6. **Workflows:** The deadline applies to the whole workflow, so estimates add up along the longest chain of dependent jobs (`needs:`).
7. **Feedback:** Predicted vs. actual runtime is logged and shown on the dashboard. Backends that keep running late get a larger margin automatically.

For the demo, the 30-day replay fills the history in advance, so estimates work from the first run.

## Policy Example

```yaml
# ecorouter.yml (example values)
backends:
  - { label: ubuntu-latest,                          price: 0.006,  zone: auto }
  - { label: ubicloud-standard-2,                    price: 0.002,  zone: DE }
  - { label: "runs-on=${{run}}/runner=2cpu-linux-x64/region=eu-north-1", price: 0.0015, zone: SE }
  - { label: [self-hosted, office],                  price: 0,      zone: DE, solar: inverter }
classes:
  urgent:     { when: [human_pr], pick: fastest }
  deferrable: { when: [agent_pr, bot_pr, draft, schedule], deadline: 4h, pick: cheapest, green_tolerance: 15% }
```

## Inputs

- **Grid:** Electricity Maps (live), ENTSO-E (free day-ahead forecasts).
- **Prices:** per backend from config, plus Blacksmith Flex discounts.
- **Sites:** PV output (inverter API or sensor) and dynamic tariff.
- **GitHub:** a queued job can wait up to 24 h before it is cancelled.

## Physical Hardware Live Demo
Raspberry Pis act as runners in different grid zones, plus a solar-powered office site: agent PRs flow to the cheapest and cleanest node, human PRs take the fast lane, and a dashboard shows € and kgCO₂e saved per 100 agent PRs.

Each Raspberry Pi is a backend in a different zone (Stockholm, Frankfurt, Helsinki) or the office site, labelled with its live gCO₂/kWh and price. The office Pi has a mini solar panel read by an INA219 sensor.

1. A coding agent opens 5 PRs live. The router sends them to the cheap Stockholm Pi and the dashboard shows why.
2. A human opens a PR, and it goes straight to the fast lane with no wait.
3. We shine a lamp on the office panel, and its marginal cost drops to ~0. Queued jobs move to the office Pi.
4. We pull the plug on a backend. Its jobs fall back to GitHub, and no deadline is missed.

## Impact Metrics

- **Primary:** € and kgCO₂e saved per 100 agent PRs.
- **Guardrail:** p95 time-to-green for human PRs must not get worse.
- **Proof:** replay 30 days of Actions history from a real, busy open-source repo through the router with real prices and grid data.

## MVP
 A route action that classifies jobs and scores real backends by price and live grid data, with a deadline queue and automatic fallback. 
 
**Build:** route action + rules classifier, scoring on price and grid data (live and forecast), 3 real backends (GitHub-hosted, one cheap provider, one RunsOn region or self-hosted Pi), deadline queue, dashboard, Pi demo.
**Cut:** a GitHub App or webhook broker (v2), cloud VM provisioning, ML classification, GitLab.


## Long-term
A webhook broker for org-wide routing, GitLab support, more backends (Namespace, WarpBuild, Cirun, Hetzner), and inverter and tariff integrations. It becomes the neutral, open-source routing layer above every CI runner vendor, with sustainability built in instead of sold separately.

