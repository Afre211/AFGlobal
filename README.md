# AFGlobal

AFGlobal is an online guidance platform that helps rising football and basketball student-athletes find colleges where they can thrive academically and athletically. The tool balances academic readiness with recruiting projections to generate school recommendations that reduce the risk of poor academic fit while acknowledging athletic ambitions.

## Project goals
- Provide an objective matching engine that emphasizes graduation and retention outcomes alongside athletic trajectories.
- Combine NCAA/IPEDS academic data with public recruiting rankings and player statistics to keep recommendations current.
- Offer transparent indices that explain why a school is suggested, reducing emotional or incentive-driven decision making during recruiting.

## High-level approach
1. **Collect and normalize data** from NCAA/IPEDS (graduation rates, admissions selectivity, cost, support resources) and recruiting databases (class rank, star ratings, positional benchmarks, usage/efficiency stats).
2. **Score athletes** on two dimensions:
   - **Academic Orientation Index (AOI):** synthesized from GPA, SAT/ACT, core-course completion, and class rank against institutional distributions.
   - **Athletic Intensity Index (AII):** benchmarked against recruiting class distributions (e.g., top-100, top-300, power-five starters) using position-specific comparables.
3. **Match to schools** using rules and weights that favor academic fit first, then athletic opportunity (depth charts, playing time probability, conference strength).
4. **Explain recommendations** with transparent factors (e.g., "80% of similar GPAs graduated in 6 years at School X; depth chart shows sophomore starter opening").

## Repository structure
- `docs/requirements.md`: detailed functional requirements, data inputs, and scoring definitions.
- `docs/data-model.md`: draft data model for schools, athletes, indices, and recommendations.

## Next steps
- Build ETL pipelines for NCAA/IPEDS and recruiting sources.
- Prototype AOI/AII calculators and calibration datasets.
- Create an interactive UI for athletes/parents to input academic and athletic profiles.
- Add monitoring to keep recruiting datasets refreshed for each class cycle.
