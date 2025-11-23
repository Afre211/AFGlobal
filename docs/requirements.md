# Functional requirements

## Audience & user journeys
- **Athletes/parents** enter academic (GPA, SAT/ACT, core courses, honors/AP), athletic profile (sport, position, measurables, stats), and recruiting signal (stars, composite rank).
- **Counselors/coaches** review athlete profiles, adjust academic risk tolerance, and download guidance for meetings.
- **Admins** manage data refresh schedules, source credentials, and scoring parameters.

## Data sources
- **Academic:** NCAA/IPEDS datasets (6-year graduation rate, GSR/FGR, retention, admissions selectivity, tuition/aid, academic support programs).
- **Athletic:** Recruiting services (class ranks, star ratings, verified measurables, game stats), team depth charts, redshirt/portal data.
- **Institutional context:** Conference strength, coaching stability, roster sizes, positional utilization.

## Core capabilities
1. **Profile intake**
   - Validates GPA scale (4.0/5.0), test superscore options, and core-course compliance.
   - Collects sport-specific stats (e.g., YPA/COMP% for QBs, PER/TS% for guards) and measurables.
2. **Indices**
   - **Academic Orientation Index (AOI):** weighted composite of GPA percentile vs. school distribution, standardized tests vs. middle-50%, course rigor, and trend (improving/declining).
   - **Athletic Intensity Index (AII):** position-adjusted percentile vs. recruiting class and current roster; includes starter probability and transfer/portal risk.
3. **Matching engine**
   - Filters by basic constraints (sport offered, division level, geography preferences, cost limits).
   - Scores schools on academic fit first (weight > athletic fit) and recommends tiers: **Ideal**, **Reach with support**, **Athletic-first risk**.
   - Surfaces explanation factors (e.g., "AOI matches schools where >75% of similar students graduate").
4. **Data freshness**
   - Nightly ETL to refresh NCAA/IPEDS feeds; weekly sync for recruiting rankings; automated alerts on schema/source changes.
5. **Reporting & export**
   - Shareable PDF/CSV of recommended schools with rationale and assumptions.

## Non-functional requirements
- **Transparency:** display how AOI/AII are calculated and allow weight adjustments.
- **Privacy:** PII minimized; athlete identifiers kept separate from analytics datasets.
- **Auditability:** log data source versions and scoring inputs per recommendation run.
- **Reliability:** background ETL with retries and monitoring dashboards.

## Open questions
- Which recruiting sources (247/On3/Rivals) are licensable for automated pulls?
- How to validate self-reported stats/measurables (video links, coach verification)?
- Do we recommend JUCO/NAIA as off-ramps for low-AOI but high-AII athletes?
