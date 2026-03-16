# AI Exposure of the Canadian Job Market

Analyzing how susceptible every occupation in the Canadian economy is to AI and automation, using data from the [ESDC Job Bank](https://www.jobbank.gc.ca/trend-analysis/search-occupations) (NOC 2021) and Statistics Canada Labour Force Survey.

**Live demo:** [Click here](https://whitedeserts.github.io/canada-jobs/)

Inspired by [karpathy/jobs](https://github.com/karpathy/jobs) — a similar tool for the US economy using BLS data.

Note:This is a research tool for visually exploring ESDC Job Bank and Statistics Canada labour market data. This is not a report, a paper, or a serious economic publication — it is a development tool for exploring Canadian occupational data visually.


---

## What's here

The ESDC Job Bank covers occupations across the full Canadian economy under the **NOC 2021** classification system, with data on job duties, education requirements, median wages, and employment outlook. This repo pairs that data with LLM-generated AI exposure scores and builds an interactive treemap.

| File | Description |
|---|---|
| `site/index.html` | Interactive treemap visualization (self-contained, deployable via GitHub Pages) |
| `site/data.json` | Occupation data: employment, wages, outlook, education, AI exposure, rationale |
| `occupations.csv` | Flat CSV summary for easy inspection / downstream use |

---

## Visualization

The main visualization is an interactive **treemap** where:

- **Area** of each rectangle is proportional to employment (number of jobs)
- **Color** uses the RdYlGn diverging scale — red = worse outcome, green = better — consistent across all four layers
- **Layout** groups occupations by NOC 2021 major sector
- **Hover** shows a detailed tooltip with wage, employment, outlook, education, AI exposure, and LLM rationale
- **Click** any tile to search the ESDC Job Bank for that occupation

### Layers

| Layer | Red | Green |
|---|---|---|
| Job outlook | Declining | Very good |
| Median pay | ~$16/hr | $100+/hr |
| Education required | Secondary school | Graduate degree |
| Digital AI Exposure | High exposure (10) | Low exposure (0) |

---

## AI exposure scoring

Each occupation is scored on a single **Digital AI Exposure** axis from 0 to 10, measuring how much AI will reshape that occupation. The score considers both direct automation (AI doing the work) and indirect effects (AI making workers so productive that fewer are needed).

A key signal is whether the job's work product is fundamentally digital — if the job can be done entirely from a home office on a computer, AI exposure is inherently high. Conversely, jobs requiring physical presence, manual skill, or real-time human interaction have a natural barrier.

**Calibration examples:**

| Score | Meaning | Examples |
|---|---|---|
| 0–1 | Minimal | Landscapers, personal support workers, firefighters |
| 2–3 | Low | Plumbers, electricians, dental hygienists, paramedics |
| 4–5 | Moderate | Registered nurses, truck drivers, civil engineers |
| 6–7 | High | Teachers, managers, accountants, lawyers |
| 8–9 | Very high | Software developers, graphic designers, bookkeepers, data scientists |
| 10 | Maximum | Data entry clerks |

> **Caveat:** These are rough LLM estimates, not rigorous predictions. A high score does not predict the job will disappear. Software developers score 9/10 because AI is transforming their work — but demand for software could easily *grow* as each developer becomes more productive. The score does not account for demand elasticity, latent demand, regulatory barriers, or social preferences for human workers.

---

## Data sources

- **Employment & outlook:** [ESDC Job Bank — Occupational Trends](https://www.jobbank.gc.ca/trend-analysis/search-occupations)
- **Wages:** [Statistics Canada LFS — Table 14-10-0340-01](https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=1410034001)
- **Classification:** [NOC 2021 — Statistics Canada](https://www23.statcan.gc.ca/imdb/p3VD.pl?Function=getVD&TVQ=1267)
- **AI exposure scores:** Generated using an LLM prompt rubric (see `site/index.html` — "View scoring prompt")

Employment figures are approximate estimates based on published LFS ranges. Wages are median hourly (CAD).

---

## Running locally

No build step required — the site is a single self-contained HTML file loading data from `data.json`.

```bash
git clone https://github.com/whitedeserts/canada-jobs
cd canada-jobs/site
python -m http.server 8000
# open http://localhost:8000
```


## Extending this

The data pipeline mirrors Karpathy's approach and can be extended:

1. **Scrape real NOC data** — the Job Bank has a [public API](https://www.jobbank.gc.ca/developers) and downloadable datasets
2. **Add province filter** — wages and outlook vary considerably between BC, Ontario, Alberta, and the Maritimes
3. **Add bilingual labels** — NOC 2021 has full French occupational titles
4. **LLM re-scoring** — write a custom prompt and call an LLM API (e.g. OpenAI, Anthropic, OpenRouter) to score all occupations on any criterion: climate risk, remote-work potential, offshoring susceptibility, etc.
5. **Finer NOC granularity** — the current dataset uses ~90 representative occupations; a full NOC 2021 scrape covers 500+ unit groups

---

## License

MIT
