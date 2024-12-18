## Introdução
Um relatório é uma entidade estática que não oferece nenhuma intuição sobre como algo evolui com o tempo.
```R
# shows what a report might look like
report = par(mar=c(10,4,4,2) + 0.1)  # margin formatting
barplot(mtcars$mpg, names.arg=row.names(mtcars), las=2, ylab="Fuel Efficiency in Miles per Gallon")
```
![Rplot11](https://github.com/user-attachments/assets/8d559204-06ed-443c-bcc8-a15b050ef52d)

A model is any sort of function that has predictive power.

```R
head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

As colunas podem ser chamadas de variáveis, características (ou *features*), preditores, etc.

Podemos tentar visulizar se há algum tipo de relação entre a eficiência de combustível do carro (mpg) e qualquer outra feature.
```R
pairs(mtcars[1:7], lower.panel = NULL)
```
![Rplot12](https://github.com/user-attachments/assets/235d9ec3-12f5-4c24-9c00-eff49c3e4177)

Some of these plots are more interesting for trending purposes than others. None of the plots in cyl row, for example, look like they lend themselves easily to simple regression modeling.

This is up to the investigator to pick out what patters look interesting. We're gonna focus on mpg as a function of wt.

```R
plot(y=mtcars$mpg, x=mtcars$wt, xlab="Vehicle Weight", ylab="Vehicle Fuel Efficiency in Miles per Gallon")
```
![Rplot13](https://github.com/user-attachments/assets/5fc97125-2f18-45d8-a0f7-692315617d11)

We use the lm() function to model the value we’re interested in, called a response, against other features in our dataset:

```R
mt.model = lm(mtcars$mpg ~ mtcars$wt)
mt.model
Call:
lm(formula = mtcars$mpg ~ mtcars$wt)

Coefficients:
(Intercept)    mtcars$wt  
     37.285       -5.344

abline(mt.model, col = "blue")
```
![Rplot14](https://github.com/user-attachments/assets/9c7b38e9-7736-471c-9f01-8d2ca427b375)


We modeled the vehicle's fuel efficiency (mpg) as a function of the vehicle's weight (wt) and extracted values from that model object to use in an equation that we can write as follows:

Fuel Efficiency = -5.344 * Vehicle Weight + 37.285

Now if we wanted to know what the fuel efficiency was for any car, not just those in the dataset, all we would need to input is the weight of it, and we get a result.

Entretanto, algumas considerações devem ser feitas:

Estamos lidando com carros fabricados na década de 70, logo, tentar usar o mesmo modelo para carros modernos não parece uma boa ideia.

---
Now let's plot the fuel efficiency of the cars (mpg) in the dataset as a function of their engine size, or displacement (disp) in cubic inches:

```R
plot(y=mtcars$mpg, x=mtcars$disp, xlab="Engine Size (cubic inches)", ylab="Fuel Efficiency (milles per gallon)")
```
![Rplot15](https://github.com/user-attachments/assets/6d5c1f8f-067e-400d-9865-9e6ef9e55182)

We can see from the plot that the fuel efficiency decreases as the size of the engine increases.

However, if you have some new engine for which you want to know the efficency, you need to build a linear model.

```R
model = lm(mtcars$mpg ~ mtcars$disp)
coef(model)
(Intercept) mtcars$disp 
29.59985476 -0.04121512

abline(model)
```
![Rplot16](https://github.com/user-attachments/assets/65561aa6-69d3-4a91-97d4-180ec2031595)

Regression modeling is of the form: y = mx + b. So the model looks like the following:

Fuel efficiency = -0.041 * Engine size + 29.599

You can put any input for the engine size and get a value out. Let's look at the fuel efficiency for a car with a 200-cubic-inch displacement:
```R
coef(model)[2] * 200 + coef(model)[1]
mtcars$disp 
   21.35683
```

## Training and Testing of Data

We have a mdoel that we can use to predict future values. Yet, we know nothing about how accurate the model is for the moment.

One way to determine the accuracy is to look at the R-squared value from the model:
```R
summary(model)
Call:
lm(formula = mtcars$mpg ~ mtcars$disp)

Residuals:
    Min      1Q  Median      3Q     Max 
-4.8922 -2.2022 -0.9631  1.6272  7.2305 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 29.599855   1.229720  24.070  < 2e-16 ***
mtcars$disp -0.041215   0.004712  -8.747 9.38e-10 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.251 on 30 degrees of freedom
Multiple R-squared:  0.7183,	Adjusted R-squared:  0.709 
F-statistic: 76.51 on 1 and 30 DF,  p-value: 9.38e-10
```
The accuarcy parameter that's most important to us now is the Adjusted R-squared value.

The R-squared value tells us how linearly correlated the data is; the closer the value is to 1, the more likely the model output is governed by data that's almost exactly a straight line with some kind of slope value to it.

For low numbers of features the adjusted and multiple R-squared values are basically the same thing.

For models that have many features, we want to use multiple R-squared values, instead, because it will give a more accurate assessment of the model error if we have many dependent features instead of just one.





