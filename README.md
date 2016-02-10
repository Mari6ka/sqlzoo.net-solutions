#sqlzoo.net-solutions

<h4>SELECT basics</h4>

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
3.
Show the name and the population for 'Ireland', 'Iceland' and 'Denmark'.
```sql
SELECT name, population
FROM world
WHERE name IN ('Ireland', 'Iceland','Denmark')
```
4.
Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area 
FROM world
WHERE area BETWEEN 200000 AND 250000
```
<h4>SELECT from WORLD</h4>
1.
Observe the result of running a simple SQL command.
```sql
SELECT name, continent, population 
FROM world
```
2.
Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```sql
SELECT name 
FROM world
WHERE population>=200000000
```
3.
Give the name and the per capita GDP for those countries with a population of at least 200 million.
```sql
SELECT name,GDP/population 
FROM world 
WHERE population >=200000000
```
4.
Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
SELECT name,population/1000000  
FROM world
WHERE continent= 'South America'
ORDER BY name
```
5.
Show the name and population for France, Germany, Italy
```sql
SELECT name,population 
FROM world
WHERE name in ('France','Germany','Italy')
```
6.
Show the countries which have a name that includes the word 'United'
```sql
SELECT name 
FROM world
WHERE name like ('%United%')
```
4.
```sql
```
4.
```sql
```
4.
```sql
```
4.
```sql
```
4.
```sql
```
