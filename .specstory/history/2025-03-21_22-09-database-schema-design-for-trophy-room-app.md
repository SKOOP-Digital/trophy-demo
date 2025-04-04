<!-- Generated by SpecStory -->

# Database Schema Design for Trophy Room App (2025-03-21 18:09:22)

_**User**_

I need to make the database tables for this app that we will use instead of data.json

Please give me all the tables and header names needed for this app.

---

_**Assistant**_

<think>
Let me examine the data.json file and the application structure to understand what tables and fields we need to create for a database implementation. The application appears to be a virtual trophy room for Saginaw Schools, showcasing sports achievements, teams, athletes, and trophy models.

Looking at data.json and the app's functionality, I can identify these key entities:

1. Schools
2. Sports
3. Teams/Achievements (by year)
4. Roster/Players
5. D1 Athletes
6. Pro Athletes
7. Sport Categories

Let me break down each entity and its relationships:

1. Schools - Contains basic school information
2. Sports - Sports for each school 
3. Teams - Teams for each sport by year with achievements
4. Players - Players on each team
5. Athletes - Special athletes (D1, Pro) that graduated from schools
6. Trophy Models - 3D models of trophies

Now I'll define database tables and their columns based on these entities:
</think>

# Database Schema for Saginaw Schools Trophy Room

Based on the current application structure, I'll design a relational database schema to replace the data.json file. This schema will support all the functionality shown in the app while providing a more scalable and maintainable data structure.

## Tables and Fields

### 1. Schools

```
Table: schools
- id (PRIMARY KEY)
- name
- primary_color
- secondary_color
- accent_color
- background_video_url
```

### 2. SportCategories

```
Table: sport_categories
- id (PRIMARY KEY)
- name
```

### 3. Sports

```
Table: sports
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- sport_category_id (FOREIGN KEY references sport_categories.id)
- gender (enum: 'mens', 'womens', 'coed')
```

### 4. Teams

```
Table: teams
- id (PRIMARY KEY)
- sport_id (FOREIGN KEY references sports.id)
- year
- achievement
- team_photo_url
- trophy_model_url
```

### 5. Players

```
Table: players
- id (PRIMARY KEY)
- team_id (FOREIGN KEY references teams.id)
- name
- number
- position
- photo_url
```

### 6. PlayerStats

```
Table: player_stats
- id (PRIMARY KEY)
- player_id (FOREIGN KEY references players.id)
- stat_name
- stat_value
```

### 7. D1Athletes

```
Table: d1_athletes
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- name
- graduation_year
- sport
- college
- photo_url
```

### 8. ProAthletes

```
Table: pro_athletes
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- name
- graduation_year
- sport
- professional_team
- photo_url
```

## Relationships

1. A School has many Sports
2. A Sport belongs to a School and a SportCategory
3. A Sport has many Teams (by year)
4. A Team has many Players
5. A Player has many PlayerStats
6. A School has many D1Athletes and ProAthletes

## Alternative Design: Athletes Table

For a more normalized approach, you could replace the separate D1Athletes and ProAthletes tables with:

```
Table: athletes
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- name
- graduation_year
- sport
- type (enum: 'd1', 'pro')
- college (NULL for pro athletes)
- professional_team (NULL for d1 athletes)
- photo_url
```

This schema provides a solid foundation for the application while allowing for future expansion. It eliminates the nested structure from the JSON file and creates proper relationships between entities. Player statistics are stored in a separate table using a key-value approach to accommodate different statistical fields across various sports.

Would you like me to make any adjustments to this schema or provide additional details for implementation?

---

_**User**_

Ok, let's go with the alternative design. give me the full table list and headers

---

_**Assistant**_

<think>
I'll provide a comprehensive list of all tables and their headers using the alternative design where we use a single athletes table instead of separate D1Athletes and ProAthletes tables.
</think>

# Complete Database Schema with Alternative Athletes Design

Here's the complete list of tables and their headers for the Saginaw Schools Trophy Room application:

## 1. Schools

```
Table: schools
- id (PRIMARY KEY)
- name
- primary_color
- secondary_color
- accent_color
- background_video_url
```

## 2. SportCategories

```
Table: sport_categories
- id (PRIMARY KEY)
- name
```

## 3. Sports

```
Table: sports
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- sport_category_id (FOREIGN KEY references sport_categories.id)
- gender (enum: 'mens', 'womens', 'coed')
```

## 4. Teams

```
Table: teams
- id (PRIMARY KEY)
- sport_id (FOREIGN KEY references sports.id)
- year
- achievement
- team_photo_url
- trophy_model_url
```

## 5. Players

```
Table: players
- id (PRIMARY KEY)
- team_id (FOREIGN KEY references teams.id)
- name
- number
- position
- photo_url
```

## 6. PlayerStats

```
Table: player_stats
- id (PRIMARY KEY)
- player_id (FOREIGN KEY references players.id)
- stat_name
- stat_value
```

## 7. Athletes (Combined D1 and Pro Athletes)

```
Table: athletes
- id (PRIMARY KEY)
- school_id (FOREIGN KEY references schools.id)
- name
- graduation_year
- sport
- type (enum: 'd1', 'pro')
- college (NULL for pro athletes)
- professional_team (NULL for d1 athletes)
- photo_url
```

This schema provides a unified approach for both D1 and professional athletes while maintaining all the necessary relationships and data points from the original JSON structure. The use of a type field in the athletes table allows for easy filtering while keeping the data structure more normalized.

The schema supports all the functionality seen in the application and provides a solid foundation for future enhancements and scaling.

---

_**User**_

Can you minimize the numner of tables as much as possible? to eliminate as many primary/foreign keys as possible. to make it more easy to manually enter data into the tables without having to keep track of too many keys. ideally none.

---

_**Assistant**_

<think>
The user wants to simplify the database schema I proposed earlier to minimize the number of tables and reduce the reliance on primary/foreign keys. They specifically want to make manual data entry easier by eliminating the need to track many keys.

Looking at the application structure from the files provided (index.html and generate_trophy.html), I can see it's a fairly straightforward trophy showcase application for schools, their sports teams, achievements, and athletes.

Let's think about ways to simplify the schema while maintaining the necessary data relationships:

1. Combine related tables where possible
2. Use natural keys instead of surrogate keys where feasible
3. Denormalize some relationships to reduce table count
4. Consider a flatter structure for some hierarchical data

Here's how I might simplify the previous schema:
</think>

# Simplified Database Schema for Trophy Room App

Based on your request to minimize the number of tables and reduce the complexity of primary/foreign key relationships, I've redesigned the database schema to be more straightforward for manual data entry.

## Simplified Schema with Minimal Tables

### 1. Schools Table

```
Table: schools
- id (PRIMARY KEY, could be the same as the "saginaw-high" format in JSON)
- name
- primary_color
- secondary_color
- accent_color
- background_video_url
```

### 2. Teams Table

```
Table: teams
- id (PRIMARY KEY, could be auto-increment)
- school_id (matches schools.id but not enforced as FK)
- sport_name (e.g., "Men's Basketball", "Football")
- year (e.g., "2023")
- achievement (e.g., "District Champions")
- team_photo_url
- trophy_model_url
```

### 3. Players Table

```
Table: players
- id (PRIMARY KEY, could be auto-increment)
- team_id (matches teams.id but not enforced as FK)
- name
- number
- position
- photo_url
- points_per_game (nullable)
- assists (nullable)
- rebounds (nullable)
- passing_yards (nullable)
- touchdowns (nullable)
- completion_percentage (nullable)
- other_stat_name (nullable)
- other_stat_value (nullable)
```

### 4. Athletes Table

```
Table: athletes
- id (PRIMARY KEY, could be auto-increment)
- school_id (matches schools.id but not enforced as FK)
- name
- graduation_year
- sport
- type (either 'd1' or 'pro')
- college (NULL for pro athletes)
- professional_team (NULL for d1 athletes)
- photo_url
```

## Benefits of This Approach:

1. Reduced from 7 tables to just 4 tables
2. No enforced foreign key constraints (but maintains ID relationships)
3. Common stats for players included directly in the players table
4. Sport categories are stored as plain text in the teams table
5. Simpler data entry process

## Considerations:

1. This design sacrifices some normalization for simplicity
2. Player statistics are partially denormalized into the player table
3. The most common stats are given dedicated columns
4. You can add a free-form "other_stat" field for sport-specific stats

## Data Entry Process:

1. First, add a school to the schools table
2. Next, add teams for that school (manually referencing the school's ID)
3. Then add players for a team (manually referencing the team's ID)
4. Finally, add notable athletes (manually referencing the school's ID)

This simplified structure makes manual data entry much easier while preserving the core functionality and relationships needed for the application.

---

_**User**_

can it be made even less tables with keeping the same data

---

_**Assistant**_

<think>
Let's see if I can minimize the tables even further while maintaining all the data from the original schema. To do this, I'll need to think about further denormalization while ensuring we don't lose any functionality.

Current minimal tables:
1. Schools
2. Teams 
3. Players
4. Athletes

To reduce further, I need to consider what tables could potentially be combined:

1. Players and Teams could potentially be merged, with team details repeated for each player
2. Athletes could potentially be merged into a combined structure with Players

Let's explore a potential two-table solution:
1. Schools
2. Members (combining Teams, Players, and Athletes)

Or even an extreme one-table solution where everything is in a single table, but this would create many nullable fields and might be inefficient.
</think>

# Ultra-Simplified Database Schema for Trophy Room App

I can further reduce the schema to just two tables while maintaining all the data functionality:

## 1. Schools Table

```
Table: schools
- id (PRIMARY KEY)
- name
- primary_color
- secondary_color
- accent_color
- background_video_url
```

## 2. Achievements Table (Combined Everything)

```
Table: achievements
- id (PRIMARY KEY)
- school_id (matches schools.id)
- type (enum: 'team', 'd1_athlete', 'pro_athlete')
- sport_name (e.g., "Men's Basketball", "Football")
- year (e.g., "2023")
- achievement (e.g., "District Champions")
- team_photo_url (NULL for individual athletes)
- trophy_model_url (NULL for individual athletes)
- name (player name or athlete name)
- number (NULL for individual athletes)
- position (NULL for individual athletes)
- photo_url
- graduation_year (NULL for team entries)
- college (NULL except for d1 athletes)
- professional_team (NULL except for pro athletes)
- points_per_game (NULL for non-basketball players)
- assists (NULL for non-basketball players)
- rebounds (NULL for non-basketball players)
- passing_yards (NULL for non-football players)
- touchdowns (NULL for non-football players)
- completion_percentage (NULL for non-football players)
- other_stat_name (NULL if not applicable)
- other_stat_value (NULL if not applicable)
```

## How This Works:

1. **Schools** - Contains basic school information
2. **Achievements** - A "polymorphic" table that stores three types of records:
   - Team achievements (with associated players as separate records)
   - D1 athlete achievements
   - Pro athlete achievements

## Data Organization:

### For Teams and Players:
1. Create one record with `type = 'team'` containing team details
   - This record will have `name = null` since it represents a team not an individual
   - Fill in sport_name, year, achievement, team_photo_url, trophy_model_url
2. Create multiple records with `type = 'team'` for each player
   - These will share the same school_id, sport_name, and year
   - Player statistics go in their respective columns

### For D1 Athletes:
- Create records with `type = 'd1_athlete'`
- Fill in name, graduation_year, sport_name, college, photo_url

### For Pro Athletes:
- Create records with `type = 'pro_athlete'`
- Fill in name, graduation_year, sport_name, professional_team, photo_url

## Benefits:

1. Just two tables total
2. Eliminates the need to track multiple foreign keys
3. Maintains all the data from the original JSON structure
4. Simple querying by type to get different categories of data

## Considerations:

1. This is a highly denormalized design optimized for simplicity
2. Many fields will be NULL depending on the record type
3. Data integrity must be maintained at the application level
4. Filters by "type" will be necessary for most queries

This is the absolute minimum number of tables while preserving all the data from the original JSON structure.

---

_**User**_

do the players and athletes tables have to be separate?

---

_**Assistant**_

<think>
Looking at the data model, I can see an opportunity to further simplify by combining the players and athletes tables. Both represent individuals associated with a school and a sport, just in different contexts.

Let me analyze what's in each:

Players:
- Associated with a team (school, sport, year)
- Have stats specific to their sport
- Have attributes like position, number

Athletes:
- Associated with a school
- Can be D1 or Pro
- Have attributes like graduation year, college/professional team

There's definitely overlap here, and we could simplify to a single table for all individuals (players and notable athletes).
</think>

# Combined Players and Athletes into a Single Table

You're right - we can combine players and athletes tables into a single table to further reduce the schema while keeping all the data. Here's an updated schema with just 3 tables:

## 1. Schools Table

```
Table: schools
- id (PRIMARY KEY)
- name
- primary_color
- secondary_color
- accent_color
- background_video_url
```

## 2. Teams Table

```
Table: teams
- id (PRIMARY KEY)
- school_id (matches schools.id)
- sport_name (e.g., "Men's Basketball", "Football")
- year (e.g., "2023")
- achievement (e.g., "District Champions")
- team_photo_url
- trophy_model_url
```

## 3. Individuals Table (All Players & Athletes)

```
Table: individuals
- id (PRIMARY KEY)
- school_id (matches schools.id)
- team_id (matches teams.id, NULL for standalone athletes)
- type (enum: 'player', 'd1_athlete', 'pro_athlete')
- name
- photo_url
- number (NULL for non-players)
- position (NULL for non-players)
- graduation_year (NULL for active players)
- sport (NULL for players as it's in the team record)
- college (NULL except for d1 athletes)
- professional_team (NULL except for pro athletes)
- points_per_game (NULL for non-basketball players)
- assists (NULL for non-basketball players)
- rebounds (NULL for non-basketball players)
- passing_yards (NULL for non-football players)
- touchdowns (NULL for non-football players)
- completion_percentage (NULL for non-football players)
- other_stat_name (NULL if not applicable)
- other_stat_value (NULL if not applicable)
```

## How This Works:

1. **Schools** - Contains basic school information
2. **Teams** - Contains team achievements by school, sport, and year
3. **Individuals** - Contains all people (players, D1 athletes, and pro athletes)

## Data Organization:

### For Current Team Players:
- Create record with `type = 'player'`
- Link to a team with team_id
- Fill in player details (name, number, position, stats)

### For D1 Athletes:
- Create record with `type = 'd1_athlete'`
- Set team_id to NULL (they're not on a current team)
- Fill in name, graduation_year, sport, college, photo_url

### For Pro Athletes:
- Create record with `type = 'pro_athlete'`
- Set team_id to NULL (they're not on a current team)
- Fill in name, graduation_year, sport, professional_team, photo_url

## Benefits:

1. Only 3 tables total
2. Players and athletes are in the same table, simplifying queries about people
3. Maintains all the data from the original JSON structure
4. Simple querying by type to get different categories of individuals

This combined approach gives you the best balance of simplicity for manual data entry while still maintaining enough structure to properly separate teams from individuals.

---

_**User**_

ok I went with 

Schools: name	primary_color	secondary_color	accent_color	media_url
Team: school_id	sport_name	year	achievement	team_photo_url	trophy_model_url	media_url
Individuals: school_id	team_id	type	name	media_url	number	position	graduation_year	sport	college	professional_team	points_per_game	assists	rebounds	passing_yards	touchdowns	completion_percentage	other_stat_name	other_stat_value

I will use the names as the ids

the individuals wont need ids

Please give me all the sample data in this format in 3 sheets to import as csvs

---

_**Assistant**_

<think>
I need to create sample data in CSV format for 3 sheets:
1. Schools
2. Team
3. Individuals

Based on the provided data.json and the schema the user has decided on:

For Schools:
- name (Primary Key)
- primary_color
- secondary_color
- accent_color
- media_url

For Team:
- school_id (matches Schools.name)
- sport_name
- year
- achievement
- team_photo_url
- trophy_model_url
- media_url

For Individuals:
- school_id (matches Schools.name)
- team_id (reference to Team - constructed as school_id-sport_name-year)
- type ('player', 'd1_athlete', 'pro_athlete')
- name
- media_url
- number
- position
- graduation_year
- sport
- college
- professional_team
- points_per_game
- assists
- rebounds
- passing_yards
- touchdowns
- completion_percentage
- other_stat_name
- other_stat_value

I'll create sample data based on what's in the JSON file that was shared.
</think>

# Sample CSV Data for Saginaw Schools Trophy Room

Here are the sample data sheets formatted for CSV import, based on your database structure:

## 1. Schools.csv

```csv
name,primary_color,secondary_color,accent_color,media_url
Saginaw High,#000080,#FFD700,#000000,placeholder_url
Arthur Hill,#000080,#FFD700,#000000,placeholder_url
Saginaw United,#000080,#FFD700,#000000,placeholder_url
```

## 2. Team.csv

```csv
school_id,sport_name,year,achievement,team_photo_url,trophy_model_url,media_url
Saginaw High,Men's Basketball,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Arthur Hill,Women's Basketball,2022,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Saginaw United,Football,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
```

## 3. Individuals.csv

```csv
school_id,team_id,type,name,media_url,number,position,graduation_year,sport,college,professional_team,points_per_game,assists,rebounds,passing_yards,touchdowns,completion_percentage,other_stat_name,other_stat_value
Saginaw High,Saginaw High-Men's Basketball-2023,player,John Smith,placeholder_url,23,Guard,,,,,18.5,5.2,4.1,,,,, 
Arthur Hill,Arthur Hill-Women's Basketball-2022,player,Sarah Johnson,placeholder_url,11,Forward,,,,,15.8,3.2,8.4,,,,, 
Saginaw United,Saginaw United-Football-2023,player,Marcus Williams,placeholder_url,7,Quarterback,,,,,,,,2500,28,65.4,, 
Saginaw High,,d1_athlete,Michael Thompson,placeholder_url,,,2020,Basketball,Michigan State,,,,,,,,, 
Saginaw High,,pro_athlete,LaMarr Woodley,placeholder_url,,,2003,Football,,NFL - Pittsburgh Steelers,,,,,,,, 
```

## Notes on the Data Format:

1. **Team IDs**: For team references, I've used a composite format: `{school_id}-{sport_name}-{year}` which should make it easy to identify the correct team while being human-readable.

2. **Sport-Specific Stats**: Different sports have different statistics. Basketball players have points, assists, and rebounds while football players have passing yards, touchdowns, and completion percentage. The database structure supports this through separate columns for each common stat type.

3. **Empty Fields**: Many fields are left empty with commas because they don't apply to certain types of individuals (e.g., college doesn't apply to pro athletes).

4. **Media URLs**: I've used "placeholder_url" as placeholders for all media URLs.

5. **Trophy Model**: All teams use the same trophy model "./trophy.gltf" which I see is generated by the generate_trophy.html file.

These CSV files can be directly imported into your database system or spreadsheet software. The data matches the structure and relationships from your original JSON file.

---

_**User**_

why do we need {school_id}-{sport_name}-{year}   it should be separate, i don't want any weid formats

---

_**Assistant**_

<think>
You're right, using a composite key format like `{school_id}-{sport_name}-{year}` isn't ideal and adds unnecessary complexity. Since we're optimizing for simplicity and avoiding weird formats, I should revise this approach.

For the Team table, each record would naturally be identified by the combination of school_id, sport_name, and year. When referring to a team in the Individuals table, we can just use these three separate columns instead of a composite key.

Let me revise the Individuals.csv format to use separate fields for team identification:
</think>

# Revised Sample CSV Data Without Composite Keys

You're absolutely right. Using a composite key format adds unnecessary complexity. Let me revise the data to use simple identifiers without any special formatting.

## 1. Schools.csv (unchanged)

```csv
name,primary_color,secondary_color,accent_color,media_url
Saginaw High,#000080,#FFD700,#000000,placeholder_url
Arthur Hill,#000080,#FFD700,#000000,placeholder_url
Saginaw United,#000080,#FFD700,#000000,placeholder_url
```

## 2. Team.csv (unchanged)

```csv
school_id,sport_name,year,achievement,team_photo_url,trophy_model_url,media_url
Saginaw High,Men's Basketball,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Arthur Hill,Women's Basketball,2022,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Saginaw United,Football,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
```

## 3. Individuals.csv (revised)

```csv
school_id,team_id,type,name,media_url,number,position,graduation_year,sport,college,professional_team,points_per_game,assists,rebounds,passing_yards,touchdowns,completion_percentage,other_stat_name,other_stat_value
Saginaw High,1,player,John Smith,placeholder_url,23,Guard,,,,,18.5,5.2,4.1,,,,, 
Arthur Hill,2,player,Sarah Johnson,placeholder_url,11,Forward,,,,,15.8,3.2,8.4,,,,, 
Saginaw United,3,player,Marcus Williams,placeholder_url,7,Quarterback,,,,,,,,2500,28,65.4,, 
Saginaw High,,d1_athlete,Michael Thompson,placeholder_url,,,2020,Basketball,Michigan State,,,,,,,,, 
Saginaw High,,pro_athlete,LaMarr Woodley,placeholder_url,,,2003,Football,,NFL - Pittsburgh Steelers,,,,,,,, 
```

## Alternative Approach for Team ID

Since you mentioned not wanting to use any composite keys or weird formats, I've simplified the team_id in the Individuals table to use a simple numeric identifier that would match an auto-incrementing ID in your Team table.

If you prefer to not use IDs at all, we could also use these three columns in the Individuals table instead of a team_id:
- school_name
- sport_name
- year

This would completely eliminate the need to track IDs between tables. Would you prefer this approach?

---

_**User**_

yes do not use ids

---

_**Assistant**_

<think>
The user wants to completely eliminate IDs from the data, preferring to use natural keys instead. I'll revise the CSV data to use school name, sport name, and year directly instead of any ID fields. This will make the data more human-readable and easier to work with for manual entry.
</think>

# Revised Sample CSV Data Without Any IDs

Here are the revised CSV files with no IDs at all, using natural keys (names, sport names, years) to relate records:

## 1. Schools.csv

```csv
name,primary_color,secondary_color,accent_color,media_url
Saginaw High,#000080,#FFD700,#000000,placeholder_url
Arthur Hill,#000080,#FFD700,#000000,placeholder_url
Saginaw United,#000080,#FFD700,#000000,placeholder_url
```

## 2. Team.csv

```csv
school_name,sport_name,year,achievement,team_photo_url,trophy_model_url,media_url
Saginaw High,Men's Basketball,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Arthur Hill,Women's Basketball,2022,District Champions,placeholder_url,./trophy.gltf,placeholder_url
Saginaw United,Football,2023,District Champions,placeholder_url,./trophy.gltf,placeholder_url
```

## 3. Individuals.csv

```csv
school_name,team_sport,team_year,type,name,media_url,number,position,graduation_year,sport,college,professional_team,points_per_game,assists,rebounds,passing_yards,touchdowns,completion_percentage,other_stat_name,other_stat_value
Saginaw High,Men's Basketball,2023,player,John Smith,placeholder_url,23,Guard,,,,,18.5,5.2,4.1,,,,, 
Arthur Hill,Women's Basketball,2022,player,Sarah Johnson,placeholder_url,11,Forward,,,,,15.8,3.2,8.4,,,,, 
Saginaw United,Football,2023,player,Marcus Williams,placeholder_url,7,Quarterback,,,,,,,,2500,28,65.4,, 
Saginaw High,,,d1_athlete,Michael Thompson,placeholder_url,,,2020,Basketball,Michigan State,,,,,,,,, 
Saginaw High,,,pro_athlete,LaMarr Woodley,placeholder_url,,,2003,Football,,NFL - Pittsburgh Steelers,,,,,,,, 
```

## Notes on the Revised Format:

1. **Natural Keys**: The tables use actual names and descriptive information instead of IDs. This makes the data more readable and easier to manually enter.

2. **Relationship Tracking**: Instead of using team_id, the Individuals table includes:
   - school_name
   - team_sport (for relating to sport_name in the Team table)
   - team_year (for relating to year in the Team table)

3. **Individual Athletes**: For D1 and pro athletes not associated with a team, the team_sport and team_year fields are left blank.

4. **Column Naming**: Changed "sport_name" to "team_sport" in the Individuals table to clearly distinguish it from the "sport" column used for individual athletes.

This approach completely eliminates IDs while maintaining all the relationships between your data, making it much easier for manual data entry and management.

---

_**User**_

make the individuals more comprehensive with some examples of people who were on multiple years or multiple teams

---



---

