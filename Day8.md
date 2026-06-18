# Day 8 — Personal Environmental Health Analyzer

## What I Built
An interactive environmental health dashboard for Varanasi using Claude AI,
featuring live AQI data, water quality analysis, and personalized health insights.

## Tools Used
- Claude Sonnet (claude.ai) for data retrieval, analysis, and artifact generation
- Real-time AQI data from AQI India, AQICN, IQAir, AQI Today
- Water quality research from BHU Varanasi & CPCB studies

## Key Findings
- Varanasi AQI: 142 (Poor) — ranks in global top 15 most polluted cities
- PM2.5 at 71 µg/m³ = 14× WHO guideline
- Water hardness: 580 mg/L (limit: 300 mg/L) — critical for hair & skin
- BHU campus (AQI 29) vs Ardhali Bazar (AQI 155) — 5× gap within same city

## Dashboard Features
- 7 interactive sections: Overview, Air, Water, Health, Report Card, Insights, Recs
- Live charts: bar, line, doughnut, radar
- Environmental Health Score: 34/100

## Learnings
- Prompt engineering with multi-role context (Analyst + UX + Dev) yields richer output
- Claude can fetch live web data and embed it directly into interactive artifacts
- Varanasi's cremation ghats create a unique 24/7 PM2.5 source unlike any other city
