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

## Visualizations

The PowerPoint presentation (`ppt.pptx`) includes visualizations for the queries and their results. Open the file to view the graphs and insights derived from the data.

## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
"""

# Save the README.md content to a file
file_path = "/mnt/data/README.md"
with open(file_path, "w") as file:
    file.write(readme_content)

file_path
