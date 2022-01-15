SELECT basics
SELECT from world
SELECT from nobel
SELECT in SELECT

SELECT basics
1.
SELECT population FROM world
  WHERE name = 'Germany'
2.
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark')
3.
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
  
SELECT from world
1.
SELECT name, continent, population FROM world
2.
SELECT name FROM world
WHERE population >= 200000000
3.
SELECT name, gdp/population FROM world
WHERE population >= 200000000
4.
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
5.
SELECT name, population FROM world
WHERE name in ('France', 'Germany', 'Italy')
6.
select name from world
WHERE name LIKE '%United%'
7.
SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000
8.
SELECT name, population, area FROM world
WHERE area > 3000000 XOR population > 250000000
9.
SELECT name, ROUND(population/1000000,2), ROUND(gdp/1000000000,2) FROM world
WHERE continent = 'South America' 
10.
SELECT name, ROUND(gdp/population,-3)
FROM world
WHERE gdp > 1000000000000
11.
SELECT name, capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital)
12.
SELECT name, capital
FROM world
WHERE (LEFT(name,1)=LEFT(capital,1)) and not(name=capital)
13.
SELECT name
   FROM world
WHERE name LIKE '%o%a%i%u%e%'
  AND name NOT LIKE '% %'

SELECT from nobel
1.
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
2.
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
3.
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
4.
SELECT winner
FROM nobel
WHERE subject = 'Peace' and yr >= 2000  
5.
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Literature ' and yr BETWEEN 1980 AND 1989
6.
SELECT yr, subject, winner FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter',
                   'Barack Obama')
7.
SELECT winner
FROM nobel
WHERE winner LIKE 'John %'
8.
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' AND yr = 1984)
9.
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')
10.
SELECT yr, subject, winner
FROM nobel
WHERE (yr < 1910 AND subject = 'Medicine') OR (yr >= 2004 AND subject = 'Literature')

SELECT in SELECT

1.
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
2. 
SELECT name FROM world
  WHERE gdp/population >
     (SELECT gdp/population FROM world
      WHERE name='United Kingdom') and continent = 'Europe'
3.
select name, continent
from world
where continent in (select continent from world where name in ('Argentina', 'Australia'))
order by name
4.
select name, population
from world
where population > (select population from world where name = 'Canada') and population < (select population from world where name = 'Poland')
5.
select name, 
concat(round(100*population/(select population from world where name = 'Germany')),'%')
from world
where continent = 'Europe'
6.
select name
from world
where gdp > ALL (select gdp from world where gdp is not null and continent = 'Europe')
7.
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent)
8.
select s.continent, s.name
from world s
where s.name <=ALL (select s2.name from world s2 where s.continent=s2.continent)
9.
select s.name, s.continent, s.population
from world s
where 25000000 >= ALL( select s2.population from world s2 where s.continent=s2.continent and s2.population is not null)
10.