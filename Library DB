> This file contains my SQL solutions to the scenarios in the LinkedIn Learning course "SQL Code Challenges" by Scott Simpson
> This file is specific to the problems for the Library database, which can be found under Exercise Files in the course

#1
**Find out how many copies of Dracula the library owns, and how many are available for checkout**
SELECT
  (SELECT COUNT(Title) FROM Books WHERE Title = "Dracula")
  -
  (SELECT COUNT(Books.Title)
  FROM Loans JOIN Books ON Loans.BookID = Books.BookID
  WHERE Title = "Dracula" AND ReturnedDate IS NULL;)
AS AvailableBooks;


#2
**Add 2 new books to the library**
INSERT INTO Books
	(Title, Author, Published, Barcode)
	VALUES
	("Dracula", "Bram Stoker", "1897", "4819277482"), 
	("Gulliver's Travels", "Jonathan Swift", "1729", "4899254401")
  
  
#3
**Check out 2 books to a certain patron by adding them to the Loans table**
INSERT INTO Loans
	(BookID, PatronID, LoanDate, DueDate)
	VALUES
	((SELECT BookID FROM Books WHERE Barcode = "2855934983"), (SELECT PatronID FROM Patrons WHERE Email = "jvaan@wisdompets.com"), "2020-08-25", "2020-09-08"),
	((SELECT BookID FROM Books WHERE Barcode = "4043822646"), (SELECT PatronID FROM Patrons WHERE Email = "jvaan@wisdompets.com"), "2020-08-25", "2020-09-08")
  

#4
**Generate a report with books due back on July 13, 2020 and patron contact information**
SELECT Loans.DueDate, Books.Title, Patrons.FirstName, Patrons.LastName, Patrons.Email
FROM Loans
JOIN Books ON Loans.BookID = Books.BookID
JOIN Patrons ON Loans.PatronID = Patrons.PatronID
WHERE Loans.DueDate = "2020-07-13" AND Loans.ReturnedDate IS NULL;


#5
**Update Loans table to add a returned date for books that have been turned in**
**Book 1**
UPDATE Loans
SET ReturnedDate = "2020-07-05"
WHERE BookID=(SELECT BookID From Books WHERE Barcode="6435968624" AND ReturnedDate IS NULL)

**Book 2**
UPDATE Loans
SET ReturnedDate = "2020-07-05"
WHERE BookID=(SELECT BookID From Books WHERE Barcode="5677520613" AND ReturnedDate IS NULL)

**Book 3**
UPDATE Loans
SET ReturnedDate = "2020-07-05"
WHERE BookID=(SELECT BookID From Books WHERE Barcode="8730298424" AND ReturnedDate IS NULL)


#6
**Generate a report showing the 10 patrons who have checked out the fewest books**
SELECT COUNT(Loans.LoanID) AS no_of_loans, Patrons.FirstName, Patrons.Email
FROM Loans
JOIN Patrons ON Loans.PatronID = Patrons.PatronID
GROUP BY Patrons.PatronID
ORDER BY no_of_loans ASC
LIMIT 10;


#7
**Generate a report showing books published in the 1890s that are available to be checked out**
SELECT Books.Title, Books.Author, Books.Published
FROM Books
JOIN Loans ON Books.BookID = Loans.BookID
WHERE (Books.Published BETWEEN "1890" AND "1899") AND Loans.ReturnedDate IS NOT NULL
GROUP BY Books.BookID
ORDER BY Books.Title;


#8
**Create 2 reports about book statistics**
**Create a report showing how many books in the library were published each year, sorted most to least**
SELECT Published, COUNT(DISTINCT(Title)) AS pub_count 
FROM Books
GROUP BY Published
ORDER BY pub_count DESC;

**Create a report showing the 5 most popular books patrons have checked out**
SELECT COUNT(Loans.LoanID) AS loan_count, Books.Title
FROM Loans
JOIN Books ON Loans.BookID = Books.BookID
GROUP BY Books.Title
ORDER BY loan_count DESC
LIMIT 5;
