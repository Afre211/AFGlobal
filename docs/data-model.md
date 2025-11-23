# Data model draft

## Entities
- **AthleteProfile**
  - Academic: GPA, scale, class rank percentile, SAT/ACT, core-course completion, honors/AP count, improvement trend.
  - Athletic: sport, position, measurables (height/weight/wingspan/40-time/vertical), season stats, awards.
  - Recruiting signal: star rating, composite rank, offer list, year, verified events (combine, camps).
- **School**
  - Identity: name, conference, division (FBS/FCS/D1/D2/D3/NAIA/JUCO), geography.
  - Academic metrics: 6-year graduation rate, GSR/FGR, first-year retention, admit rate, middle-50 SAT/ACT, avg GPA, aid/support programs.
  - Athletic metrics: roster counts by position, historical playing time distribution by class, recent transfers/portal entries, coaching tenure.
- **Indices**
  - **Academic Orientation Index (AOI):** normalized 0-1 score; percentiles vs. school-level academic distributions and readiness thresholds.
  - **Athletic Intensity Index (AII):** normalized 0-1 score; percentile vs. recruiting class and roster competition.
- **Recommendation**
  - School, tier (Ideal / Reach with support / Athletic-first risk), AOI alignment, AII opportunity, rationale factors, data version IDs.

## Relationships
- AthleteProfile has many Recommendations.
- School joins to many Recommendations.
- Indices reference source snapshot IDs for auditability.

## Example schemas (JSON-like)
```json
AthleteProfile {
  id: string,
  sport: "football" | "basketball",
  position: string,
  class_year: int,
  academics: {
    gpa: float,
    gpa_scale: 4.0 | 5.0,
    sat: int | null,
    act: int | null,
    core_courses_completed: int,
    honors_ap_classes: int,
    trend: "improving" | "stable" | "declining"
  },
  athletics: {
    height_in: int,
    weight_lb: int,
    measurables: { "40_time": float?, "vertical": float? },
    stats: { "passing_ypa": float?, "per": float? }
  },
  recruiting: {
    star_rating: float?,
    composite_rank: int?,
    offer_list: string[],
    verified_events: string[]
  }
}
```

```json
School {
  id: string,
  name: string,
  division: "FBS" | "FCS" | "D1" | "D2" | "D3" | "NAIA" | "JUCO",
  conference: string,
  geography: { state: string, region: string },
  academics: {
    grad_rate_6yr: float,
    gsr: float?,
    fgr: float?,
    retention_rate: float,
    admit_rate: float,
    sat_mid_50: [int, int]?,
    act_mid_50: [int, int]?,
    avg_gpa: float?,
    support_programs: string[]
  },
  athletics: {
    sports_offered: string[],
    roster_by_position: Record<string, int>,
    playtime_distribution: Record<string, float>,
    transfers_out: int,
    transfers_in: int,
    coaching_tenure_years: float
  }
}
```

```json
Recommendation {
  athlete_id: string,
  school_id: string,
  tier: "ideal" | "reach_support" | "athletic_first_risk",
  aoi_score: float,
  aii_score: float,
  rationale: string[],
  data_snapshot: { academics_version: string, recruiting_version: string }
}
```

## Scoring considerations
- AOI and AII should be bounded [0,1] with interpretable cutlines (e.g., >=0.7 = strong alignment).
- Use z-scores or percentiles relative to school-specific distributions (admit stats) and national recruiting data by position.
- Penalize mismatches where AII is high but AOI is below school safety threshold; flag as "Athletic-first risk".
- Boost opportunities where AOI is strong and depth chart indicates early playing time probability.
