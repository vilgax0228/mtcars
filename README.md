## Introdução

```R
op = par(mar=c(10,4,4,2) + 0.1)  # margin formatting
barplot(mtcars$mpg, names.arg=row.names(mtcars), las=2, ylab="Fuel Efficiency in Miles per Gallon")

pairs(mtcars[1:7])

plot(y=mtcars$mpg, x=mtcars$wt, xlab="Vehicle Weight", ylab="Vehicle Fuel Efficiency in Miles per Gallon")

mt.model = lm(formula = mpg ~ wt, data = mtcars)
coef(mt.model)[2]
coef(mt.model)[1]
coef(mt.model)

plot(y=mtcars$cyl, x=mtcars$wt, xlab="Vehicle Weight", ylab="Number of Cylinders")
mt.model = lm(formula = cyl ~ wt, data = mtcars)
coef(mt.model)[2]
coef(mt.model)[1]

abline(coef=coef(mt.model), col="red")
help(abline)
```
