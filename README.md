# PPM-Int-Cup-standings

Final standings of international cups in the PPM world (only Europe in soccer, as the data were originally collected to review Ukrainian results).

Examples of queries: 

-- List of all finals with Ukrainian teams

SELECT
  t.name teamname
  , r.sport
  , r.season
  , r.cup
  , r.pos
FROM intcup_playoffranking r
INNER JOIN intcup_teams t ON t.team_id = r.team_id
INNER JOIN intcup_countries c ON c.country_id = t.country_id
WHERE c.name = 'Ukraine'
AND r.pos < 3
ORDER BY sport ASC, season ASC

-- Ukrainian presence in top-16 of international cups in hockey

SELECT 
	t.name
	, r.season
	, r.cup
	, r.pos
FROM intcup_playoffranking r
INNER JOIN intcup_teams t ON t.team_id = r.team_id
INNER JOIN intcup_countries c ON c.country_id = t.country_id
WHERE r.sport = 'hockey' AND c.name = 'Ukraine'
AND r.pos < 17
ORDER BY r.pos ASC, r.season DESC

-- Ranking of countries by titles

SELECT 
	c.name country
	, COUNT(\*) titles
	, SUM(IF(r.sport = 'hockey', 1, 0)) titles_hockey
	, SUM(IF(r.sport = 'soccer', 1, 0)) titles_soccer
	, SUM(IF(r.sport = 'handball', 1, 0)) titles_handball
	, SUM(IF(r.sport = 'basketball', 1, 0)) titles_basketball
	, SUM(IF(r.sport = 'hockey' AND r.cup = 1, 1, 0)) titles_hockey_cl
	, SUM(IF(r.sport = 'soccer' AND r.cup = 1, 1, 0)) titles_soccer_cl
	, SUM(IF(r.sport = 'handball' AND r.cup = 1, 1, 0)) titles_handball_cl
	, SUM(IF(r.sport = 'basketball' AND r.cup = 1, 1, 0)) titles_basketball_cl
	, SUM(IF(r.sport = 'hockey' AND r.cup = 2, 1, 0)) titles_hockey_cwc
	, SUM(IF(r.sport = 'soccer' AND r.cup = 2, 1, 0)) titles_soccer_cwc
	, SUM(IF(r.sport = 'handball' AND r.cup = 2, 1, 0)) titles_handball_cwc
	, SUM(IF(r.sport = 'basketball' AND r.cup = 2, 1, 0)) titles_basketball_cwc
FROM intcup_playoffranking r
INNER JOIN intcup_teams t ON t.team_id = r.team_id
INNER JOIN intcup_countries c ON c.country_id = t.country_id
WHERE r.pos = 1
GROUP BY country
ORDER BY titles DESC, country ASC
