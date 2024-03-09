## Exploratory Data Analysis and Predictive Analysis using R: Boston House Data 
**Overview**
- A simple [EDA](https://towardsdatascience.com/exploratory-data-analysis-8fc1cb20fd15) on the Boston House Data
- The goal was to prepare data as best as we can for predictive analysis.
  - This included checking dataset structure
  - Remove any NA values in our columns
  - Statistical summary
    - Min, Mean, Median, Max, Quartiles
  - Visuals
    - Boxplot
      - Helped in visualizing our statistics and seeing where the outliers stood
    - Correlation plot
      - See the level of linear dependence between two variables 


- Predictive Analysis using Linear Regression
  - Used 3 different Linear Models
    - Linear Model: Y = lm(Y ~ A, ...)
        - This model is a straight-line with an implicit y-interecept
    - Linear Model: Y = lm(Y ~ A + I(A^2), ...)
         - Polynomial model to find a relationship between independent variable and dependent variable
    - Linear Model: Y = lm(Y ~ A + B, ...)
         - First-order model in A, with no interaction terms

Let me know if you have an questions via [email](mailto:mustafacangok0@gmail.com).
