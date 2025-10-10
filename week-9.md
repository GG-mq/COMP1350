-- Week -9
-- Task 6:
-- Write an SQL statement that prints the SeasonID and the year the season aired. Use s as the Season table alias.
select distinct SeasonID, year(AirDate) as 'Season Year'
from Episode;

select distinct Episode.SeasonID, year(e.AirDate) as 'Season Year'
from Episode e, Season s
where e.SeasonID = s.SeasonID; -- join Statement

-- Observe the result. If it has duplicate results, what keyword should be added to remove identical values? Try the same query using the keyword and check your answer.

-- Now, change s.SeasonID to SeasonID. What error do you get? What does it mean?

-- Task 7:

-- Write an SQL statement that prints the ContractIDs and Names of actors. Try different inner join methods to join these tables

-- Equi join
select ContractID, ActorName
from Contract c, Actor a
where c.ActorID = a.ActorID;

-- Join on clause
SELECT c.ContractID, a.ActorName
FROM contracts c
JOIN actors a ON c.ActorID = a.ActorID;

-- Join using clause
SELECT c.ContractID, a.ActorName
FROM contracts c
JOIN actors a USING (ActorID); --  both columns must be named the same

Natural join (Do not use this unless specified to do so)
SELECT ContractID, ActorName
FROM contracts NATURAL JOIN actors; --  

-- Task 8:

-- Using a JOIN clause, write an SQL statement that prints all contract details for episodes that aired in 2013.
select c.*
from Contract c join Episode e
on c.SeasonID = e.SeasonID
and c.EpisodeNo = e.EpisodeNo
-- on (c.SeasonID, c.EpisodeNo) = (e.SeasonID, e.EpisodeNo)
where year(AirDate) = 2013;


--  Task 9:
-- Using a subquery, write an SQL statement that prints all contract details for episodes that aired in 2013.

select *
from Contract
where (SeasonID, EpisodNo) in 
			(select SeasonID, EpisodeNo
            from Episode 
            where year(AirDate) = 2013)

-- Task 10:
-- Write an SQL statement that prints the contract ID, the actor's name, the airdate, and the season's name for all contracts issued, where the episode is aired after 2013, for actors with the string ‘li’ in their names.
select ContractID, ActorName, AirDate, SeasonName
from Season s join Episode e
on s.SeasonID - e,SeasonID
join Contract c
on (c.SeasonID, c.EpisodeNo) = (e.EpisodeNo, e.SeasonID)
join Actor a
on c.ActorID = a.ActorID
where year(AirDate) > 2013
and ActorName like %li%;

-- Task 11:

-- Write a query to print the Episode details (SeasonID, EpisodeNo and the number of days since the show has aired)

select SeasonID, EpisodeNo, datediff(curDate(), AirDate) as NumOfDays
from Episode;

-- Task 12:

-- Write a query to print the names of actors who have never acted in any episodes.

select ActorName
from Actor a join Contract c
on a.ActorID = c.ActorID
where ContractID is null;
