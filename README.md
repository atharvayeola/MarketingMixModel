# Marketing Mix Modeling (MMM)

A complete econometric framework for analyzing marketing effectiveness and optimizing budget allocation across multiple channels using adstock transformations and saturation curves.

## 📊 Project Overview

This project implements a Marketing Mix Model to quantify the incremental impact of marketing spend on sales. Using 104 weeks of synthetic data across four channels (TV, radio, social media, and paid search), the model accounts for advertising carryover effects and diminishing returns to provide actionable ROI insights.

## 🎯 Key Results

- **Model Performance**: R² = 0.76 with statistically significant coefficients for key media variables
- **Channel ROI** (incremental sales per $1 spent):
  - TV: $3.55
  - Social Media: $3.03
  - Paid Search: $2.30
  - Radio: $1.54 (not statistically significant)
- **Non-Media Drivers**:
  - Promotions: +$63k weekly sales lift
  - Holidays: +$59k weekly sales lift
  - Price: -$55k sales per $1 price increase

## 🔧 Technical Implementation

### Data Processing
- **104 weeks** of marketing data (Jan 2023 - Dec 2024)
- **4 media channels**: TV, radio, social media, paid search
- **Control variables**: price, promotions, holidays

### Key Features

1. **Adstock Transformation**
   - Models carryover effects with configurable decay rates (default: 0.5)
   - Captures how advertising impact persists beyond the initial exposure period

2. **Saturation Curves**
   - Log1p transformation to model diminishing returns
   - Reflects realistic response patterns as spend increases

3. **OLS Regression**
   - Statsmodels implementation with constant term
   - Generates coefficients, p-values, and confidence intervals

4. **ROI Calculation**
   - Channel-specific return on investment metrics
   - Accounts for both adstock and saturation effects

5. **Scenario Simulation**
   - Test "what-if" budget allocation scenarios
   - Forecast sales impact before making spending decisions

## 📁 Project Structure

```
MMM/
├── MMM.ipynb                          # Main Jupyter notebook
├── synthetic_marketing_data.csv       # Sample dataset (104 weeks)
├── MMM_report.pdf                     # Detailed findings report
└── plots/                             # Time series visualizations
```

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn statsmodels
```

### Usage

1. **Load and explore data**:
```python
df = load_data('synthetic_marketing_data.csv')
summary_statistics(df)
plot_time_series(df, output_dir='plots')
```

2. **Transform media variables**:
```python
media_cols = ['tv_spend', 'radio_spend', 'social_spend', 'search_spend']
df_transformed = transform_media_variables(df, media_cols, decay=0.5)
```

3. **Build and evaluate model**:
```python
feature_cols = [f"{col}_sat" for col in media_cols] + ['price', 'promotion', 'holiday']
model = build_linear_model(df_transformed, feature_cols, target_col='sales')
print(model.summary())
```

4. **Calculate ROI**:
```python
roi = calculate_roi(model, df_transformed, media_cols)
```

5. **Run scenario simulation**:
```python
scenario = {
    'tv_spend': df['tv_spend'].iloc[-1] * 1.2,      # +20% TV
    'social_spend': df['social_spend'].iloc[-1] * 0.8  # -20% social
}
predicted_sales = scenario_simulation(model, df_transformed, media_cols, scenario)
```

## 💡 Key Insights

### Budget Allocation Recommendations

1. **Prioritize high-ROI channels**: TV and social media deliver 3x+ returns
2. **Reassess underperformers**: Radio shows limited incremental impact; consider reallocation
3. **Leverage promotions**: Clear $63k lift per promotional week
4. **Monitor price sensitivity**: Strong negative elasticity (-$55k per $1 increase)

### Scenario Analysis Example

Simulating a 20% TV increase and 20% social decrease resulted in **-$17.5k sales decline**, demonstrating that cutting high-ROI channels to fund others can hurt overall performance.

## 📈 Model Specifications

- **Response Variable**: Weekly sales
- **Predictors**: Transformed media spend (adstock + saturation), price, promotion flag, holiday flag
- **Method**: Ordinary Least Squares (OLS)
- **Observations**: 104
- **Adjusted R²**: 0.741
- **F-statistic**: 43.11 (p < 0.001)

## 🔄 Future Enhancements

- [ ] Bayesian MMM implementation for uncertainty quantification
- [ ] Grid search for optimal adstock decay parameters
- [ ] Cross-validation and holdout testing
- [ ] Budget optimization using constrained optimization
- [ ] Integration with real-time dashboards
- [ ] Multi-touch attribution comparison

## 📚 References

- Adstock modeling for advertising carryover effects
- Diminishing returns via log transformations
- Econometric time series analysis

## 📝 License

This project uses synthetic data for demonstration purposes. Feel free to adapt the methodology for your own marketing analytics use cases.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

---

**Note**: Results shown are based on synthetic data. Real-world applications require careful validation, regular model refresh, and A/B testing to confirm incremental lift estimates.
