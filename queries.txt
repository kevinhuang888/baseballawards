/* Kevin Huang
		Jul 10, 2022
    PostgreSQL "Baseball Awards" */


/* 
	Heaviest Hitters
  ~ Team with highest average batters in a given year
*/
SELECT teams.name, AVG(people.weight)
FROM people,teams,batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2 DESC;

/* 
	Shortest Sluggers
  ~ Team with smallest average height of batters in a given year
*/
SELECT teams.name, AVG(people.height)
FROM people, teams, batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2;

/* 
	Biggest Spenders
  ~ Team with largest total salary of all players in a given year
*/
SELECT teams.name, SUM(salaries.salary)
FROM salaries
JOIN teams
ON salaries.yearid = teams.yearid AND salaries.teamid = teams.teamid
GROUP BY 1
ORDER BY 2 DESC;

/* 
	Most Bang For Their Buck In 2010
  ~ Smallest cost per win (total salary / wins in 2010)
*/
SELECT teams.name, ROUND(SUM(salaries.salary)/SUM(teams.w)) AS costperwin
FROM salaries
JOIN teams
ON salaries.yearid = teams.yearid AND salaries.teamid = teams.teamid
GROUP BY 1
ORDER BY 2;

/* 
	Heaviest Hitters
  ~ Team with highest average batters in a given year
*/
SELECT teams.name, AVG(people.weight)
FROM people,teams,batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2 DESC;

/* 
	Shortest Sluggers
  ~ Team with smallest average height of batters in a given year
*/
SELECT teams.name, AVG(people.height)
FROM people, teams, batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2;

/* 
	Priciest Starter
  ~ Pitcher who cost the most money as the starting pitcher (must start at least 10 games)
*/
SELECT people.namefirst, people.namelast, ROUND(salaries.salary/pitching.gs) AS costpergame, salaries.yearid, pitching.gs
FROM salaries
JOIN pitching
ON salaries.playerid = pitching.playerid AND salaries.yearid = pitching.yearid AND salaries.teamid = pitching.teamid
JOIN people
ON salaries.playerid = people.playerid
WHERE pitching.gs >= 10
ORDER BY salaries.salary/pitching.gs DESC;

/* 
	Heaviest Hitters
  ~ Team with highest average batters in a given year
*/
SELECT teams.name, AVG(people.weight)
FROM people,teams,batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2 DESC;

/* 
	Shortest Sluggers
  ~ Team with smallest average height of batters in a given year
*/
SELECT teams.name, AVG(people.height)
FROM people, teams, batting
WHERE people.playerid = batting.playerid AND batting.teamid = teams.teamid
GROUP BY 1
ORDER BY 2;

/* 
	Biggest Spenders
  ~ Team with largest total salary of all players in a given year
*/
SELECT teams.name, SUM(salaries.salary)
FROM salaries
JOIN teams
ON salaries.yearid = teams.yearid AND salaries.teamid = teams.teamid
GROUP BY 1
ORDER BY 2 DESC;

/* 
	Most Bang For Their Buck In 2010
  ~ Smallest cost per win (total salary / wins in 2010)
*/
SELECT teams.name, ROUND(SUM(salaries.salary)/SUM(teams.w)) AS costperwin
FROM salaries
JOIN teams
ON salaries.yearid = teams.yearid AND salaries.teamid = teams.teamid
GROUP BY 1
ORDER BY 2;


/* 
	Bean Machine 
  ~ Pitcher most likely to hit a batter by a pitch (highest hbp)
*/
SELECT people.namefirst, people.namelast, pitching.hbp
FROM pitching
JOIN people
ON pitching.playerid = people.playerid
WHERE pitching.hbp IS NOT NULL
ORDER BY 3 DESC;

/* 
	Canadian Ace 
  ~ Pitcher with lowest ERA (earned run average) for a team whose stadium is in Canada
*/
SELECT people.namefirst, people.namelast, MIN(pitching.era), teams.park, teams.name, pitching.yearid
FROM pitching
JOIN teams
ON pitching.teamid = teams.teamid
JOIN parks
ON teams.park = parks.parkname
JOIN people
ON pitching.playerid = people.playerid
WHERE parks.country = 'CA' AND pitching.gs >= 10
GROUP BY 1,2,4,5,6
ORDER BY 3;

/* 
	Worst of the Best
  ~ Hall of Fame batters with the least home runs
*/
SELECT people.namefirst, people.namelast, MIN(batting.hr), halloffame.ballots, halloffame.needed, halloffame.votes, halloffame.yearid
FROM batting
JOIN halloffame
ON batting.playerid = halloffame.playerid
JOIN people
ON batting.playerid = people.playerid
WHERE halloffame.inducted = 'Y' AND votes IS NOT NULL AND batting.g >=10
GROUP BY 1, 2, 4, 5, 6, 7
ORDER BY 3;
