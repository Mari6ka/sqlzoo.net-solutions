#sqlzoo.net-solutions

<h4><style="color: #00EE00">SELECT basics</style></h4>

1. 
Modify it to show the population of Germany
```sql
SELECT population 
FROM world
WHERE name = 'Germany'
```
2.
Modify it to show the name and per capita gdp: gdp/population for each country where the area is over 5,000,000 km2
```sql
SELECT name, gdp/population
FROM world
WHERE area > 5000000
```
