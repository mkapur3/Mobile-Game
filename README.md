# PROJECT: MOBILE GAME 1-YEAR CUSTOMER DATA

## 1. INTRODUCTION

Our client is a mobile game company. As it is the game's one-year anniversary, our manager has asked us to investigate player retention. Of specific interest is calculating the rolling 30-day retention rate and expressing it as a fraction of the total player base at the time. Rolling 30-day retention asks the following question: Did a given player play a match 30 days after he or she joined? A player is either `retained` or `not retained` with respect to this retention metric. 

The project also demanded exploration of other important insights that the company could leverage from the analysis. For the secondary objective, we decided to analyze the retention rate by location, average age of players by location, and number of players retained by age.

## 2. ROLLING 30-DAY RETENTION RATE

We have written a query that gives us a table that has a row for every day in the game's lifespan, with the following four columns:
- `day`: The day in question
- `num_joined`: The total number of players who joined that day
- `num_retained`: Of the players who joined, how many were retained
- `fractional_retention`: The fractional retention (the third column divided by the second column)

[Link](Project%20query.txt) to full query here.

To most effectively address the question of 30-day rolling retention rate, we decided to use a subquery and join two tables player_info and match_info. In the subquery, we select  player_id and joined and then we evaluated the below `CASE` statement for each unique player:
```
 CASE  
            WHEN (MAX(m.day) - min (p.joined) ) > 30 
            THEN 1
            ELSE 0
 END AS retention_status
```
The `CASE` statement assigned a value of '1' for a player who played a match anytime after 30 days of joining and '0' for those players who did not.

Finally, in the main query, we calculated the `SUM` of players’ retention status for each day based on scores assigned to each player from the `CASE` statement. The fourth and last column of `ret_rate` displays the fractional retention rate for each day by dividing the players retained by total players joined, multiplying by a 100, and rounding to two decimals. 

We `GROUP BY` `joined` which is aliased `AS` `day_joined`.

![Rolling 30-day Retention Rate](/Screenshot%202022-02-28%20235144.jpg)

There appear to be several significant spikes in player retention (Day 55 and Day 277 are high points) and also several drops in the `ret_rate` but no clear pattern can be drawn based solely off of this data. 

When we compare the retention rate per day with the total number of players who joined that day, we can see that these peaks and declines are corresponding to the number of players. 

## 3. ROLLING 30-DAY RETENTION RATE BY LOCATION

[Link](Project%20query.txt) to full query here.

We use the same query as in Section 2 but GROUP BY and ORDER BY location to aggregate the total players joined and retained by each continent as well as to compare the retention rate across them. 

![Retention rate by location](/Screenshot%202022-03-01%20001049.jpg)

The above graph shows that retention rate is extremely consistent across all continents in the range of 61.3% - 63.6%. The game’s largest market in absolute terms is North America with 7,506 total players joining the game in its first year of service. The game’s positive performance in both attracting and retaining players across diverse geographies in its debut year is a highly encouraging sign for the corporation. 

We use the same query as in Section 2 but `GROUP BY` and `ORDER BY` `age` to aggregate the `total_players_joined` and `total_players_retained` by each age year as well as to compare the retention rate (i.e. `ret_rate` in the query across different ages). 

[Link](Project%20query.txt) to full query here.

## 4. NUMBER OF PLAYERS RETAINED BY AGE

[Link](Project%20query.txt) to full query here.

We use the same query as in Section 2 but remove the day_joined and total_players_retained columns in the main query. Instead, we add in the age column in the `SELECT` clause of the main query.
 
We `GROUP BY` `p.age` in the subquery and  we also `GROUP BY` and `ORDER BY` `age` in the main query to get the results segmented by players’ `age`. 

![Number of Players joined and Players retained by Age](/Screenshot%202022-03-01%20002627.jpg)

The game is most popular in the college demographic of 18-22year olds and the company can focus on these key target markets in their strategy and marketing going forward.

## 5. AVERAGE AGE OF PLAYERS BY LOCATION

[Link](Project%20query.txt) to full query here.

We delve deeper into the two variables we analyzed - `age` and `location`. We use the same query as in Section 3 but `SELECT` the `location` variable as a column in the main query. 

In the subquery, we add the `age` column to the `SELECT` clause and `GROUP` the results by continents.

![Average Age of Players by Location](/Screenshot%202022-03-01%20004231.jpg)

Based on the above chart, we can observe that the average age of players across different continents is extremely similar at approximately 20 years old. 

## 6. CONCLUSION

We learned a several key insights from this data set:
- The game has extremely high market potential based off of the first year's strong metrics. The age demographic that the game most appealed to was 18-22 year olds and the company would be well served to target this segment in its marketing and advertising in the coming year.
- The retention rate fluctuated a lot within the year, but in general, it averaged around 61% which is quite high for a game in its first year.
- The game attracted interest in equal measure from all continents as shown by similar metrics in total players joined/retained as well as 30-day retention rate.


