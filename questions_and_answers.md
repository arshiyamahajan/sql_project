# Movie_Database_Analysis
## Questions and Answers

**Collaborators**: Aryana Sharma, Arshiya Mahajan

#### Ques 1. What were the top 5 highest-rated movies from 2006-2016?

````sql
SELECT Title, Rating FROM movies.movie_ratings
ORDER BY Rating DESC
LIMIT 5;
````

**Results:**

| Title            | Rating | 
|------------------|--------|
| The Dark Knight  |      9 |   
| Inception        |    8.8 |   
| Kimi no na wa    |    8.6 |   
| Interstellar     |    8.6 |   
| The Intouchables |    8.6 |


#### Ques 2. Do the Directors and the Cast have an impact on movie ratings?
````sql
SELECT Director, Actors, AVG(Rating) AS average_rating
FROM movies.movie_ratings
GROUP BY Director, Actors
HAVING AVG(Rating) > (SELECT AVG(Rating) FROM movies.movie_ratings)
limit 20;
````
**Results:**

| Director                         | Actors                                                                        | average_rating |   |   |
|----------------------------------|-------------------------------------------------------------------------------|----------------|---|---|
| Christopher Nolan                | Christian Bale, Heath Ledger, Aaron Eckhart,Michael Caine                     |              9 |   |   |
| Christopher Nolan                | Leonardo DiCaprio, Joseph Gordon-Levitt, Ellen Page, Ken Watanabe             |            8.8 |   |   |
| Olivier Nakache                  | François Cluzet, Omar Sy, Anne Le Ny, Audrey Fleurot                          |            8.6 |   |   |
| Christopher Nolan                | Matthew McConaughey, Anne Hathaway, Jessica Chastain, Mackenzie Foy           |            8.6 |   |   |
| Makoto Shinkai                   | Ryûnosuke Kamiki, Mone Kamishiraishi, Ryô Narita, Aoi Yuki                    |            8.6 |   |   |
| Christopher Nolan                | Christian Bale, Hugh Jackman, Scarlett Johansson, Michael Caine               |            8.5 |   |   |
| Martin Scorsese                  | Leonardo DiCaprio, Matt Damon, Jack Nicholson, Mark Wahlberg                  |            8.5 |   |   |
| Florian Henckel von Donnersmarck | Ulrich Mühe, Martina Gedeck,Sebastian Koch, Ulrich Tukur                      |            8.5 |   |   |
| Aamir Khan                       | Darsheel Safary, Aamir Khan, Tanay Chheda, Sachet Engineer                    |            8.5 |   |   |
| Christopher Nolan                | Christian Bale, Tom Hardy, Anne Hathaway,Gary Oldman                          |            8.5 |   |   |
| Damien Chazelle                  | Miles Teller, J.K. Simmons, Melissa Benoist, Paul Reiser                      |            8.5 |   |   |
| Rajkumar Hirani                  | Aamir Khan, Madhavan, Mona Singh, Sharman Joshi                               |            8.4 |   |   |
| Quentin Tarantino                | Jamie Foxx, Christoph Waltz, Leonardo DiCaprio,Kerry Washington               |            8.4 |   |   |
| Quentin Tarantino                | Brad Pitt, Diane Kruger, Eli Roth,Mélanie Laurent                             |            8.3 |   |   |
| Pete Docter                      | Edward Asner, Jordan Nagai, John Ratzenberger, Christopher Plummer            |            8.3 |   |   |
| Lee Unkrich                      | Tom Hanks, Tim Allen, Joan Cusack, Ned Beatty                                 |            8.3 |   |   |
| Thomas Vinterberg                | Mads Mikkelsen, Thomas Bo Larsen, Annika Wedderkopp, Lasse Fogelstrøm         |            8.3 |   |   |
| Damien Chazelle                  | Ryan Gosling, Emma Stone, Rosemarie DeWitt, J.K. Simmons                      |            8.3 |   |   |
| Guillermo del Toro               | Ivana Baquero, Ariadna Gil, Sergi López,Maribel Verdú                         |            8.2 |   |   |
| Juan José Campanella             | Ricardo Darín, Soledad Villamil, Pablo Rago,Carla Quevedo                     |            8.2 |   |   |

Conclusion: In this query, we retrieve the director, cast, and average ratings for movies grouped by the director and cast. The `HAVING` clause filters the results to only include combinations (director and cast) that have an average rating higher than the overall average rating of all movies.

By comparing the average rating of movies associated with a specific director and cast to the overall average rating, this query helps identify instances where a director and cast combination tends to yield higher-rated movies, suggesting an impact on the movie rating.

#### Ques 3. Do votes and ratings have a correlation?

````sql
SELECT
CORR(votes, rating) AS correlation_coefficient
FROM
movies.movie_ratings;
````

**Results:**

| correlation_coefficient |   |   |   |   |
|-------------------------|---|---|---|---|
|            0.5174521336 |   |   |   |   |
|                         |   |   |   |   |
|                         |   |   |   |   |

Conclusion: A value close to 1 indicates a strong positive correlation, a value close to -1 indicates a strong negative correlation and a value close to 0 indicates no or weak correlation.

Hence, it can be seen that there exists minimal positive correlation between votes and ratings.

#### Ques 4. Retrieve the directors whose movies have consistently high ratings (all movies have a rating above 8).

````sql
SELECT Director
FROM movies.movie_ratings
GROUP BY Director
HAVING MIN(Rating) > 8;

````

**Results:**

| Director                         |   |   |
|----------------------------------|---|---|
| Christopher Nolan                |   |   |
| Florian Henckel von Donnersmarck |   |   |
| Aamir Khan                       |   |   |
| Sean Penn                        |   |   |
| Juan José Campanella             |   |   |
| Pete Docter                      |   |   |
| Rajkumar Hirani                  |   |   |
| Lee Unkrich                      |   |   |
| Olivier Nakache                  |   |   |
| Thomas Vinterberg                |   |   |
| Damien Chazelle                  |   |   |
| Xavier Dolan                     |   |   |
| Damián Szifron                   |   |   |
| Tom McCarthy                     |   |   |
| Lenny Abrahamson                 |   |   |
| Garth Davis                      |   |   |
| Byron Howard                     |   |   |
| Chan-wook Park                   |   |   |
| Makoto Shinkai                   |   |   |

Conclusion: We can see that Christopher Nolan, Florian Henckel von Donnersmarck, Aamir Khan, Sean Penn, Juan José Campanella are among the directors who directed the highest rating movies of all times.

#### Ques 5. Retrieve the movies that have a rating higher than the average rating of all movies in the same genre and year.

````sql
SELECT Title, Genre, Year, Rating
FROM movies.movie_ratings
WHERE Rating > (
  SELECT AVG(Rating)
  FROM movies.movie_ratings
  WHERE genre = Genre AND year = Year
)
limit 30;
````

**Results:**

| Title                                                         | Genre                      | Year | Rating |   |
|---------------------------------------------------------------|----------------------------|------|--------|---|
| The Host                                                      | Comedy,Drama,Horror        | 2006 |      7 |   |
| Babel                                                         | Drama                      | 2006 |    7.5 |   |
| Perfume: The Story of a Murderer                              | Crime,Drama,Fantasy        | 2006 |    7.5 |   |
| Casino Royale                                                 | Action,Adventure,Thriller  | 2006 |      8 |   |
| The Pursuit of Happyness                                      | Biography,Drama            | 2006 |      8 |   |
| Blood Diamond                                                 | Adventure,Drama,Thriller   | 2006 |      8 |   |
| The Prestige                                                  | Drama,Mystery,Sci-Fi       | 2006 |    8.5 |   |
| The Departed                                                  | Crime,Drama,Thriller       | 2006 |    8.5 |   |
| The Lives of Others                                           | Drama,Thriller             | 2006 |    8.5 |   |
| Pirates of the Caribbean: Dead Man's Chest                    | Action,Adventure,Fantasy   | 2006 |    7.3 |   |
| The Fountain                                                  | Drama,Sci-Fi               | 2006 |    7.3 |   |
| Rescue Dawn                                                   | Adventure,Biography,Drama  | 2006 |    7.3 |   |
| Apocalypto                                                    | Action,Adventure,Drama     | 2006 |    7.8 |   |
| Little Miss Sunshine                                          | Comedy,Drama               | 2006 |    7.8 |   |
| Lucky Number Slevin                                           | Crime,Drama,Mystery        | 2006 |    7.8 |   |
| Cars                                                          | Animation,Adventure,Comedy | 2006 |    7.1 |   |
| The Illusionist                                               | Drama,Mystery,Romance      | 2006 |    7.6 |   |
| Inside Man                                                    | Crime,Drama,Mystery        | 2006 |    7.6 |   |
| Pan's Labyrinth                                               | Drama,Fantasy,War          | 2006 |    8.2 |   |
| A Good Year                                                   | Comedy,Drama,Romance       | 2006 |    6.9 |   |
| Mission: Impossible III                                       | Action,Adventure,Thriller  | 2006 |    6.9 |   |
| Children of Men                                               | Drama,Sci-Fi,Thriller      | 2006 |    7.9 |   |
| The Fall                                                      | Adventure,Comedy,Drama     | 2006 |    7.9 |   |
| Rocky Balboa                                                  | Drama,Sport                | 2006 |    7.2 |   |
|                                                           300 | Action,Fantasy,War         | 2006 |    7.7 |   |
| Knocked Up                                                    | Comedy,Romance             | 2007 |      7 |   |
| 28 Weeks Later                                                | Drama,Horror,Sci-Fi        | 2007 |      7 |   |
| Harry Potter and the Order of the Phoenix                     | Adventure,Family,Fantasy   | 2007 |    7.5 |   |
| Juno                                                          | Comedy,Drama               | 2007 |    7.5 |   |
| The Assassination of Jesse James by the Coward Robert Ford    | Biography,Crime,Drama      | 2007 |    7.5 |   |
                                             

#### Ques 6. What is the most common genre combinations in movies?

````sql
SELECT
Genre,
COUNT(*) AS frequency
FROM (
SELECT
ARRAY_TO_STRING(ARRAY_AGG(DISTINCT Genre ORDER BY Genre), ',') AS Genre
FROM movies.movie_ratings
GROUP BY Title
) subquery
GROUP BY Genre
ORDER BY frequency DESC
limit 30;

````

**Results:**

| Genre                                        | frequency |   |   |   |
|----------------------------------------------|-----------|---|---|---|
| Action,Adventure,Sci-Fi                      |        50 |   |   |   |
| Comedy,Drama,Romance                         |        30 |   |   |   |
| Drama                                        |        29 |   |   |   |
| Drama,Romance                                |        27 |   |   |   |
| Animation,Adventure,Comedy                   |        26 |   |   |   |
| Comedy                                       |        26 |   |   |   |
| Action,Adventure,Fantasy                     |        25 |   |   |   |
| Comedy,Drama                                 |        24 |   |   |   |
| Comedy,Romance                               |        22 |   |   |   |
| Crime,Drama,Thriller                         |        18 |   |   |   |
| Crime,Drama,Mystery                          |        18 |   |   |   |
| Action,Adventure,Drama                       |        17 |   |   |   |
| Action,Crime,Drama                           |        16 |   |   |   |
| Adventure,Family,Fantasy                     |        14 |   |   |   |
| Action,Adventure,Comedy                      |        14 |   |   |   |
| Drama,Thriller                               |        12 |   |   |   |
| Biography,Drama,History                      |        12 |   |   |   |
| Action,Comedy,Crime                          |        12 |   |   |   |
| Action,Crime,Thriller                        |        11 |   |   |   |
| Action,Adventure,Thriller                    |        11 |   |   |   |
| Biography,Drama                              |        11 |   |   |   |
| Horror,Thriller                              |        11 |   |   |   |
| Adventure,Comedy,Drama                       |         8 |   |   |   |
| Biography,Crime,Drama                        |         8 |   |   |   |
| Animation,Action,Adventure                   |         8 |   |   |   |
| Action,Thriller                              |         8 |   |   |   |
| Crime,Drama                                  |         8 |   |   |   |
| Horror                                       |         7 |   |   |   |
| Horror,Mystery,Thriller                      |         7 |   |   |   |
| Biography,Drama,Sport                        |         7 |   |   |   |

Conclusion: Action,Adventure,Sci-Fi is the most famous combination of genres throughput 2006-2016.

#### Ques 7. What are the movies with a rating higher than the average rating of their genre?

````sql
SELECT
Title,
Genre,
Rating
FROM (
SELECT
Title,
Genre,
Rating,
AVG(Rating) OVER (PARTITION BY Genre) AS average_rating
FROM movies.movie_ratings
) subquery
WHERE rating > average_rating
limit 30
````

**Results:**

| Title                                                      | Genre                      | Rating |
|------------------------------------------------------------|----------------------------|--------|
| Män som hatar kvinnor                                      | Drama,Mystery,Thriller     |    7.8 |
| We Need to Talk About Kevin                                | Drama,Mystery,Thriller     |    7.5 |
| Tinker Tailor Soldier Spy                                  | Drama,Mystery,Thriller     |    7.1 |
| Sunshine                                                   | Adventure,Sci-Fi,Thriller  |    7.3 |
| The Hunger Games                                           | Adventure,Sci-Fi,Thriller  |    7.2 |
| The Love Witch                                             | Comedy,Horror              |    6.2 |
| Underworld Awakening                                       | Action,Fantasy,Horror      |    6.4 |
| The Mortal Instruments: City of Bones                      | Action,Fantasy,Horror      |    5.9 |
| Interstellar                                               | Adventure,Drama,Sci-Fi     |    8.6 |
| The Host                                                   | Comedy,Drama,Horror        |      7 |
| 28 Weeks Later                                             | Drama,Horror,Sci-Fi        |      7 |
| I Am Legend                                                | Drama,Horror,Sci-Fi        |    7.2 |
| The Fighter                                                | Action,Biography,Drama     |    7.8 |
| Lone Survivor                                              | Action,Biography,Drama     |    7.5 |
| Rush                                                       | Action,Biography,Drama     |    8.1 |
| American Sniper                                            | Action,Biography,Drama     |    7.3 |
| Creed                                                      | Drama,Sport                |    7.6 |
| Apocalypto                                                 | Action,Adventure,Drama     |    7.8 |
| Terminator Salvation                                       | Action,Adventure,Drama     |    6.6 |
| The Book of Eli                                            | Action,Adventure,Drama     |    6.9 |
| Robin Hood                                                 | Action,Adventure,Drama     |    6.7 |
| Dawn of the Planet of the Apes                             | Action,Adventure,Drama     |    7.6 |
| Everest                                                    | Action,Adventure,Drama     |    7.1 |
| Shin Gojira                                                | Action,Adventure,Drama     |    6.9 |
| The Illusionist                                            | Drama,Mystery,Romance      |    7.6 |
| El secreto de sus ojos                                     | Drama,Mystery,Romance      |    8.2 |
| Ah-ga-ssi                                                  | Drama,Mystery,Romance      |    8.1 |
| Ted 2                                                      | Adventure,Comedy,Romance   |    6.3 |
| Hanna                                                      | Action,Drama,Thriller      |    6.8 |
| Deepwater Horizon                                          | Action,Drama,Thriller      |    7.2 |

#### Ques 8. Who are the top-rated directors who have movies in multiple genres?

````sql
SELECT
Director,
COUNT(DISTINCT Genre) AS num_genres,
AVG(Rating) AS average_rating
FROM movies.movie_ratings
GROUP BY Director
HAVING COUNT(DISTINCT Genre) > 1
ORDER BY average_rating DESC;

````

**Results:**

| Director                    | num_genres | average_rating |
|-----------------------------|------------|----------------|
| Christopher Nolan           |          5 |           8.68 |
| Damien Chazelle             |          2 |            8.4 |
| Rajkumar Hirani             |          2 |            8.3 |
| Quentin Tarantino           |          3 |    8.166666667 |
| Mel Gibson                  |          2 |              8 |
| Martin Scorsese             |          5 |           7.92 |
| Wes Anderson                |          2 |            7.9 |
| Gabriele Muccino            |          2 |           7.85 |
| David Fincher               |          4 |           7.82 |
| Gavin O'Connor              |          2 |            7.8 |
| Alejandro González Iñárritu |          3 |    7.766666667 |
| Denis Villeneuve            |          5 |           7.76 |
| Joss Whedon                 |          2 |           7.75 |
| Matthew Vaughn              |          4 |          7.725 |
| John Carney                 |          2 |            7.7 |
| F. Gary Gray                |          2 |           7.65 |
| Steve McQueen               |          3 |    7.633333333 |
| J.J. Abrams                 |          4 |           7.58 |
| Martin McDonagh             |          2 |           7.55 |
| J.A. Bayona                 |          2 |           7.55 |
| Morten Tyldum               |          2 |           7.55 |
| Paul Greengrass             |          3 |    7.533333333 |
| Clint Eastwood              |          3 |    7.533333333 |
| Tom Hooper                  |          3 |    7.533333333 |
| Peter Jackson               |          2 |          7.475 |
| Edgar Wright                |          3 |    7.466666667 |
| Duncan Jones                |          3 |    7.466666667 |
| John Lee Hancock            |          3 |    7.466666667 |
| Neill Blomkamp              |          2 |           7.45 |
| Richard Linklater           |          2 |           7.45 |
| Todd Phillips               |          2 |           7.45 |
| David Yates                 |          3 |    7.433333333 |
| Jon Favreau                 |          3 |          7.425 |
| Guy Ritchie                 |          3 |          7.425 |
| Danny Boyle                 |          5 |           7.42 |
| Paul Thomas Anderson        |          2 |            7.4 |
| David O. Russell            |          4 |          7.375 |
| Phil Lord                   |          2 |    7.366666667 |
| Jean-Marc Vallée            |          3 |    7.366666667 |
| Ben Affleck                 |          4 |           7.35 |
| Ethan Coen                  |          3 |    7.333333333 |
| Jonathan Levine             |          2 |            7.3 |
| Shane Black                 |          2 |            7.3 |
| George Miller               |          2 |            7.3 |
| Brad Bird                   |          3 |            7.3 |
| Sam Mendes                  |          2 |            7.3 |
| Derek Cianfrance            |          2 |            7.3 |
| Tate Taylor                 |          2 |            7.3 |
| Matt Reeves                 |          3 |    7.266666667 |
| Brian Helgeland             |          2 |           7.25 |
| Neil Burger                 |          3 |    7.233333333 |
| James Wan                   |          2 |          7.225 |
| Ruben Fleischer             |          2 |            7.2 |
| Christopher McQuarrie       |          2 |            7.2 |
| Guillermo del Toro          |          4 |            7.2 |
| Yorgos Lanthimos            |          2 |            7.2 |
| Scott Derrickson            |          2 |            7.2 |
| David Ayer                  |          3 |    7.166666667 |
| James Gunn                  |          3 |    7.133333333 |
| Marc Webb                   |          3 |    7.133333333 |
| Ang Lee                     |          2 |            7.1 |
| Zack Snyder                 |          5 |           7.04 |
| Antoine Fuqua               |          5 |           7.04 |
| Tom Tykwer                  |          3 |    7.033333333 |
| Darren Aronofsky            |          3 |    7.033333333 |
| Francis Lawrence            |          3 |          7.025 |
| Woody Allen                 |          5 |           7.02 |
| Bong Joon Ho                |          2 |              7 |
| Adam McKay                  |          4 |              7 |
| Gavin Hood                  |          2 |              7 |
| Lone Scherfig               |          2 |              7 |
| Nicolas Winding Refn        |          2 |              7 |
| Glenn Ficarra               |          2 |              7 |
| Andrew Stanton              |          2 |              7 |
| Gore Verbinski              |          2 |    6.966666667 |
| Ron Howard                  |          4 |           6.95 |
| Baz Luhrmann                |          2 |           6.95 |
| Sylvester Stallone          |          3 |    6.933333333 |
| Edward Zwick                |          3 |    6.933333333 |
| Paul McGuigan               |          2 |            6.9 |
| Steven Spielberg            |          4 |            6.9 |
| Joseph Kosinski             |          2 |            6.9 |
| Oliver Stone                |          2 |            6.9 |
| Jeff Nichols                |          2 |            6.9 |
| Robert Zemeckis             |          3 |    6.866666667 |
| Peter Berg                  |          5 |           6.86 |
| Ridley Scott                |          7 |           6.85 |
| Richard LaGravenese         |          2 |           6.85 |
| Lars von Trier              |          2 |           6.85 |
| Fede Alvarez                |          2 |           6.85 |
| Scott Cooper                |          2 |           6.85 |
| John Crowley                |          2 |           6.85 |
| Justin Lin                  |          2 |           6.82 |
| Martin Campbell             |          2 |            6.8 |
| David Frankel               |          2 |            6.8 |
| Tarsem Singh                |          3 |            6.8 |
| Greg Mottola                |          3 |            6.8 |
| Pierre Morel                |          2 |            6.8 |
| Marc Forster                |          2 |            6.8 |
| Rupert Wyatt                |          2 |            6.8 |
| John Hillcoat               |          2 |            6.8 |
| Jaume Collet-Serra          |          3 |    6.766666667 |
| George Tillman Jr.          |          3 |    6.766666667 |
| Mikael Håfström             |          2 |           6.75 |
| Len Wiseman                 |          2 |           6.75 |
| Alan Taylor                 |          2 |           6.75 |
| Kenneth Branagh             |          3 |    6.733333333 |
| Tim Burton                  |          4 |            6.7 |
| Judd Apatow                 |          2 |           6.65 |
| Robert Luketic              |          2 |           6.65 |
| Rawson Marshall Thurber     |          2 |           6.65 |
| Anne Fletcher               |          2 |            6.6 |
| Jeff Wadlow                 |          2 |            6.6 |
| Jason Moore                 |          2 |            6.6 |
| Evan Goldberg               |          2 |            6.6 |
| Nicholas Stoller            |          3 |           6.55 |
| Peyton Reed                 |          2 |           6.55 |
| Wes Ball                    |          2 |           6.55 |
| Steven Soderbergh           |          3 |    6.533333333 |
| Shawn Levy                  |          3 |    6.533333333 |
| Louis Leterrier             |          4 |          6.525 |
| Michael Bay                 |          3 |    6.483333333 |
| Seth MacFarlane             |          3 |    6.466666667 |
| Phillip Noyce               |          2 |           6.45 |
| Paul Feig                   |          3 |           6.45 |
| Joe Wright                  |          3 |            6.4 |
| McG                         |          3 |    6.366666667 |
| Robert Schwentke            |          3 |    6.366666667 |
| Brett Ratner                |          2 |           6.35 |
| Rob Marshall                |          2 |           6.35 |
| Luc Besson                  |          2 |           6.35 |
| Ben Wheatley                |          2 |           6.35 |
| Ben Stiller                 |          3 |    6.333333333 |
| Will Gluck                  |          3 |    6.333333333 |
| Adam Shankman               |          2 |            6.3 |
| Andrew Niccol               |          2 |            6.3 |
| David Gordon Green          |          2 |            6.3 |
| Mike Flanagan               |          2 |            6.3 |
| Sam Raimi                   |          2 |           6.25 |
| Terrence Malick             |          2 |           6.25 |
| Timur Bekmambetov           |          2 |            6.2 |
| Michael Mann                |          2 |            6.2 |
| Sean Anders                 |          2 |            6.2 |
| Jon M. Chu                  |          3 |    6.166666667 |
| Alexandre Aja               |          3 |    6.133333333 |
| Burr Steers                 |          2 |            6.1 |
| Thor Freudenthal            |          2 |           6.05 |
| Harald Zwart                |          2 |           6.05 |
| Barry Sonnenfeld            |          2 |           6.05 |
| Kevin Smith                 |          2 |              6 |
| Karyn Kusama                |          2 |            5.9 |
| Adam Wingard                |          2 |            5.9 |
| Jonathan Liebesman          |          2 |           5.85 |
| Kirk Jones                  |          2 |           5.85 |
| Roland Emmerich             |          2 |    5.833333333 |
| Dennis Dugan                |          3 |          5.825 |
| M. Night Shyamalan          |          5 |            5.8 |
| Paul W.S. Anderson          |          5 |    5.766666667 |
| Chris Columbus              |          2 |           5.75 |
| D.J. Caruso                 |          3 |    5.533333333 |
| Elizabeth Banks             |          2 |            5.4 |
| Eli Roth                    |          3 |    5.266666667 |
| Rob Cohen                   |          2 |            4.9 |

Conclusion : Christopher Nolan, Damien Chazelle ,Rajkumar Hirani, Quentin Tarantino, Mel Gibson, Martin Scorsese, Wes Anderson , Gabriele Muccino , David Fincher are among the top-rated directors who have movies in multiple genres from 2006 to 2016.

#### Ques 9. What are the top 5 most recurring combination of actors and their movie ratings?

````sql
SELECT
Actors,
AVG(Rating) AS average_rating
FROM movies.movie_ratings
WHERE Actors IS NOT NULL
GROUP BY Actors
ORDER BY COUNT(*) DESC
LIMIT 5;

````

**Results:**

| Actors                                                              | average_rating |
|---------------------------------------------------------------------|----------------|
| Daniel Radcliffe, Emma Watson, Rupert Grint, Michael Gambon         |            7.8 |
| Jennifer Lawrence, Josh Hutcherson, Liam Hemsworth, Woody Harrelson |           6.65 |
| Shia LaBeouf, Megan Fox, Josh Duhamel, Tyrese Gibson                |           6.55 |
| Gerard Butler, Aaron Eckhart, Morgan Freeman,Angela Bassett         |            6.2 |
| Elijah Wood, Brittany Murphy, Hugh Jackman, Robin Williams          |            6.5 |

Conclusion: 
Daniel Radcliffe, Emma Watson, Rupert Grint, Michael Gambon is the combination of actors that has the top most movie rating of 7.8.

#### Ques 10. What is the correlation between revenue and rating?

````sql

SELECT
Rating,
AVG(Revenue__Millions_) AS average_revenue
FROM movies.movie_ratings
GROUP BY Rating
ORDER BY Rating desc;

````

**Results:**

| Rating | average_revenue |
|--------|-----------------|
|      9 |          533.32 |
|    8.8 |          292.57 |
|    8.6 |     68.61666667 |
|    8.5 |     109.8583333 |
|    8.4 |           84.66 |
|    8.3 |          196.03 |
|    8.2 |     71.55444444 |
|    8.1 |     175.4858333 |
|      8 |     133.7136842 |
|    7.9 |        131.0755 |
|    7.8 |     124.6326316 |
|    7.7 |         90.3208 |
|    7.6 |     103.4053846 |
|    7.5 |     87.07515152 |
|    7.4 |     107.7731034 |
|    7.3 |     79.00810811 |
|    7.2 |     88.93210526 |
|    7.1 |          80.296 |
|      7 |     93.95186047 |
|    6.9 |     58.81434783 |
|    6.8 |     57.30121212 |
|    6.7 |     88.25452381 |
|    6.6 |     75.93432432 |
|    6.5 |     62.54628571 |
|    6.4 |          74.114 |
|    6.3 |     55.29583333 |
|    6.2 |     80.23515152 |
|    6.1 |     64.02409091 |
|      6 |          78.717 |
|    5.9 |     53.68933333 |
|    5.8 |     57.86809524 |
|    5.7 |         41.8575 |
|    5.6 |     36.77384615 |
|    5.5 |     79.56181818 |
|    5.4 |          44.991 |
|    5.3 |        55.53375 |
|    5.2 |     80.75166667 |
|    5.1 |           16.56 |
|      5 |           64.51 |
|    4.9 |     129.0383333 |
|    4.8 |           34.33 |
|    4.7 |           31.23 |
|    4.6 |           19.58 |
|    4.4 |            0.18 |
|    4.3 |     53.42333333 |
|    4.1 |          166.15 |
|      4 |           20.76 |
|    3.9 |           47.73 |
|    2.7 |            9.35 |
|    1.9 |           14.17 |

Conclusion: Here, we observe the correlation between revenue and rating. From the output, we can conclude that a high- rated movie generates more revenue than a low-rated movie. 

#### Ques 11. How does the genre affect the revenue earned by the movie?

````sql

SELECT
Genre,
AVG(Revenue__Millions_) AS average_revenue
FROM movies.movie_ratings
GROUP BY Genre
ORDER BY average_revenue DESC
limit 10;

````

**Results:**

| Genre                      | average_revenue |
|----------------------------|-----------------|
| Action,Sci-Fi              |          318.34 |
| Adventure,Drama,Fantasy    |         276.008 |
| Adventure,Fantasy          |     272.1566667 |
| Action,Adventure           |          223.74 |
| Animation,Adventure,Comedy |     221.3365385 |
| Action,Fantasy,War         |          210.59 |
| Action,Adventure,Sci-Fi    |        209.2302 |
| Action,Adventure,Fantasy   |        209.0808 |
| Adventure,Drama,Sci-Fi     |          208.21 |
| Animation,Action,Adventure |       206.46875 |

Conclusion: Here, we can observe that the combination of genre earning the most revenue is Action,Sci-Fi with an average revenue of 318.34 millions.

#### Ques 12. How does the director affect the revenue earned by the movie?

````sql

SELECT
Director,
AVG(Revenue__Millions_) AS average_revenue
FROM movies.movie_ratings
GROUP BY Director
ORDER BY average_revenue DESC
limit 20;

````

**Results:**

| Director          | average_revenue |
|-------------------|-----------------|
| James Cameron     |          760.51 |
| Colin Trevorrow   |          652.18 |
| Joss Whedon       |         541.135 |
| Lee Unkrich       |          414.98 |
| Gary Ross         |             408 |
| Chris Buck        |          400.74 |
| Chris Renaud      |          368.31 |
| Gareth Edwards    |         366.415 |
| Tim Miller        |          363.02 |
| Byron Howard      |          341.26 |
| J.J. Abrams       |          336.69 |
| Kyle Balda        |          336.03 |
| Anthony Russo     |         333.915 |
| Francis Lawrence  |        324.9525 |
| Pete Docter       |         324.715 |
| Pierre Coffin     |         309.775 |
| Christopher Nolan |         303.018 |
| David Slade       |          300.52 |
| Bill Condon       |          286.79 |
| Sam Raimi         |         285.715 |

Conclusion: In conclusion, James Cameron has the highest revenue out of all the directors averaging out to 760.51 million.

#### Ques 13. Which genre was the most popular throughout the years 2006-2016?

````sql

WITH Genre AS (
SELECT
Year AS release_year,
Genre,
COUNT(*) AS movie_count,
ROW_NUMBER() OVER (PARTITION BY Year ORDER BY COUNT(*) DESC) AS rank
FROM movies.movie_ratings
GROUP BY release_year, Genre
)
SELECT
release_year,
Genre,
movie_count
FROM Genre
WHERE rank = 1
ORDER BY release_year;


````

**Results:**

| release_year | Genre                                                                                                                              | movie_count |
|--------------|------------------------------------------------------------------------------------------------------------------------------------|-------------|
|         2006 | {   "Genre": {     "release_year": "2006",     "Genre": "Action,Adventure,Thriller",     "movie_count": "2",     "rank": "1"   } } |           2 |
|         2007 | {   "Genre": {     "release_year": "2007",     "Genre": "Adventure,Family,Fantasy",     "movie_count": "3",     "rank": "1"   } }  |           3 |
|         2008 | {   "Genre": {     "release_year": "2008",     "Genre": "Action,Adventure,Fantasy",     "movie_count": "3",     "rank": "1"   } }  |           3 |
|         2009 | {   "Genre": {     "release_year": "2009",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "5",     "rank": "1"   } }   |           5 |
|         2010 | {   "Genre": {     "release_year": "2010",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "4",     "rank": "1"   } }   |           4 |
|         2011 | {   "Genre": {     "release_year": "2011",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "5",     "rank": "1"   } }   |           5 |
|         2012 | {   "Genre": {     "release_year": "2012",     "Genre": "Crime,Drama,Thriller",     "movie_count": "3",     "rank": "1"   } }      |           3 |
|         2013 | {   "Genre": {     "release_year": "2013",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "6",     "rank": "1"   } }   |           6 |
|         2014 | {   "Genre": {     "release_year": "2014",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "8",     "rank": "1"   } }   |           8 |
|         2015 | {   "Genre": {     "release_year": "2015",     "Genre": "Action,Adventure,Sci-Fi",     "movie_count": "8",     "rank": "1"   } }   |           8 |
|         2016 | {   "Genre": {     "release_year": "2016",     "Genre": "Comedy",     "movie_count": "9",     "rank": "1"   } }                    |           9 |
