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
7.
Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.
```sql
SELECT name, population, area 
FROM world
WHERE area>3000000 or population>250000000
```
8.
Show the countries that are big by area or big by population but not both. Show name, population and area.
```sql
SELECT name, population, area 
FROM world
WHERE area>3000000  xor population>250000000
```
9.
For South America show population in millions and GDP in billions to 2 decimal places.
```sql
SELECT name, ROUND(population/1000000,2), ROUND(GDP/1000000000,2) 
FROM world
WHERE continent =  'South America'
```
10.
Show per-capita GDP for the trillion dollar countries to the nearest $1000.
```sql
SELECT name, Round(GDP/population,-3) 
FROM  world
WHERE GDP>=1000000000000
```
11.
Show the name - but substitute Australasia for Oceania - for countries beginning with N.
```sql
SELECT name, CASE
 WHEN continent='Oceania' THEN 'Australasia'
 ELSE continent END "CASE continent"
FROM world
WHERE name LIKE 'N%';
```
12.
Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B
```sql
SELECT name, CASE 
 WHEN continent IN ('Europe', 'Asia') THEN 'Eurasia'
 WHEN continent IN ('North America', 'South America', 'Caribbean') THEN 'America'
 ELSE continent END
FROM world
```
13.
Put the continents right...
Oceania becomes Australasia
Countries in Eurasia and Turkey go to Europe/Asia
Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America
Show the name, the original continent and the new continent of all countries.
```sql
SELECT name, continent, CASE 
 WHEN continent ='Oceania' THEN 'Australasia'
 WHEN continent  in ('Eurasia','Turkey' ) THEN 'Europe/Asia'
 WHEN continent= 'Caribbean' and name like 'B%' THEN 'North America' 
 WHEN continent = 'Caribbean' THEN 'South America'
 ELSE continent END
FROM world
```
<h4>SELECT from Nobel</h4>
1.
Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```
2.
Show who won the 1962 prize for Literature.
```sql
SELECT winner
FROM nobel
WHERE yr = 1962 AND subject = 'Literature'
```
3.
Show the year and subject that won 'Albert Einstein' his prize.
```sql
SELECT yr, subject 
FROM nobel
WHERE winner = 'Albert Einstein'
```
4.
Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
SELECT winner 
FROM nobel
WHERE subject='peace' AND yr>=2000
```
5.
Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT * 
FROM nobel
WHERE subject='Literature' AND yr between 1980 and 1989
```
6.
Show all details of the presidential winners:
Theodore Roosevelt
Woodrow Wilson
Jimmy Carter
```sql
SELECT * 
FROM nobel
WHERE  winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter')
```
7.
Show the winners with first name John
```sql
SELECT winner 
FROM nobel 
WHERE winner LIKE 'John%'
```
8.
Show the Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE
	subject='Physics' AND yr='1980'
OR 
	subject='Chemistry' AND yr='1984'
```
9.
Show the winners for 1980 excluding the Chemistry and Medicine
```sql
SELECT *
FROM nobel 
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')
```
10.
Show who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
SELECT *
FROM nobel
WHERE subject='Medicine' AND yr<1910
OR   subject='Literature'AND yr>=2004
ORDER BY yr
```
11.
Find all details of the prize won by PETER GRÜNBERG
```sql
SELECT * 
FROM nobel
WHERE winner LIKE 'PETER%BERG'
```
12.
Find all details of the prize won by EUGENE O'NEILL
```sql
SELECT * 
FROM nobel
WHERE winner LIKE 'EUGENE%ILL'
```
13.
List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner
```
14.
The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.
Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
```sql
SELECT winner, subject
FROM nobel
WHERE yr = 1984
ORDER BY subject IN ('Chemistry', 'Physics'), subject, winner
```
<h4>SELECT within SELECT</h4>
1.
List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name FROM world
WHERE population >
  (SELECT population 
  FROM world
  WHERE name='Russia')
```
2.
```sql
```
3.
```sql
```
4.
```sql
```
5.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
6.
```sql
```
