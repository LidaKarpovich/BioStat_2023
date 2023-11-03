---
title: "HW1 visualisation"
author: "Карпович Лидия"
date: "2023-11-03"
output: 
  html_document:
    keep_md: true
---



1. Загрузите датасет insurance_cost.csv

```r
insurance_cost <- read.csv("C:/Users/79214/Downloads/insurance_cost.csv",
                 stringsAsFactors = T)
```

2. Выведите гистограммы всех нумерических переменных.

```r
str(insurance_cost)
```

```
## 'data.frame':	1338 obs. of  7 variables:
##  $ age     : int  19 18 28 33 32 31 46 37 37 60 ...
##  $ sex     : Factor w/ 2 levels "female","male": 1 2 2 2 2 1 1 1 2 1 ...
##  $ bmi     : num  27.9 33.8 33 22.7 28.9 ...
##  $ children: int  0 1 3 0 0 0 1 3 2 0 ...
##  $ smoker  : Factor w/ 2 levels "no","yes": 2 1 1 1 1 1 1 1 1 1 ...
##  $ region  : Factor w/ 4 levels "northeast","northwest",..: 4 3 3 2 2 3 3 2 1 2 ...
##  $ charges : num  16885 1726 4449 21984 3867 ...
```

```r
#не включаем int переменные

hist(insurance_cost$bmi)
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
hist(insurance_cost$charges)
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

3. Нарисуйте график плотности по колонке charges. Отметьте вертикальные линии
средней и медианы на графике. Раскрасьте текст и линии средней и медианы
разными цветами. Добавьте текстовые пояснения значения средней и медианы.
Подберите тему для графика. Назовите оси.



```r
charges_mean <- round(mean(insurance_cost$charges), 1)
charges_median <- round(median(insurance_cost$charges), 1)

density_charges <- ggplot(data = insurance_cost, 
       aes(x = charges)) +
  geom_density() +
  geom_vline(aes(xintercept = charges_mean), color = 'purple') +
  geom_vline(aes(xintercept = charges_median), color = 'blue') +
  annotate("text", 
           x = charges_mean+10000,
           y = 0.00005,
           label=paste0("Mean=", charges_mean),
           color = 'red') +
  annotate("text", 
           x = charges_median-5000,
           y = 0.00007,
           label=paste0("Median=", charges_median),
           color = 'green') +
  theme_minimal() +
  ggtitle('Плотность распределения расходов стаховой компании') + 
  labs(y = 'Расоды', x = 'Плотность') +
  theme( 
    title = element_text(size = 12),
    axis.title.y = element_text(size=14)
    )
density_charges
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

4. Сделайте три box_plot по отношению переменных charges и (1) sex (2) smoker (3)
region. Подберите тему для графика. Назовите оси.

```r
charges_sex <- ggplot() +
  geom_boxplot(data = insurance_cost, 
               aes(x = charges, y = sex), 
               color = 'blue') +
  theme_minimal() +
  ggtitle('Отношение расходов в зависимости от пола')

charges_smoker <- ggplot() +
  geom_boxplot(data = insurance_cost, 
               aes(x = charges, y = smoker), 
               color = 'purple') +
  theme_minimal() +
  ggtitle('Отношение расходов в зависимости от статуса курильщика')

charges_region <- ggplot() +
  geom_boxplot(data = insurance_cost, 
               aes(x = charges, y = region), 
               color = 'pink') +
  theme_minimal() +
  ggtitle('Отношение расходов в зависимости от региона')

charges_sex
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
charges_smoker
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

```r
charges_region
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-4-3.png)<!-- -->

5. Объедините графики из заданий 3 и 4 в один так, чтобы сверху шёл один график из
задания 3, а под ним 3 графика из задания 4. Сделайте общее название для графика.

```r
general_plot <- grid.arrange(density_charges, charges_sex, charges_smoker, charges_region, ncol = 1, nrow = 4, top = "Общий график для плотности расходов и отношения пола, статуса курильщика и региона к расходам")
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
general_plot
```

```
## TableGrob (5 x 1) "arrange": 5 grobs
##   z     cells    name                grob
## 1 1 (2-2,1-1) arrange      gtable[layout]
## 2 2 (3-3,1-1) arrange      gtable[layout]
## 3 3 (4-4,1-1) arrange      gtable[layout]
## 4 4 (5-5,1-1) arrange      gtable[layout]
## 5 5 (1-1,1-1) arrange text[GRID.text.447]
```

6. Сделайте фасет графика из задания 3 по колонке region.

```r
density_charges +
  facet_grid(. ~ region) +
  theme_minimal()
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

7. Постройте scatter plot отношения переменных age и charges. Добавьте названия
осей, название графика и тему. Сделайте так, чтобы числа по оси Х отображались
14 шрифтом.

```r
scatter <- insurance_cost %>% 
  filter(age != 0 & charges != 0) %>% 
  ggplot(aes(x=age, y=charges)) + 
  geom_point(size=3) +
  theme_minimal() +
  ggtitle('Scatter plot отношения переменных age и charges') + 
  labs(y = 'charges', x = 'age') +
  theme( 
    title = element_text(size = 12),
    axis.title.x = element_text(size=14)
    )
scatter
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

8. Проведите линию тренда для предыдущего графика.

```r
scatter_trend <- scatter + 
  geom_smooth(method=lm,
              color="red", fullrange = T,
              fill="lightblue", 
              se=T
              ) +
  theme_minimal()

scatter_trend
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

9. Сделайте разбивку предыдущего графика по колонке smokers (у вас должно
получится две линии тренда для курящих и нет).

```r
scatter_trend_smoker <- insurance_cost %>% 
  filter(age != 0 & charges != 0) %>% 
  ggplot(aes(x=age, y=charges, color = smoker, fill = smoker, group = smoker)) + 
  geom_point(size=3) +
  theme_minimal() +
  ggtitle('Scatter plot отношения переменных age и charges') + 
  labs(y = 'charges', x = 'age') +
  geom_smooth(method=lm,
              fullrange = T,
              alpha = 0.2,
              se=T) +
  theme( 
    title = element_text(size = 12),
    axis.title.x = element_text(size=14)
    )
scatter_trend_smoker
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

10. Сделайте график из заданий 7-9, но вместо переменной age используйте
переменную bmi.

```r
scatter_bmi <- insurance_cost %>% 
  filter(bmi != 0 & charges != 0) %>% 
  ggplot(aes(x=bmi, y=charges, color = smoker, fill = smoker, group = smoker)) + 
  geom_point(size=3) +
  theme_minimal() +
  ggtitle('Scatter plot отношения переменных bmi и charges') + 
  labs(y = 'charges', x = 'bmi') +
  geom_smooth(method=lm,
              fullrange = T,
              alpha = 0.2,
              se=T, color = 'red') +
  theme( 
    title = element_text(size = 12),
    axis.title.x = element_text(size=14)
    )

scatter_bmi
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

11. Самостоятельно задайте вопрос No1 к данным (вопрос должен быть про какую-то
подвыборку данных). Ответьте на него построив график на подвыборке данных.
График должен содержать все основные элементы оформления (название, подписи
осей, тему и проч.). Аргументируйте выбор типа графика.


```r
#Как зависит масса тела от возраста у разных полов

scatter_mass_age <- insurance_cost %>% 
  filter(bmi != 0 & age != 0) %>% 
  ggplot(aes(x=age, y=bmi, color = sex, fill = sex, group = sex)) + 
  geom_point(size=3) +
  theme_minimal() +
  ggtitle('Scatter plot отношения массы тела от возраста у разных полов') + 
  labs(y = 'bmi', x = 'age') +
  facet_grid(. ~ sex) +
  geom_smooth(method=lm,
              fullrange = T,
              alpha = 0.2,
              se=T, color = 'red') +
  theme( 
    title = element_text(size = 12),
    axis.title.x = element_text(size=14)
    )

scatter_mass_age
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

```r
#с увеличением возраста масса тела увеличивается, у разных полов отличий нет. Был выбран scatter plot, т.к видно распределение точек, линия тренда помогает нагляднее визуализировать зависимость
```

12. Самостоятельно задайте вопрос No2 к данным (вопрос должен быть про какую-то
подвыборку данных). Ответьте на него построив график на подвыборке данных.
График должен содержать все основные элементы оформления (название, подписи
осей, тему и проч.). Аргументируйте выбор типа графика.

```r
#Затратнее ли страховать курильщиков?
insurance_cost <- insurance_cost %>% 
  mutate(
    age_group = case_when(
      age < 35 ~ "age: 21-34",
      age >= 35 & age < 50 ~ "age: 35-49",
      age >= 50 ~ "age: 50+"
    ))

ggplot() +
  geom_density(data = insurance_cost, 
               aes(x = charges, fill = smoker), 
               alpha = 0.5 
               ) +
  theme_minimal()
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
#да, стоимость затрат на курильщиков гораздо выше. Выбран график распределения плотности, потому что можно понять в каких областях стоимости лежат затраты на курильщиков и не курильщиков
```

14. (это задание засчитывается за два) Приблизительно повторите график:

```r
insurance_cost <- insurance_cost %>% 
  mutate(
    log_charges = log(charges))

insurance_cost %>% 
  filter(bmi != 0 & log_charges != 0) %>% 
  ggplot(aes(x=bmi, y=log_charges)) + 
  geom_point(size=1, alpha = 0.4, color = '#4B1D91') +
  theme_minimal() +
  ggtitle('Отношение индекса массы тела к логарифму трат по возрастным группам') + 
  labs(y = 'log(charges)', x = 'bmi') +
  facet_grid(. ~ age_group) +
  geom_smooth(method=lm,
              fullrange = T,
              alpha = 0.3,
              se=T,
              aes(color = age_group)) +
  theme( 
    title = element_text(size = 12),
    axis.title.x = element_text(size=14),
    legend.position = "bottom")
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](HW1-Карпович_files/figure-html/unnamed-chunk-13-1.png)<!-- -->









