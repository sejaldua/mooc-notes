---
marp: true
theme: gaia
size: 16:9
_class: 
- lead
- invert
paginate: true
math: katex
---

# Sports Performance Analytics Specialization

---

# Foundation of Sports Analytics

---

## Week 1

- Pythagorean Expectation & Baseball
  - win percentage = $\frac{\text{number of wins}}{\text{number of games}}$
  - pythagorean expectation = $\frac{\text{(runs scored)}^2}{{\text{(runs scored)}}^2 + {\text{(runs against)}}^2}$
    ```python
    sns.relplot(x="pyth", y="wpc", data=MLB18)
    ```

    ```python
    pyth_lm = smf.ols(formula = 'wpc ~ pyth', data=MLB18).fit()
    pyth_lm.summary()
    ```

