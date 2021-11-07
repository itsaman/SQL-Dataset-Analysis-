use dataset;
select * from athlete_events;
select * from noc_regions;

--1 How many olympics games have been held
select count(distinct games) as total_games from athlete_events

--2 List down all Olympics games held so far.
select distinct year, season, city 
from athlete_events
order by year;
--3 Mention the total no of nations who participated in each olympics game?
select games, count(distinct noc) as tota_countries
from athlete_events
group by games
order by games

--4 Which year saw the highest and lowest no of countries participating in olympics



--5 nation participated in all the games held
select n.region, count(distinct a.games) as cg
from noc_regions n
join athlete_events a
on n.noc = a.noc
group by n.region
having count(distinct a.games) = (select top 1 count(distinct games) from athlete_events)

--6  Identify the sport which was played in all summer olympics.
with t1 as(
 select season, count(distinct games) as cg
 from athlete_events
 group by season
 having season ='summer')

select sport, temp.cg2 as no_of_games, t1.cg as total_games
from (
select sport,season, count(distinct games) as cg2
from athlete_events
group by sport, season
having season ='summer') temp, t1
where temp.cg2 = 29
order by sport asc

--7 Which Sports were just played only once in the olympics(not solved)
with t1 as(
select  sport, count(distinct games) as no_of_games 
from athlete_events
group by sport
having count(distinct games) = 1
)

select t.sport, t.rn
from
(select sport, 
count(sport) over(partition by games) as rn,
games
from athlete_events) t
where t.rn = 1

--8	Fetch the total no of sports played in each olympic games.			
select games, count(distinct sport)
from athlete_events
group by games 

--9 Fetch oldest athletes to win a gold medal
select temp.* 
from
(select name, sex, age, team, games, city, sport, event, medal,
rank()over(order by age desc) as rn
from athlete_events a
where a.medal = 'gold') temp
where temp.rn = 1


--10  Ratio of male and female athletes participated in all olympic games.
select 
count(case when sex = 'M' then 1 else 0 end)/count(1) as male,
count(case when sex = 'F' then 1 else 0 end )/count(*) as female
from athlete_events;

--11  the top 5 athletes who have won the most gold medals.
select t.name,t.team,t.me
from (
select name,medal,team,count(medal) as me
from athlete_events
group by name,medal,team
having medal = 'gold') t
order by t.me desc

--12 Fetch the top 5 athletes who have won the most medals (gold/silver/bronze).(not solved)
select t.name,t.team,t.me
from (
select name,medal,team,count(medal) as me
from athlete_events
where medal not in ('NA')
group by name,medal,team) t
order by t.me desc

--13 Fetch the top 5 most successful countries in olympics. Success is defined by no of medals won
select temp.re, temp.medal_count,
row_number() over(order by temp.medal_count desc) as rn
from 
(select nr.region as re, count(medal) as medal_count
from athlete_events ae
inner join noc_regions nr
on ae.noc = nr.noc
where medal not in ('NA')
group by nr.region) temp

--14 List down total gold, silver and bronze medals won by each country.
select nr.region,
Sum(case when medal = 'gold' then 1 else 0 end ) as gold,
sum(case when medal = 'silver' then 1 else 0 end ) as silver,
sum(case when medal = 'bronze' then 1 else 0 end) as bronze
from athlete_events ae
inner join noc_regions nr
on ae.noc= nr.noc
group by nr.region 
order by gold desc, silver desc, bronze desc

-- 15 List down total gold, silver and bronze medals won by each country corresponding to each olympic games.
select ae.games,nr.region,
Sum(case when medal = 'gold' then 1 else 0 end ) as gold,
sum(case when medal = 'silver' then 1 else 0 end ) as silver,
sum(case when medal = 'bronze' then 1 else 0 end) as bronze
from athlete_events ae
inner join noc_regions nr
on ae.noc= nr.noc
group by nr.region,ae.games 

-- 16 Identify which country won the most gold, most silver and most bronze medals in each olympic games.
with temp as (
	select ae.games,
	concat(nr.region,' - ',sum(case when medal = 'gold' then 1 else 0 end)) as gold,
	concat(nr.region,' - ',sum(case when medal = 'silver' then 1 else 0 end)) as silver,
	concat(nr.region,' - ',sum(case when medal = 'bronze' then 1 else 0 end)) as bronze
	from athlete_events ae
	inner join noc_regions nr
	on ae.noc = nr.noc
	group by games, nr.region
)
select distinct games,
first_value(gold)over(partition by games order by gold desc)
from temp



-- 17

--18 Which countries have never won gold medal but have won silver/bronze medals?
select t.region, t.gold, t.silver,t.bronze
from (
select nr.region,
Sum(case when medal = 'gold' then 1 else 0 end ) as gold,
sum(case when medal = 'silver' then 1 else 0 end ) as silver,
sum(case when medal = 'bronze' then 1 else 0 end) as bronze
from athlete_events ae
inner join noc_regions nr
on ae.noc= nr.noc
group by nr.region ) t
where t.gold = 0
order by t.silver desc, t.bronze desc

-- 19 In which Sport/event, India has won highest medals.
select top 1 ae.Sport, count(ae.medal) as me
from athlete_events ae
inner join noc_regions nr
on ae.noc = nr.noc
where nr.region = 'India' and medal not in ('NA')
group by sport
order by me desc

--20 Break down all olympic games where India won medal for Hockey and how many medals in each olympic games
select  nr.region,ae.Sport, ae.games, count(ae.medal) as me
from athlete_events ae
inner join noc_regions nr
on ae.noc = nr.noc
where nr.region = 'India' and medal not in ('NA')
group by sport, ae.games, nr.region
order by me desc




