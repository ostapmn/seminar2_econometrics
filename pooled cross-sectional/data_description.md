### World Happiness Report Dataset Description (2017–2019)

This dataset provides a comprehensive collection of information regarding global happiness levels across various countries for the years 2017, 2018, and 2019. It is based on the World Happiness Report, a landmark survey that ranks nations by how happy their citizens perceive themselves to be. The data combines economic, social, and health factors to explain the variations in happiness scores across the globe.

Column Descriptions:

Country – The name of the country or region.

**Score** – The Happiness Score, a metric measured by asking respondents to rate their own current lives on a scale from 0 (worst possible life) to 10 (best possible life).

**GDP_per_capita** – The extent to which Gross Domestic Product per person contributes to the calculation of the Happiness Score.

**Healthy_life_expectancy** – The extent to which life expectancy and health standards contribute to the happiness score in each country.

**Freedom** – The degree to which the perception of freedom to make life choices (autonomy) contributes to the country's happiness ranking.

**Generosity** – A numerical value representing the impact of charitable giving and generosity on the overall happiness evaluation.

**Perceptions_of_corruption** – The extent to which the absence of corruption in government and business contributes to the happiness of the population.

**year** – The specific year of the report (2017, 2018, or 2019), allowing for longitudinal analysis of happiness trends.

**Dataset Summary:** The dataset contains 467 observations (rows), representing a merger of data from the 2017, 2018, and 2019 reports. It includes approximately 164 unique countries. Each entry provides a snapshot of a nation's well-being based on the "Cantril Ladder" evaluation and identifies the key socio-economic drivers—such as wealth, health, and trust—that make some countries rank higher than others compared to a hypothetical "Dystopia" (a benchmark country with the world’s lowest national averages).

### Why pooled cross-sectional

**Pooled cross-sectional because it combines independent random samples from a population at different time points(2018-2019), acting like one large cross-section to analyze trends or policy impacts**

This dataset is pooled cross-sectional because:

1.  **Multiple time periods:** Data from 2017, 2018, and 2019 (3 independent cross-sections)

2.  **Independent samples at each time:** Each year represents a separate "snapshot" of countries - we're not tracking individual countries' changes over time, but rather treating each year-country observation as independent

3.  **Different units across time:** While the same countries appear in different years, we **don't follow specific countries' trajectories**. Instead, we pool all observations together (e.g., Norway 2017, Norway 2018, and Norway 2019 are treated as three separate observations, not one country tracked over time)

4.  **No longitudinal structure:** Unlike panel data, we don't have a country identifier that links observations of the same country across years - we simply stack years together

```{r}
data<-read.csv("data3.csv")
head(pooled_data)
```

```{r}
pooled_data <- data %>%
  mutate(
    # Convert all numeric columns to proper numeric type
    Score = as.numeric(Score),
    GDP_per_capita = as.numeric(GDP_per_capita),
    Healthy_life_expectancy = as.numeric(Healthy_life_expectancy),
    Freedom = as.numeric(Freedom),
    Generosity = as.numeric(Generosity),
    Perceptions_of_corruption = as.numeric(Perceptions_of_corruption)
  )
```

```{r}
model1 <- lm(Score ~ GDP_per_capita + Healthy_life_expectancy + 
             Freedom + Generosity + Perceptions_of_corruption, 
             data = pooled_data)


summary(model1)
```

We estimated a linear regression model to predict happiness scores using economic, health, and social factors.

Score = β₀ + β₁(GDP) + β₂(Health) + β₃(Freedom) + β₄(Generosity) + β₅(Corruption) + ε

The model explains 75.7% of the variation in happiness (R² = 0.757, F = 286, p \< 0.001).

Freedom to make life choices had the strongest effect (β = 1.94, p \< 0.001), followed by healthy life expectancy (β = 1.31, p \< 0.001) and GDP per capita (β = 1.27, p \< 0.001).

Generosity showed a marginally significant positive effect (β = 0.44, p = 0.071), while perceptions of corruption had no significant relationship with happiness (β = 0.44, p = 0.159).

**Strong predictors:** Freedom, Health, GDP\
**Weak predictor:** Generosity\
**Not significant:** Corruption perceptions\
**Model fit:** Very good (75.7%)\
**Most important finding:** **Freedom matters most for happiness!**

### Questions:

1\. Which factors matter most for national happiness?

2\. Does economic wealth still matter for happiness?

3\. Is freedom more important than wealth for happiness?
