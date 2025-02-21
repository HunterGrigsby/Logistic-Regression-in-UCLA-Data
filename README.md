
# Logistic Regression in UCLA Data
## Hunter Grigsby


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

# Data Preperation
### Resources: "https://stats.idre.ucla.edu/stat/data/binary.csv"

```{r}
# packages required
library(aod)
library(ggplot2)

path <- "https://stats.idre.ucla.edu/stat/data/binary.csv"
mydata <- read.csv(path)
```

- The dataset contains information on applicants, specifically whether they were admitted (admit), their GRE scores (gre), GPAs (gpa), and the rank of their undergraduate institution (rank).

## Data Checkup
```{r}
str(mydata)
mydata$rank = as.factor(mydata$rank)
str(mydata)

summary(mydata)
```
- The summary(mydata) output provides key descriptive statistics for each variable in the dataset, such as minimum, maximum, median, mean, and the quartiles, which help in understanding the distribution and central tendencies of GRE scores, GPAs, and undergraduate institution ranks. Additionally, the conversion of the rank variable to a factor type in R, as shown in the str(mydata) output, facilitates categorical analysis, ensuring that statistical methods appropriate for nominal data are applied in subsequent analyses.

# Regression = Estimatation
```{r}
mylogit = glm(admit ~ gre + gpa + rank, data = mydata, family = "binomial")
summary(mylogit)
```
- The output from the logistic regression model shows that GRE scores (p = 0.038465) and GPA (p = 0.015388) are statistically significant predictors of admission, indicating that higher values in these variables increase the likelihood of admission. The negative coefficients for rank2, rank3, and rank4 (all p < 0.05) suggest that applicants from lower-ranked institutions have a reduced chance of admission compared to those from top-ranked institutions, controlling for other factors.

# Calculate Odds Ratios
```{r}
coefs = coefficients(mylogit)
ORs = exp(coefs)
CIs = exp(confint(mylogit))
CIs
cbind(round(ORs,2), round(CIs,2))
```
- GRE Scores (gre): The odds ratio of 1.00 indicates negligible impact on admission odds per unit increase in GRE scores.
- GPA (gpa): With an odds ratio of 2.23, each unit increase in GPA significantly boosts admission odds, suggesting GPA is a strong predictor.
- Rank 2 (rank2): Applicants from institutions ranked '2' have roughly half the admission odds compared to top-ranked schools.
- Rank 3 (rank3) and Rank 4 (rank4): These ranks significantly lower the admission odds to 0.26 and 0.21, respectively, indicating much lower chances of admission for applicants from these institutions.
