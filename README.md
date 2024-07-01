CREATE DATABASE LibraryManagement;
USE LibraryManagement;
CREATE TABLE Books (
    BookID INT PRIMARY KEY AUTO_INCREMENT,
    Title VARCHAR(255) NOT NULL,
    Author VARCHAR(255) NOT NULL,
    Publisher VARCHAR(255),
    YearPublished YEAR,
    ISBN VARCHAR(13) UNIQUE NOT NULL,
    Genre VARCHAR(100),
    CopiesAvailable INT DEFAULT 1
);
CREATE TABLE Members (
    MemberID INT PRIMARY KEY AUTO_INCREMENT,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100) NOT NULL,
    Address VARCHAR(255),
    PhoneNumber VARCHAR(15),
    Email VARCHAR(100) UNIQUE NOT NULL,
    JoinDate DATE DEFAULT CURDATE()
);
CREATE TABLE Loans (
    LoanID INT PRIMARY KEY AUTO_INCREMENT,
    BookID INT NOT NULL,
    MemberID INT NOT NULL,
    LoanDate DATE DEFAULT CURDATE(),
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID)
);
INSERT INTO Books (Title, Author, Publisher, YearPublished, ISBN, Genre, CopiesAvailable) VALUES
('1984', 'George Orwell', 'Secker & Warburg', 1949, '9780451524935', 'Dystopian', 3),
('To Kill a Mockingbird', 'Harper Lee', 'J.B. Lippincott & Co.', 1960, '9780060935467', 'Fiction', 2),
('The Great Gatsby', 'F. Scott Fitzgerald', 'Charles Scribner\'s Sons', 1925, '9780743273565', 'Tragedy', 5);
INSERT INTO Members (FirstName, LastName, Address, PhoneNumber, Email) VALUES
('John', 'Doe', '123 Maple St.', '555-1234', 'johndoe@example.com'),
('Jane', 'Smith', '456 Oak St.', '555-5678', 'janesmith@example.com');
SELECT * FROM Books;
SELECT * FROM Members;
SELECT * FROM Loans;
UPDATE Books
SET CopiesAvailable = CopiesAvailable - 1
WHERE BookID = 1;
UPDATE Loans
SET ReturnDate = CURDATE()
WHERE LoanID = 1;

UPDATE Books
SET CopiesAvailable = CopiesAvailable + 1
WHERE BookID = (SELECT BookID FROM Loans WHERE LoanID = 1);
DELETE FROM Members
WHERE MemberID = 2;
DELETE FROM Books
WHERE BookID = 3;
ALTER TABLE Loans
ADD COLUMN OverdueStatus BOOLEAN DEFAULT FALSE,
ADD COLUMN FineAmount DECIMAL(5, 2) DEFAULT 0.00;

-- Example: Mark a book as overdue and set a fine
UPDATE Loans
SET OverdueStatus = TRUE, FineAmount = 5.00
WHERE LoanID = 1 AND ReturnDate IS NULL AND LoanDate < DATE_SUB(CURDATE(), INTERVAL 30 DAY);
CREATE TABLE Reservations (
    ReservationID INT PRIMARY KEY AUTO_INCREMENT,
    BookID INT NOT NULL,
    MemberID INT NOT NULL,
    ReservationDate DATE DEFAULT CURDATE(),
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID)
);

-- Example: Add a reservation
INSERT INTO Reservations (BookID, MemberID) VALUES (1, 1);

