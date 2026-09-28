# Lassa-Fever-predictions
This project builds a reproducible weekly Lassa fever surveillance dataset from Nigeria Centre for Disease Control and Prevention reports published between 2020 and 2025. It develops weekly forecasts for 2026–2028 and evaluates forecast accuracy by comparing predicted 2026 case counts with subsequently reported NCDC epidemiological-week data.
Lassa fever is a recurring public health threat in Nigeria, with the Nigeria Centre for Disease Control (NCDC) publishing weekly surveillance reports since at least 2017. These reports are the country's primary record of laboratory-confirmed cases, but they exist only as individual PDF situation reports scattered across NCDC's website, in at least three different formatting conventions that have changed over the years. No structured, analysis-ready dataset existed that could answer a basic epidemiological question: does confirmed Lassa fever transmission in Nigeria follow a predictable seasonal pattern, and if so, can that pattern be used to anticipate the timing and scale of upcoming outbreaks?

What I built

I designed and implemented a reproducible data pipeline, developed collaboratively in a Jupyter Notebook, that:

Scraped NCDC's official Lassa fever situation-report archive and built a catalogue of every available report (480 reports spanning 2017 to 2026), recording each report's title, source URL, and metadata.
Downloaded all report PDFs, verified file integrity using SHA-256 hashing, and logged every download attempt for auditability.
Extracted weekly laboratory-confirmed case counts directly from the PDF text, handling at least three distinct table formats used by NCDC across different years.
Cross-validated every report's stated week and year against the actual content of the PDF, catching several genuine data errors on NCDC's own site, including a report mislabeled by an entire calendar year, a Yellow Fever report mistakenly linked under Lassa fever, and several structurally corrupted PDF files that could not be recovered even with dedicated repair tools.
Documented every correction, exclusion, and data gap in a formal audit trail, so any number in the final dataset can be traced back to its source and justified.
Aggregated the validated weekly data into a monthly time series covering January 2020 to December 2025 (72 months), the period with the most consistent reporting quality.

The analytical question

Using this monthly series, I tested whether standard time-series forecasting methods could predict short-term Lassa fever trends. I trained four candidate approaches (a seasonal-naive benchmark, SARIMA, a trend-plus-seasonality regression, and Holt-Winters exponential smoothing) on 2020 to 2024 data and evaluated each against the actual 2025 figures using standard accuracy measures (MAE, RMSE, MAPE).

What I found

The seasonal pattern itself is real and consistent: confirmed cases peak every year between January and March and drop to a low, stable baseline from April through November. That finding held up well against real 2025 and partial 2026 data.

The forecasting models, however, disagreed with each other substantially, and the more complex models (SARIMA in particular) performed worse than the simplest one. This is a known risk with short time series: only 60 to 72 monthly data points are not enough to reliably estimate the extra parameters that more sophisticated models require, so they tend to fit noise rather than signal. When I checked the models against real 2026 data as it became available, the ranking of "best" model changed again, which is further evidence that no single model can yet be trusted for multi-year planning numbers with this amount of data.

How I'm framing the result

Given this, I decided not to present any single 2026 to 2028 forecast as an operational prediction. Instead, I'm framing this as an exploratory, proof-of-concept forecasting evaluation: it demonstrates that the seasonal pattern is detectable and reproducible, and it establishes a validated, reusable dataset and methodology, but it stops short of claiming precise multi-year projections, because the sample size does not support that level of confidence. I present the four models' outputs side by side as illustrative scenarios rather than a single locked-in number, and I explicitly name what would be needed to move from exploratory to operational: a longer historical baseline (extending back to 2017), weekly rather than monthly modeling to increase the number of data points, and external covariates like rainfall or state-level surveillance capacity.
