#sqlzoo.net-solutions

<h4>SELECT basics</h4>

*1.
Modify it to show the population of Germany*
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
SELECT name, GDP/population 
FROM world 
WHERE population >=200000000
```
4.
Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
SELECT name, population/1000000  
FROM world
WHERE continent= 'South America'
ORDER BY name
```
5.
Show the name and population for France, Germany, Italy
```sql
SELECT name, population 
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
WHERE area>3000000 or population>250000000
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
 WHEN continent  IN ('Eurasia','Turkey' ) THEN 'Europe/Asia'
 WHEN continent= 'Caribbean' AND name LIKE 'B%' THEN 'North America' 
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
SELECT name 
FROM world
WHERE population > (SELECT population FROM world WHERE name='Russia')
```
2.
Show the countries in Europe with a per capita GDP greater than 'United Kingdom'
```sql
SELECT name 
FROM world
WHERE continent ='Europe'
       AND gdp/population > (SELECT gdp/population  FROM world WHERE name='United Kingdom')
```
3.
List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent 
FROM world
 WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina','Australia'))
ORDER BY name
```
4.
Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
SELECT name, population 
FROM world
WHERE population > (SELECT population FROM world WHERE name ='Canada' ) 
AND   population < (SELECT population FROM world WHERE name ='Poland' )
```
5.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, CONCAT(round(population*100/(SELECT population FROM world WHERE name='Germany'), 0), '%') '%'
FROM world 
WHERE continent='Europe'
```
6.
Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE gdp>0 AND continent='Europe')
```
7.
Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
SELECT continent, name, area 
FROM world x
WHERE area >= ALL (SELECT area FROM world y
                   WHERE y.continent=x.continent  AND area>0)
ORDER BY area DESC
```
8.
List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name 
FROM world x 
WHERE name < ALL(SELECT name FROM world y 
                 WHERE y.continent = x.continent  AND name > 0 ) 
GROUP BY continent
```
9.
Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.l
```sq
SELECT name, continent, population
FROM world x
WHERE 25000000 > ALL (SELECT population FROM world y
                    WHERE y.continent = x.continent)
ORDEER BY continent

```
10.
Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent 
FROM world x
WHERE population > ALL (SELECT population*3 FROM world y
                        WHERE y.continent=x.continent AND y.name <> x.name)
ORDEER BY name
```
<h4> SUM and COUNT</h4>
1.
Show the total population of the world
```sql
SELECT SUM(population)
FROM world
```
2.
List all the continents - just once each.
```sql
SELECT DISTINCTt(continent) 
FROM world
```
3.
Give the total GDP of Africa
```sql
SELECT SUM(gdp) 
FROM world
WHERE continent='Africa'
```
4.
How many countries have an area of at least 1000000
```sql
SELECT COUNT(name) 
FROM world
WHERE area>= 1000000
```
5.
What is the total population of ('France','Germany','Spain')
```sql
SELECT SUM(population) 
FROM world
WHERE name in ('France','Germany','Spain')
```
6.
For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name) 
FROM world
GROUP BY continent
```
7.
For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT continent, COUNT(name ) 
FROM world
WHERE population >=10000000 
GROUP BY continent
```
8.
List the continents that have a total population of at least 100 million.
```sql
SELECT continent 
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```
<h4>The JOIN operation</h4>
1.
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT matchid, player 
FROM goal  
WHERE teamid = 'GER'
```
2.
Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id, stadium, team1, team2
FROM game 
WHERE id=1012
```
3.
Modify it to show the player, teamid, stadium and mdate and for every German goal.
```sql
SELECT player, teamid, stadium, mdate 
FROM game JOIN goal ON id=matchid
WHERE teamid = 'GER'
```
4.
Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT team1, team2, player
FROM game JOIN goal ON id=matchid
WHERE player LIKE 'Mario%'
```
5.
Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT player, teamid,coach, gtime
FROM goal JOIN eteam ON teamid=id
WHERE gtime<=10
```
6.
List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT 	mdate, teamname 
FROM game JOIN eteam ON team1=eteam.id
WHERE coach='Fernando Santos' 
```
7.
List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT player 
FROM goal JOIN game ON matchid=id
WHERE stadium LIKE '%Warsaw%'
```
8.
Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT player
FROM game JOIN goal ON matchid = id 
WHERE (team1='GER' OR  team2='GER') AND teamid != 'GER'
```
9.
Show teamname and the total number of goals scored.
```sql
SELECT teamname, COUNT(teamid)
FROM eteam JOIN goal ON id=teamid
GROUP BY teamname
```
10.
Show the stadium and the number of goals scored in each stadium.
```sql
SELECT stadium, COUNT(teamid)
FROM game JOIN goal ON id=matchid
GROUP BY stadium
```
11.
For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT matchid, mdate, count(teamid)
FROM game JOIN goal ON matchid = id 
WHERE (team1 = 'POL' OR team2 = 'POL')
ORDER BY matchid
```
12.
For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
SELECT matchid, mdate, count(teamid)
FROM game JOIN goal ON id=matchid
WHERE (team1 = 'GER' OR team2 = 'GER') AND (teamid='GER')
GROUP BY matchid
```
13.
List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises. Sort your result by mdate, matchid, team1 and team2.
```sql
SELECT mdate,
    team1, SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1, 
    team2, SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate, matchid, team1, team2
ORDER BY mdate, matchid, team1, team2
```
<h4>More JOIN operations</h4>
1.
List the films where the yr is 1962 [Show id, title]
```sql
SELECT id, title
FROM movie
WHERE yr=1962
```
2.
Give year of 'Citizen Kane'.
```sql
SELECT yr 
FROM movie
WHERE title='Citizen Kane'
```
3.
List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT id, title, yr 
FROM movie 
WHERE title LIKE '%star trek%' 
ORDER BY yr
```
4.
What are the titles of the films with id 11768, 11955, 21191
```sql
SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)
```
5.
What id number does the actress 'Glenn Close' have?
```sql
SELECT id 
FROM actor
WHERE name='Glenn Close'
```
6.
What is the id of the film 'Casablanca'
```sql
SELECT id 
FROM movie
WHERE title='Casablanca'

```
7.
Obtain the cast list for 'Casablanca'.
what is a cast list?
Use movieid=11768, this is the value that you obtained in the previous question.
```sql
SELECT name 
FROM actor JOIN casting ON id = actorid 
WHERE movieid = 11768
```
8.
Obtain the cast list for the film 'Alien'
```sql
SELECT name 
FROM 
(actor JOIN casting ON actor.id=actorid)
 JOIN movie ON movie.id=movieid
WHERE title= 'Alien'
```
9.
List the films in which 'Harrison Ford' has appeared
```sql
SELECT title 
FROM 
(actor JOIN casting ON actor.id=actorid)
 JOIN movie ON movie.id=movieid
WHERE name=  'Harrison Ford'
```
10.
List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]
```sql
SELECT title
FROM (actor JOIN casting ON actor.id=actorid)
      JOIN movie ON movie.id=casting.movieid
WHERE name= 'Harrison Ford' AND  ord!=1 
```
11.
List the films together with the leading star for all 1962 films.
```sql
SELECT title, name
FROM (actor JOIN casting ON actor.id=actorid)
      JOIN movie ON movie.id=casting.movieid
WHERE yr=1962 AND ord=1
```
12.
Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```sql
SELECT yr, COUNT(title) 
FROM movie JOIN casting ON movie.id=movieid
           JOIN actor ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) 
FROM  (SELECT yr,COUNT(title) AS c 
FROM  movie JOIN casting ON movie.id=movieid
            JOIN actor   ON actorid=actor.id
 WHERE name='John Travolta'  GROUP BY yr) AS t
)
```
13.
```sql
```
14.
```sql
```
1.
```sql
```


