# IPL-AUCTION
Historical IPL data was analyzed to develop an auction strategy for a new franchise
# IPL Auction SQL Project

This repository contains a PostgreSQL project focused on analyzing IPL (Indian Premier League) auction data. The project includes SQL queries to perform various analyses on IPL matches and player performance, alongside visualizations presented in a PowerPoint presentation.

## Project Structure

- `Course Project- SQL (DS PGC).docx.pdf`: Documentation of the project, including project goals, methodology, and results.
- `ppt.pptx`: PowerPoint presentation with visualizations of the queries and their results.
- `IPL AUCTION (all queries).txt`: Text file containing all the SQL queries used in this project.

## Getting Started

### Prerequisites

- PostgreSQL installed on your system.
- Basic knowledge of SQL and database management.

### Setting Up the Database

1. **Create the Tables**: Run the following SQL commands to create the required tables:

    ```sql
    CREATE TABLE IPL_Ball (
        id INT, 
        inning INT, 
        over INT, 
        ball INT, 
        batsman VARCHAR, 
        non_striker VARCHAR, 
        bowler VARCHAR, 
        batsman_runs INT, 
        extra_runs INT, 
        total_runs INT, 
        is_wicket INT, 
        dismissal_kind VARCHAR,
        player_dismissed VARCHAR, 
        fielder VARCHAR, 
        extras_type VARCHAR,
        batting_team VARCHAR,
        bowling_team VARCHAR
    );

    CREATE TABLE IPL_matches (
        id INT, 
        city VARCHAR, 
        date DATE, 
        player_of_match VARCHAR, 
        venue VARCHAR, 
        neutral_venue INT, 
        team1 VARCHAR, 
        team2 VARCHAR, 
        toss_winner VARCHAR, 
        toss_decision VARCHAR, 
        winner VARCHAR, 
        result VARCHAR, 
        result_margin INT, 
        eliminator VARCHAR, 
        method VARCHAR, 
        umpire1 VARCHAR, 
        umpire2 VARCHAR
    );
    ```

2. **Load the Data**: Use the `COPY` command to load data from CSV files into the tables.

    ```sql
    COPY ipl_ball FROM 'path_to_your_data/IPL_Ball.csv' DELIMITER ',' CSV HEADER;
    COPY IPL_matches FROM 'path_to_your_data/IPL_matches.csv' DELIMITER ',' CSV HEADER;
    ```

## SQL Queries

The project includes a variety of SQL queries to analyze the data. Here are some examples:

1. **Top Batsmen by Strike Rate**:

    ```sql
    SELECT batsman,
        ROUND((SUM(batsman_runs) * 1.0 / COUNT(ball) * 1.0) * 100, 2) AS strike_rate,
        SUM(batsman_runs) AS batsman_total_run,
        COUNT(ball) AS batsman_faced_ball
    FROM ipl_ball
    WHERE NOT extras_type = 'wides'
    GROUP BY batsman
    HAVING COUNT(ball) >= 500
    ORDER BY strike_rate DESC
    LIMIT 10;
    ```

2. **Top Bowlers by Economy**:

    ```sql
    SELECT bowler, 
        ROUND((SUM(total_runs) * 1.0 / (COUNT(ball) / 6.0) * 1.0), 2) AS economy,
        SUM(total_runs) AS total_runs,
        COUNT(ball) AS total_balls_thrown
    FROM ipl_ball
    GROUP BY bowler
    HAVING COUNT(ball) >= 500
    ORDER BY economy ASC
    LIMIT 10;
    ```
3. **Hard-hitting players**:

    ```sql
    select batsman,
    round((sum(case when batsman_runs=4 then 4
    when batsman_runs=6 then 6 
    else 0 end)*1.0/sum(batsman_runs))*100,2) as boundries_percentage,
    sum(case when batsman_runs= 4 then 4
    when batsman_runs= 6 then 6 else 0 end) as boundary_total_run,
    sum(case when batsman_runs = 4 then 1 else 0 end) as four_by_batsman,
    sum(case when batsman_runs = 6 then 1 else 0 end) as six_by_batsman,
    sum(batsman_runs) as batsman_total_run
    from IPL_ball
    group by batsman 
    having count(distinct id)>28
    order by boundries_percentage desc limit 10;

4. **bowlers with good economy**:

    ```sql
    select * from
    (select bowler, round((sum(total_runs)*1.0/(count(ball)/6.0)*1.0),2) as economy,
    sum(total_runs) as total_runs,
    count(ball) as total_balls_throw, 
    count(ball)/6 as total_over
    from ipl_ball group by bowler) as a 
    where a.total_balls_throw >= 500
    order by a.economy desc
    limit 10;

 5. **bowlers with the best strike rate**:

    ```sql
    select * from
    (select bowler, round((count(ball)*1.0/sum(is_wicket)*1.0),2) as bowler_strike_rate,
    count(ball) as total_bowled,
    sum(is_wicket) as total_wicket
    from ipl_ball group by bowler) as a 
    where a.total_bowled >= 500
    order by a.bowler_strike_rate desc
    limit 10;  

 6. **All_rounders**:

    ```sql
    select a.bowler as player, b.strike_rate as batsman_strikerate, a.strike_rate as bowler_strikerate,
    a.total_ball from (select bowler, count(ball) as total_ball,
    round((count(ball)*1.0/sum(is_wicket)*1.0),2) as strike_rate from ipl_ball
    group by bowler having count(ball)>=300 order by strike_rate asc) as a
    inner join (select batsman, round((sum(batsman_runs)*1.0/ sum(ball)*1.0)*100,2) as strike_rate from ipl_ball 
    group by batsman
    having count(ball)>=500) as b
    on a.bowler=b.batsman
    order by batsman_strikerate desc, bowler_strikerate desc
    limit 10; 

## Visualizations

The PowerPoint presentation (`ppt.pptx`) includes visualizations for the queries and their results. Open the file to view the graphs and insights derived from the data.
[Uploading ppt.pptx…]()
## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request with your changes.
