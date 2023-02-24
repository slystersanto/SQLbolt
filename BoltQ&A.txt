-- Exercise 1

-- Find the title of each film
SELECT Title 
FROM movies;

-- Find the director of each film
SELECT director  
FROM movies;

-- Find the title and director of each film
SELECT director,title  
FROM movies;

-- Find the title and year of each film
SELECT year ,title  
FROM movies;

-- Find all the information about each film
SELECT *   
FROM movies;

-- Exercise 2

-- Find the movie with a row id of 6
SELECT * 
FROM movies 
WHERE ID=6;

-- Find the movies released in the years between 2000 and 2010
SELECT * 
FROM movies 
WHERE Year BETWEEN 2000 AND 2010;

-- Find the movies not released in the years between 2000 and 2010
SELECT * 
FROM movies 
WHERE Year NOT BETWEEN 2000 AND 2010;

-- Find the first 5 Pixar movies and their release year
SELECT * 
FROM movies 
LIMIT 5;

-- Exercise 3

-- Find all the Toy Story movies
SELECT * 
FROM movies 
WHERE Title like "%toy Story%";

-- Find all the movies directed by John Lasseter
SELECT * 
FROM movies 
WHERE Director  like "%John Lasseter%";

-- Find all the movies (and director) not directed by John Lasseter
SELECT * 
FROM movies 
WHERE Director not like "%John Lasseter%";

-- Find all the WALL-* movies
SELECT * 
FROM movies 
WHERE Title  like "%WALL-%";

-- Exercise 4

-- List all directors of Pixar movies (alphabetically), without duplicates
SELECT DISTINCT Director 
FROM movies 
ORDER BY Director;

-- List the last four Pixar movies released (ordered from most recent to least)
SELECT * 
FROM movies 
ORDER BY Year desc 
limit 4;

-- List the first five Pixar movies sorted alphabetically
SELECT * 
FROM movies 
order by Title 
limit 5 ;

-- List the next five Pixar movies sorted alphabetically
SELECT * 
FROM movies 
order by Title 
limit 5 offset 5 ;

-- Exercise 5

-- List all the Canadian cities and their populations
SELECT * 
FROM north_american_cities 
where country = "Canada";

-- Order all the cities in the United States by their latitude from north to south
SELECT * 
FROM north_american_cities 
WHERE Country like "%United States%" 
order by latitude desc ;

-- List all the cities west of Chicago, ordered from west to east
SELECT City 
FROM north_american_cities 
WHERE Longitude <-87.629798 
order by Longitude ;

-- List the two largest cities in Mexico (by population)
SELECT * 
FROM north_american_cities 
WHERE Country LIKE "%Mexico%"  
order by Population desc 
limit 2 ;

-- List the third and fourth largest cities (by population) in the United States and their population
SELECT city 
FROM north_american_cities 
WHERE Country like "%United States%" 
order by Population desc 
limit 2 offset 2 ;

-- Exercise 6

-- Find the domestic and international sales for each movie
 SELECT title,Domestic_sales,International_sales 
 FROM movies 
 join Boxoffice on Id=Movie_id ;

-- Show the sales numbers for each movie that did better internationally rather than domestically
 SELECT title,Domestic_sales,International_sales 
 FROM movies 
 join Boxoffice on Id=Movie_id 
 where Domestic_sales<International_sales;

-- List all the movies by their ratings in descending order
 SELECT title,rating 
 FROM movies 
 join Boxoffice on Id=Movie_id 
 order by Rating desc;

-- Exercise 7

-- Find the list of all buildings that have employees
SELECT DISTINCT Building 
FROM employees 
join Buildings on Building=Building_name 
where Years_employed >0;

-- Find the list of all buildings and their capacity
SELECT * 
FROM Buildings  ;

-- List all buildings and the distinct employee roles in each building (including empty buildings)
SELECT DISTINCT Building_NAME,Role 
FROM Buildings 
LEFT JOIN Employees ON  Building_name=Building  ;

-- Exercise 8

-- Find the name and role of all employees who have not been assigned to a building
SELECT * 
FROM employees 
WHERE Building IS NULL;

-- Find the names of the buildings that hold no employees
SELECT *
FROM Buildings
LEFT JOIN Employees
ON Building_name = Building
WHERE Building IS NULL;

-- Exercise 9

-- List all movies and their combined sales in millions of dollars 
SELECT title,(Domestic_sales+International_sales)/1000000 AS TOTAL_SALES 
FROM movies  
join Boxoffice on Id=Movie_id ;

-- List all movies and their ratings in percent
SELECT title,(10*Rating) AS Rating_percentage 
FROM movies  
join Boxoffice on Id=Movie_id ;

-- List all movies that were released on even number years
SELECT title,(year%2) AS EVEN_YEAR 
FROM movies 
where EVEN_YEAR =0 ;

-- Exercise 10

-- Find the longest time that an employee has been at the studio
SELECT max(Years_employed) 
FROM employees;

-- For each role, find the average number of years employed by employees in that role
SELECT AVG(Years_employed),ROLE   
FROM employees GROUP BY ROLE;

-- Find the total number of employee years worked in each building
SELECT SUM(Years_employed),Building   
FROM employees GROUP BY Building;

-- Exercise  11

-- Find the number of Artists in the studio (without a HAVING clause)
SELECT role,count() 
FROM employees 
where role="Artist";

-- Find the number of Employees of each role in the studio
SELECT role,count(*) AS no_artist 
from employees group by role;

-- Find the total number of years employed by all Engineers
SELECT *,role,sum(Years_employed) 
FROM employees 
where role like "%Engineer%";

-- Exercise 12

-- Find the number of movies each director has directed
SELECT Director,count() 
FROM movies group by Director;

-- Find the total domestic and international sales that can be attributed to each director
SELECT Director,sum(Domestic_sales+International_sales) 
FROM movies 
JOIN Boxoffice 
ON Id=Movie_id 
GROUP BY Director;

-- Exercise 13

-- Add the studio's new production, Toy Story 4 to the list of movies (you can use any director)
INSERT INTO Movies
VALUES (4, "Toy Story 4", "John Lasseter", 2017, 123);

-- Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the BoxOffice table.
INSERT INTO Boxoffice 
VALUES (4,8.7,340000000,270000000)

-- Exercise 14

-- The director for A Bug's Life is incorrect, it was actually directed by John Lasseter
UPDATE Movies
SET Director="John Lasseter"
WHERE Title ="A Bug's Life";

-- The year that Toy Story 2 was released is incorrect, it was actually released in 1999
UPDATE Movies
SET Year=1999
WHERE Title ="Toy Story 2";

-- Both the title and director for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich
UPDATE Movies
SET Title="Toy Story 3",Director="Lee Unkrich"
WHERE Title ="Toy Story 8";

-- Exercise 15

-- This database is getting too big, lets remove all movies that were released before 2005.
DELETE FROM Movies
WHERE Year<2005;

-- Andrew Stanton has also left the studio, so please remove all movies directed by him.
DELETE FROM Movies
WHERE Director="Andrew Stanton";

-- Exercise 16

-- Create a new table named Database with the following columns:
-- – Name A string (text) describing the name of the database
-- – Version A number (floating point) of the latest version of this database
-- – Download_count An integer count of the number of times this database was downloaded
-- This table has no constraints.
CREATE TABLE Database (
Name TEXT,
Version FLOAT,
Download_count INTEGER);

-- Exercise 17

-- Add a column named Aspect_ratio with a FLOAT data type to store the aspect-ratio each movie was released in.
ALTER TABLE Movies
ADD Aspect_ratio FLOAT DEFAULT 1  ;

-- Add another column named Language with a TEXT data type to store the language that the movie was released in. Ensure that the default for this language is English.
ALTER TABLE Movies
ADD Language TEXT DEFAULT English  ;

-- Exercise 18

-- We've sadly reached the end of our lessons, lets clean up by removing the Movies table
DROP TABLE Movies;

-- And drop the BoxOffice table as well
DROP TABLE BoxOffice;