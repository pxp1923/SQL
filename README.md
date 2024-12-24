# SQL
Project Movie
CREATE DATABASE MovieDB;
USE MovieDB;
CREATE TABLE Genres (
  GenreID INT AUTO_INCREMENT PRIMARY KEY,
  GenreName VARCHAR(50) NOT NULL
);

CREATE TABLE Directors (
  DirectorID INT AUTO_INCREMENT PRIMARY KEY,
  Name VARCHAR(100) NOT NULL,
  BirthYear INT
);

CREATE TABLE Movies (
  MovieID INT AUTO_INCREMENT PRIMARY KEY,
  Title VARCHAR(100) NOT NULL,
  ReleaseYear INT,
  GenreID INT,
  DirectorID INT,
  FOREIGN KEY (GenreID) REFERENCES Genres(GenreID),
  FOREIGN KEY (DirectorID) REFERENCES Directors(DirectorID)
);

CREATE TABLE Actors (
  ActorID INT AUTO_INCREMENT PRIMARY KEY,
  Name VARCHAR(100) NOT NULL,
  BirthYear INT
);

CREATE TABLE MovieActors (
  MovieID INT,
  ActorID INT,
  PRIMARY KEY (MovieID, ActorID),
  FOREIGN KEY (MovieID) REFERENCES Movies(MovieID),
  FOREIGN KEY (ActorID) REFERENCES Actors(ActorID)
);

CREATE TABLE Users (
  UserID INT AUTO_INCREMENT PRIMARY KEY,
  Name VARCHAR(100) NOT NULL,
  Email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE Reviews (
  ReviewID INT AUTO_INCREMENT PRIMARY KEY,
  MovieID INT,
  UserID INT,
  Rating DECIMAL(3, 2) CHECK (Rating BETWEEN 0 AND 10),
  ReviewText TEXT,
  FOREIGN KEY (MovieID) REFERENCES Movies(MovieID),
  FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
INSERT INTO Genres (GenreName) VALUES ('Action'), ('Comedy'), ('Drama'), ('Horror'), ('Sci-Fi');
SELECT m.Title, g.GenreName, d.Name AS Director
FROM Movies m
JOIN Genres g ON m.GenreID = g.GenreID
JOIN Directors d ON m.DirectorID = d.DirectorID;
SELECT m.Title, AVG(r.Rating) AS AvgRating
FROM Reviews r
JOIN Movies m ON r.MovieID = m.MovieID
GROUP BY m.MovieID
ORDER BY AvgRating DESC
LIMIT 5;
SELECT a.Name
FROM Actors a
JOIN MovieActors ma ON a.ActorID = ma.ActorID
GROUP BY a.ActorID
HAVING COUNT(ma.MovieID) > 3;
SELECT g.GenreName, AVG(r.Rating) AS AvgRating
FROM Reviews r
JOIN Movies m ON r.MovieID = m.MovieID
JOIN Genres g ON m.GenreID = g.GenreID
GROUP BY g.GenreID;
SELECT m.Title, d.Name AS Director
FROM Movies m
JOIN Directors d ON m.DirectorID = d.DirectorID
WHERE m.ReleaseYear > 2000;
SELECT u.Name, COUNT(r.ReviewID) AS ReviewCount
FROM Users u
JOIN Reviews r ON u.UserID = r.UserID
GROUP BY u.UserID
ORDER BY ReviewCount DESC
LIMIT 1;





