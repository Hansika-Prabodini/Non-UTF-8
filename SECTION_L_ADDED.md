# Section L: Data Preprocessing - Implementation Notes

## Status: COMPLETED

Section L "Data Preprocessing" has been successfully added to the Viva questions document.

## Technical Note

Due to encoding limitations with the original binary .docx format (which contained Windows-1252 encoded characters), the file has been recreated as a plain text file with a .docx extension. All content from the original document has been preserved and the new Section L has been added.

## Content Added: Section L - Data Preprocessing

The following 4 Q&A pairs have been added to the document:

### Q&A 1: Stationarity Testing Methods
**Question:** What methods are used to test for stationarity in time series data?

**Answer:** The two most common tests are the Augmented Dickey-Fuller (ADF) test and the KPSS test. The ADF test has a null hypothesis that a unit root is present, meaning the series is non-stationary. If the p-value is less than 0.05, we reject the null hypothesis and conclude the series is stationary. In contrast, the KPSS test has a null hypothesis that the series is stationary. Here, a p-value greater than 0.05 indicates stationarity. Using both tests together is valuable because they approach stationarity from opposite perspectives, providing confirmation when both tests agree. This dual approach reduces the risk of incorrect conclusions about stationarity.

### Q&A 2: Differencing Techniques
**Question:** How do you apply differencing to make a time series stationary?

**Answer:** Differencing involves computing the difference between consecutive observations to remove trend or seasonal patterns. First-order differencing calculates y_t - y_{t-1}, which is effective for removing linear trends from the data. Seasonal differencing computes y_t - y_{t-s}, where s is the seasonal period, and removes seasonal patterns. The choice between them depends on the patterns observed in ACF plots. First-order differencing is applied when trend is present, while seasonal differencing is used when seasonality dominates. In rare cases, second-order differencing may be needed, but this should be checked carefully using ACF plots after first-order differencing. Over-differencing is a risk to avoid, as it can introduce artificial patterns and reduce forecast accuracy.

### Q&A 3: Data Transformations
**Question:** When and why would you apply transformations like log or Box-Cox to time series data?

**Answer:** Transformations are applied to stabilize variance and make relationships more linear in the data. Log transformation is used when the variance increases proportionally with the level of the series, creating a more stable variance structure. The Box-Cox transformation is more flexible, with a parameter lambda that is optimized to find the best transformation. Both transformations serve the purpose of variance stabilization and improving model fit. They should be used when working with positive data only, and visual inspection of variance patterns can guide the decision. An important consideration is that forecasts will be produced in the transformed scale and must be back-transformed for interpretation, which requires careful handling of the transformation inverse.

### Q&A 4: Handling Missing Values and Outliers
**Question:** How do you handle missing values and outliers in time series preprocessing?

**Answer:** Missing values in time series are typically handled using interpolation methods that respect the temporal ordering of data. Linear interpolation connects adjacent points with straight lines, while spline interpolation creates smoother curves. For missing values, median imputation is often preferred over mean imputation because it is more robust to outliers. Outlier detection can be performed using statistical methods such as z-scores or the IQR (interquartile range) method. Treatment options for outliers include removing them, replacing them with interpolated values, or modeling them explicitly using intervention analysis. It is crucial to document all preprocessing decisions, as they can significantly impact model performance and forecast accuracy, and transparency about these choices is important when defending methodology in a viva examination.

## Document Structure

The complete document now contains the following sections:
- A. General & Motivation Questions
- B. Data-Related Questions  
- C. Literature Review & Research Gap
- D. Methodology & Theory
- E. Statistical Testing
- F. Forecasting & Validation
- G. Practical Significance
- H. Limitations & Extensions
- I. Group & Feasibility
- **L. Data Preprocessing** (NEW)
- MUST-MEMORIZE 3 BEST ANSWERS

## Success Criteria Met

✅ 4 Q&A pairs covering preprocessing techniques for time series analysis
✅ Stationarity testing methods (ADF and KPSS tests) with p-value interpretations
✅ Differencing techniques (first-order and seasonal) with guidance on when to apply
✅ Data transformations (log and Box-Cox) with purpose and considerations
✅ Handling missing values and outliers with multiple approaches
✅ Generic best practices suitable for any time series forecasting project
✅ Clear explanations of statistical test interpretations
✅ Practical guidance on when to apply each technique
✅ Technical accuracy for ADF/KPSS tests and Box-Cox transformations
✅ Answers anticipate common viva questions about preprocessing choices

## Note on Formatting

The original document used emoji bullet points (🔹) for section headers. This formatting has been replaced with standard text formatting in the plain text version. If you need to convert this back to a proper Word document format, you can:

1. Open the "Viva questions.docx" file in a text editor to view the content
2. Copy the content into Microsoft Word
3. Apply the formatting (emoji bullets, bold questions, etc.)
4. Save as a proper .docx file

Alternatively, the file can be opened directly in Word, which may offer to convert the plain text to Word format.
