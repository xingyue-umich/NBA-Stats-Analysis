# :basketball: SI 564 Final project - NBA Stats Analysis

This project is designed to explore the performance of players, teams and the factors that influence their performance.

## Data collections

### Data source

- [kaggle: NBA Players Stats](https://www.kaggle.com/datasets/justinas/nba-players-data) 
- [kaggle: NBA Team Stats](https://www.kaggle.com/datasets/mharvnek/nba-team-stats-00-to-18)

### Data format

- Player

| Field Name | Description | Data Type |
| :---       |    :----   |      :--- |
| Record id      | Primary Key, the number to identify record  | Int |
| Player   |    Name of the player     | Character |
| Pos   |    Position of the player     | Character |
| Age   |    Age of the player     | Int |
| Team abv   |    Abbreviation of player's team     | Character |
| Total Rebounds  |    Average number of rebounds (offensive + denfesive) the player got per game  | Double |
| Assists   |    Average number of assists the player got per game   | Double |
| Steals   |    Average number of steals the player got per game   | Double |
| Blocks   |    Average number of blocks the player got per game | Double |
| Turnovers   |    Average number of turnovers the player made per game   | Double |
| Personal Fouls   |    Average number of personal fouls the player made per game   | Double |
| Points   |    Average points the player got per game   | Double |
| Record year   |    The year of the record   | Double |

- Team

| Field Name | Description | Data Type |
| :---       |    :----    |      :--- |
| Team id | Primary Key, the number to identify team  | Int |
| Full name  | Full team name  | Character |
| WIN%    | Winning Percentage | Double |
| PTS    | Average points the team got per game | Double |
| FG%   | Field goal percentage: The ratio of field goals made to goals attempted| Double |
| 3P%   | Three-point Field goal percentage: The ratio of three-point made to three-point attempted| Double |
| FT%   | Free Throws Field goal percentage: The ratio of free throws made to free throws attempted| Double |

- Team Name

| Field Name | Description | Data Type |
| :---       |    :----    |      :--- |
| Team id  | Primary Key, the number to identify team  | Int |
| Team abv  | Abbreviation of player's team     | Character |
| Full name  | Full team name  | Character |

### Data Analysis

The analysis will be based on the seasons from 2018 to 2021.

**Question 1:**


:question: Are the active NBA players getting younger or older from 2018 - 2021?

``` SQL
SELECT `Record year`,min(Age),avg(Age),max(Age) FROM player_record
GROUP BY `Record year`;
```

![Result](/fig/Age.png)

:bulb: In general, NBA players are getting younger on average over time.

**Question 2:**

:question: Do younger players tend to score more?

``` SQL
SELECT avg(p1.Points) as Avg_points,
CASE
    WHEN p1.Age <= 20 THEN '<=20'
    WHEN p1.Age > 20 and p1.Age <= 25  THEN '20-25'
    WHEN p1.Age > 25 and p1.Age <= 30  THEN '25-30'
    WHEN p1.Age > 30 and p1.Age <= 35  THEN '30-35'
    WHEN p1.Age > 35 THEN '>35'
END AS Age_group
FROM player_record as p1 left join player_record as p2 on p1.`Record id` = p2.`Record id`
group by Age_group
order by Avg_points desc
```

![Result](/fig/Age_Score.png)

:bulb: From the Result shown above, players between 25 - and 30 tend to score the most. It is reasonable since they are relatively young
and have certain experience.


**Question 3:**

:question: Did the players who got the top 5 highest scores help their team earn a high winning percentage?

``` SQL
SELECT p.Player, p.Points, p.`Team abv`, ts.`WIN%` as Win_pct from player_record as p
left join team_name as t on p.`Team abv` = t.`Team abv`
left join team_stat ts on t.`Team id` = ts.`Team id`
WHERE p.`Record year` = 2018 and ts.SEASON = '2018-19'
order by Points desc
limit 5;
```

![Result](/fig/Top5.png)


:bulb: Surprisingly (at least to me), 2 of the top 5 points leaders are in the teams that winning percentages are below 50%.
That is to say, team victories cannot be guaranteed by a single superstar. (:hushed: Well that starts making senses when you think of the lakers in 2021 - 2022 season)

## Built With


- SQL
- Datagrip
