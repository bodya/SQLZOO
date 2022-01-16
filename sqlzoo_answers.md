Решения sql заданий на sqlzoo.net

1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)

## SELECT basics
1.
```sql
SELECT population FROM world
  WHERE name = 'Germany'
```  
2.
```sql
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark')
```
3.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

## SELECT from world

1.
```sql
SELECT name, continent, population FROM world
```
2.
```sql
SELECT name FROM world
WHERE population >= 200000000
```
3.
```sql
SELECT name, gdp/population FROM world
WHERE population >= 200000000
```
4.
```sql
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
```
5.
```sql
SELECT name, population FROM world
WHERE name in ('France', 'Germany', 'Italy')
```
6.
```sql
select name from world
WHERE name LIKE '%United%'
```
7.
```sql
SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000
```
8.
```sql
SELECT name, population, area FROM world
WHERE area > 3000000 XOR population > 250000000
```
9.
```sql
SELECT name, ROUND(population/1000000,2), ROUND(gdp/1000000000,2) FROM world
WHERE continent = 'South America' 
```
10.
```sql
SELECT name, ROUND(gdp/population,-3)
FROM world
WHERE gdp > 1000000000000
```
11.
```sql
SELECT name, capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital)
```
12.
```sql
SELECT name, capital
FROM world
WHERE (LEFT(name,1)=LEFT(capital,1)) and not(name=capital)
```
13.
```sql
SELECT name
   FROM world
WHERE name LIKE '%o%a%i%u%e%'
  AND name NOT LIKE '% %'
```

## SELECT from nobel

1.
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
2.
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```
3.
```sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```
4.
```sql
SELECT winner
FROM nobel
WHERE subject = 'Peace' and yr >= 2000  
```
5.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Literature ' and yr BETWEEN 1980 AND 1989
```
6.
```sql
SELECT yr, subject, winner FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter',
                   'Barack Obama')
```
7.
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John %'
```
8.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' AND yr = 1984)
```
9.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')
```
10.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (yr < 1910 AND subject = 'Medicine') OR (yr >= 2004 AND subject = 'Literature')
```

## SELECT in SELECT

1.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
2. 
```sql
SELECT name FROM world
  WHERE gdp/population >
     (SELECT gdp/population FROM world
      WHERE name='United Kingdom') and continent = 'Europe'
```
3.
```sql
select name, continent
from world
where continent in (select continent from world where name in ('Argentina', 'Australia'))
order by name
```
4.
```sql
select name, population
from world
where population > (select population from world where name = 'Canada') and population < (select population from world where name = 'Poland')
```
5.
```sql
select name, 
concat(round(100*population/(select population from world where name = 'Germany')),'%')
from world
where continent = 'Europe'
```
6.
```sql
select name
from world
where gdp > ALL (select gdp from world where gdp is not null and continent = 'Europe')
```
7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent)
```
8.
```sql
select s.continent, s.name
from world s
where s.name <=ALL (select s2.name from world s2 where s.continent=s2.continent)
```
9.
```sql
select s.name, s.continent, s.population
from world s
where 25000000 >= ALL( select s2.population from world s2 where s.continent=s2.continent and s2.population is not null)
```
10.
```sql
select s.name, s.continent
from world s
where s.population >= ALL( select s2.population*3 from world s2 where s.continent=s2.continent and s2.population is not null and s.name != s2.name)
```

## SUM and COUNT

1.
```sql
SELECT SUM(population)
FROM world
```
2. 
```sql
select distinct continent
from world
```
3.
```sql
select sum(gdp)
from world
where continent= 'Africa'
```
4.
```sql
select
count(distinct name)
from world
where area > 1000000
```
5.
```sql
select 
sum(population)
from world
where name in ('Estonia', 'Latvia', 'Lithuania')
```
6.
```sql
select continent, count(name)
from world
group by continent
```
7.
```sql
select continent, count(name)
from world
where population > 10000000
group by continent
```
8.
```sql
select continent, count(name)
from world
where population > 10000000
group by continent
```
## JOIN

1.
```sql
SELECT matchid, player FROM goal 
  WHERE teamid LIKE '%GER%'
```
2. 
```sql
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012
```
3.
```sql
SELECT player, teamid, stadium, mdate
  FROM game JOIN goal ON (game.id=goal.matchid)
WHERE teamid = 'GER'
```
4.
```sql
SELECT team1, team2, player
  FROM game JOIN goal ON (game.id=goal.matchid)
WHERE player LIKE 'Mario%'
```
5.
```sql
SELECT player, teamid, coach, gtime
  FROM goal 
JOIN eteam ON (goal.teamid=eteam.id)
 WHERE gtime<=10
```
6.
```sql
SELECT mdate, teamname
FROM game
JOIN eteam ON (game.team1=eteam.id)
WHERE coach = 'Fernando Santos'
```
7.
```sql
SELECT
player
FROM goal
JOIN game ON (game.id=goal.matchid)
WHERE stadium = 'National Stadium, Warsaw'
```
8.
```sql
SELECT DISTINCT player
FROM goal
WHERE matchid IN 
(SELECT id
  FROM game
    WHERE 'GER' IN (team1, team2))
AND teamid != 'GER'
```
9.
```sql
SELECT teamname, COUNT(player)
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname
```
10.
```sql
SELECT stadium, SUM(count_goal)
FROM game
JOIN (SELECT matchid, COUNT(matchid) AS count_goal
FROM goal GROUP BY matchid) AS t ON t.matchid=game.id
GROUP BY stadium
```
11.
```sql
SELECT id AS matchid, mdate AS date, sum_goal
FROM game
JOIN (SELECT matchid, COUNT(matchid) as sum_goal FROM goal GROUP BY matchid) AS t ON t.matchid=game.id
WHERE 'POL' IN (team1, team2)
```
12.
```sql
SELECT matchid, mdate, count_goal
FROM game
JOIN (SELECT matchid, COUNT(teamid) AS count_goal FROM goal WHERE teamid = 'GER' GROUP BY matchid) AS t ON t.matchid=game.id
```
13.
```sql
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate, team1, team2
ORDER BY mdate, matchid, team1, team2
```
