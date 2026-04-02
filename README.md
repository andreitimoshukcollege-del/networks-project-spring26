# RTT vs. Speed-of-Light
## Networks Assignment — Measurement & Geography

### Overview
Light through fiber travels at roughly **2/3 the speed of light in vacuum** (~200,000 km/s). This means every city pair has a *theoretical minimum RTT* — the time it would take if the packet went in a perfectly straight line through fiber with zero processing. Real RTTs are always higher. In this assignment you measure the gap and figure out *why*.

### The Core Equation
```
theoretical_min_rtt_ms = (great_circle_distance_km / 200,000) * 2 * 1000
inefficiency_ratio     = measured_rtt_ms / theoretical_min_rtt_ms
```
An inefficiency ratio of **1.0** would mean perfect routing. Real values are typically **1.5–5x** and sometimes much higher for poorly-connected regions.

---

### Target Cities

| City | URL Target | Coordinates |
|------|-----------|-------------|
| Tokyo, Japan | `http://www.google.co.jp` | 35.6762, 139.6503 |
| São Paulo, Brazil | `http://www.google.com.br` | -23.5505, -46.6333 |
| Lagos, Nigeria | `http://www.google.com.ng` | 6.5244, 3.3792 |
| Frankfurt, Germany | `http://www.google.de` | 50.1109, 8.6821 |
| Sydney, Australia | `http://www.google.com.au` | -33.8688, 151.2093 |
| Mumbai, India | `http://www.google.co.in` | 19.0760, 72.8777 |
| London, UK | `http://www.google.co.uk` | 51.5074, -0.1278 |
| Singapore | `http://www.google.com.sg` | 1.3521, 103.8198 |

---

### Task 1 — Measure RTTs (30 pts)
Complete `measure_rtt()` in `rtt_speedoflight.py`:
- Send **15 HTTP requests** to each target using `urllib.request.urlopen()`
- Record all successful RTTs; count timeouts as lost packets
- Report: **min, mean, median, and packet loss %**
- Print a clean summary table to stdout

### Task 2 — Great-Circle Distance & Inefficiency (35 pts)
Complete `great_circle_km()` and `compute_inefficiency()`:
- Implement the **Haversine formula** from scratch (no geopy)
- Your source location is your machine — use `requests.get("https://ipinfo.io/json")` to get your coordinates
- For each city compute `theoretical_min_rtt_ms` and `inefficiency_ratio`
- Flag any city with ratio > 3.0 as high inefficiency

### Task 3 — Plots (35 pts)
Complete `make_plots()` — produce two figures:

**Figure 1 — Bar chart:** measured median RTT vs. theoretical min RTT per city, sorted by distance. Grouped bars, labeled axes, legend.

**Figure 2 — Scatter plot:** great-circle distance (x) vs. measured RTT (y), with a dashed line showing the theoretical minimum. Label each city. Color by continent.

Save both as PNGs in a `figures/` folder.

---

### Analysis (in `analysis.md`) — 20 pts bonus
Answer three questions:
1. Which city has the highest inefficiency ratio? Look it up on [submarinecablemap.com](https://submarinecablemap.com) — what cables serve it and how does that explain your result?
2. Which city is closest to the theoretical minimum? What does that tell you about routing infrastructure there?
3. Your packet to Lagos almost certainly routes through Europe first. Why does Africa route through Europe, and what would need to change to fix it?

---

### Setup
```bash
pip install requests matplotlib numpy
python rtt_speedoflight.py   # no sudo needed
```

### Deliverables
```
submission/
├── rtt_speedoflight.py
├── figures/
│   ├── fig1_rtt_comparison.png
│   └── fig2_distance_scatter.png
├── terminal_screenshot.png        ← required (see below)
└── analysis.md
```

### Terminal Screenshot Requirement
Your submission **must include a screenshot** of your terminal showing the full output of running the script. The first line printed is your auto-detected location and coordinates, e.g.:

```
Your location: Boston (42.3601, -71.0589)
```

This must be visible in the screenshot. Submissions without it, or where the measured RTTs are inconsistent with the reported location (e.g., 5ms to Tokyo from Boston), will receive a zero on Task 1.

### Grading
| Component | Points |
|-----------|--------|
| Task 1 — RTT measurement | 30 |
| Task 2 — Haversine + inefficiency | 35 |
| Task 3 — Plots | 35 |
| Analysis (bonus) | 20 |
| **Total** | **100 + 20 bonus** |
