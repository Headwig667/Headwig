CREATE TABLE MembershipTiers (TierID INT PRIMARY KEY, TierName VARCHAR(50), MaxLoans INT, MaxDigitalAccess INT);
CREATE TABLE Members (MemberID INT PRIMARY KEY, Name VARCHAR(100), Address VARCHAR(200), Phone VARCHAR(15), Email VARCHAR(100), TierID INT, DateOfBirth DATE, FOREIGN KEY (TierID) REFERENCES MembershipTiers(TierID));
CREATE TABLE Resources (ResourceID INT PRIMARY KEY, Title VARCHAR(100), Author VARCHAR(100), Type VARCHAR(50), Format VARCHAR(50), AvailabilityStatus VARCHAR(20));
CREATE TABLE Loans (LoanID INT PRIMARY KEY, MemberID INT, ResourceID INT, LoanDate DATE, ReturnDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));
CREATE TABLE DigitalAccess (AccessID INT PRIMARY KEY, MemberID INT, ResourceID INT, AccessDate DATE, ExpiryDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));

INSERT INTO MembershipTiers VALUES (1, 'Basic', 3, 2), (2, 'Premium', 7, 5), (3, 'Elite', 10, 10);
INSERT INTO Members (MemberID, Name, Address, Phone, Email, TierID) VALUES (101, 'Alice', '123 Maple St', '1234567890', 'alice@mail.com', 2), (102, 'Bob', '456 Oak St', '2345678901', 'bob@mail.com', 1), (103, 'Charlie', '789 Pine St', '3456789012', 'charlie@mail.com', 2);
INSERT INTO Resources VALUES (201, 'Data Structures', 'Mark Allen', 'Book', 'Hardcover', 'Available'), (202, 'Machine Learning', 'Andrew Ng', 'Book', 'Paperback', 'Available'), (203, 'Database Systems', 'Elmasri', 'Book', 'PDF', 'Available'), (204, 'Operating Systems', 'Galvin', 'Book', 'eBook', 'Available');
INSERT INTO Loans VALUES (1, 101, 201, '2025-04-01', '2025-04-10'), (2, 101, 202, '2025-04-02', '2025-04-11'), (3, 101, 203, '2025-04-03', '2025-04-12'), (4, 101, 204, '2025-04-04', '2025-04-13'), (5, 101, 202, '2025-04-05', '2025-04-14'), (6, 101, 203, '2025-04-06', '2025-04-15');
INSERT INTO DigitalAccess VALUES (1, 102, 203, '2025-04-01', '2025-05-01'), (2, 103, 204, '2025-04-01', '2025-05-01');

SELECT * FROM Members WHERE TierID = 2;

SELECT MemberID, COUNT(*) AS TotalLoans FROM Loans GROUP BY MemberID HAVING COUNT(*) > 5;

SELECT SUM(MaxLoans) AS TotalMaxLoans FROM MembershipTiers;

SELECT Members.Name, Loans.LoanDate FROM Members JOIN Loans ON Members.MemberID = Loans.MemberID;

DELIMITER //
CREATE TRIGGER before_loan_insert
BEFORE INSERT ON Loans
FOR EACH ROW
BEGIN
    UPDATE Resources
    SET AvailabilityStatus = 'Not Available'
    WHERE ResourceID = NEW.ResourceID;
END;
//
DELIMITER ;

ALTER TABLE Members ADD COLUMN DateOfBirth date;


CREATE TABLE MembershipTiers (TierID INT PRIMARY KEY, TierName VARCHAR(50), MaxLoans INT, MaxDigitalAccess INT);
CREATE TABLE Members (MemberID INT PRIMARY KEY, Name VARCHAR(100), Address VARCHAR(200), Phone VARCHAR(20), Email VARCHAR(100), TierID INT, FOREIGN KEY (TierID) REFERENCES MembershipTiers (TierID));
CREATE TABLE Resources (ResourceID INT PRIMARY KEY, Title VARCHAR(100), Author VARCHAR(100), Type VARCHAR(50), Format VARCHAR(50), AvailabilityStatus VARCHAR(20));
CREATE TABLE Loans (LoanID INT PRIMARY KEY, MemberID INT, ResourceID INT, LoanDate DATE, ReturnDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));
CREATE TABLE DigitalAccess (AccessID INT PRIMARY KEY, MemberID INT, ResourceID INT, AccessDate DATE, ExpiryDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));

INSERT INTO MembershipTiers VALUES (1, 'Basic', 3, 2), (2, 'Premium', 5, 5);
INSERT INTO Members VALUES (1, 'Alice', '123 Apple St', '1234567890', 'alice@example.com', 1), (2, 'Bob', '456 Banana Ave', '2345678901', 'bob@example.com', 2), (3, 'Charlie', '789 Cherry Blvd', '3456789012', 'charlie@example.com', 1);
INSERT INTO Resources VALUES (101, 'SQL for Beginners', 'John Doe', 'Book', 'Physical', 'Available'), (102, 'Advanced SQL', 'Jane Smith', 'Book', 'PDF', 'Borrowed'), (103, 'Tech Monthly', 'Tech Group', 'Magazine', 'Digital', 'Available'), (104, 'Science Today', 'Science Org', 'Journal', 'Physical', 'Available');
INSERT INTO Loans VALUES (1, 1, 101, '2024-03-01', '2024-03-15'), (2, 2, 102, '2024-04-01', NULL);
INSERT INTO DigitalAccess VALUES (1, 1, 103, '2024-04-01', '2024-04-30'), (2, 3, 104, '2024-04-05', '2024-05-05');

SELECT * FROM Resources WHERE Type IN ('Book', 'Magazine');

SELECT * FROM Members ORDER BY Name ASC;

SELECT TierID, COUNT(*) AS TotalMembers FROM Members GROUP BY TierID;

DELIMITER //
CREATE PROCEDURE GetMemberLoans(IN memID INT)
BEGIN
    SELECT * FROM Loans WHERE MemberID = memID;
END;
//
DELIMITER ;
CALL GetMemberLoans(1);

UPDATE Members SET Phone = '9876543210' WHERE MemberID = 1;

SELECT Loans.*, Members.Name, Members.Email FROM Loans LEFT JOIN Members ON Loans.MemberID = Members.MemberID;


CREATE TABLE MembershipTiers (TierID INT PRIMARY KEY, TierName VARCHAR(50), MaxLoans INT, MaxDigitalAccess INT);
CREATE TABLE Members (MemberID INT PRIMARY KEY, Name VARCHAR(100), Address VARCHAR(255), Phone VARCHAR(15), Email VARCHAR(100), TierID INT, FOREIGN KEY (TierID) REFERENCES MembershipTiers(TierID));
CREATE TABLE Resources (ResourceID INT PRIMARY KEY, Title VARCHAR(200), Author VARCHAR(100), Type VARCHAR(50), Format VARCHAR(50), AvailabilityStatus VARCHAR(20));
CREATE TABLE Loans (LoanID INT PRIMARY KEY, MemberID INT, ResourceID INT, LoanDate DATE, ReturnDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));
CREATE TABLE DigitalAccess (AccessID INT PRIMARY KEY, MemberID INT, ResourceID INT, AccessDate DATE, ExpiryDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));

INSERT INTO MembershipTiers VALUES (1, 'Basic', 2, 3), (2, 'Premium', 5, 7);
INSERT INTO Members VALUES (101, 'Alice', '123 Street', '1111111111', 'alice@example.com', 1), (102, 'Bob', '456 Avenue', '2222222222', 'bob@example.com', 2), (103, 'Carol', '789 Road', '3333333333', 'carol@example.com', 2);
INSERT INTO Resources VALUES (1, 'Database Systems', 'Elmasri', 'Book', 'PDF', 'Not Available'), (2, 'Computer Networks', 'Tanenbaum', 'Book', 'ePub', 'Available'), (3, 'AI Basics', 'Russell', 'Book', 'PDF', 'Not Available'), (4, 'Cloud Computing', 'Armbrust', 'Book', 'PDF', 'Available');
INSERT INTO Loans VALUES (201, 101, 1, '2025-04-01', NULL), (202, 102, 3, '2025-04-05', NULL);
INSERT INTO DigitalAccess VALUES (301, 101, 2, '2025-03-10', '2025-04-30'), (302, 102, 4, '2025-04-01', '2025-04-10'), (303, 103, 2, '2025-03-15', '2025-04-15'), (304, 103, 4, '2025-03-15', '2025-03-25'), (305, 103, 1, '2025-03-15', '2025-04-20');

SELECT * FROM DigitalAccess WHERE ExpiryDate >= CURDATE();

SELECT MemberID, COUNT(*) AS AccessCount FROM DigitalAccess GROUP BY MemberID HAVING COUNT(*) > 2;

SELECT AVG(MaxDigitalAccess) AS AvgDigitalLimit FROM MembershipTiers;

SELECT Resources.Title, Loans.MemberID, Loans.LoanDate FROM Resources LEFT JOIN Loans ON Resources.ResourceID = Loans.ResourceID;

DELIMITER //
CREATE TRIGGER after_loan_delete
AFTER DELETE ON Loans
FOR EACH ROW
BEGIN
    UPDATE Resources
    SET AvailabilityStatus = 'Available'
    WHERE ResourceID = OLD.ResourceID;
END;
//
DELIMITER ;

DELIMITER //
CREATE PROCEDURE GetResourcesByAvailability(IN status VARCHAR(20))
BEGIN
    SELECT ResourceID, Title, Author, Type, Format FROM Resources WHERE AvailabilityStatus = status;
END;
//
DELIMITER ;
CALL GetResourcesByAvailability('Available');

CREATE TABLE MembershipTiers (TierID INT PRIMARY KEY, TierName VARCHAR(50), MaxLoans INT, MaxDigitalAccess INT);
CREATE TABLE Members (MemberID INT PRIMARY KEY, Name VARCHAR(100), Address VARCHAR(255), Phone VARCHAR(15), Email VARCHAR(100), TierID INT, FOREIGN KEY (TierID) REFERENCES MembershipTiers (TierID));
CREATE TABLE Resources (ResourceID INT PRIMARY KEY, Title VARCHAR(100), Author VARCHAR(100), Type VARCHAR(50), Format VARCHAR(50), AvailabilityStatus VARCHAR(20));
CREATE TABLE Loans (LoanID INT PRIMARY KEY, MemberID INT, ResourceID INT, LoanDate DATE, ReturnDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));
CREATE TABLE DigitalAccess (AccessID INT PRIMARY KEY, MemberID INT, ResourceID INT, AccessDate DATE, ExpiryDate DATE, FOREIGN KEY (MemberID) REFERENCES Members(MemberID), FOREIGN KEY (ResourceID) REFERENCES Resources(ResourceID));

INSERT INTO MembershipTiers VALUES (1, 'Basic', 2, 5), (2, 'Premium', 5, 10);
INSERT INTO Members VALUES (101, 'Alice', '123 Street', '1234567890', 'alice@example.com', 1), (102, 'Bob', '456 Avenue', '0987654321', 'bob@example.com', 2);
INSERT INTO Resources VALUES (201, 'SQL Basics', 'John Doe', 'Book', 'Physical', 'Available'), (202, 'Advanced SQL', 'Jane Roe', 'Book', 'Digital', 'Unavailable'), (203, 'AI Journal', 'Dr. Smith', 'Journal', 'Digital', 'Available');
INSERT INTO Loans VALUES (301, 101, 201, '2025-04-01', '2025-04-10');
INSERT INTO DigitalAccess VALUES (401, 101, 202, '2025-04-02', '2025-05-02'), (402, 102, 203, '2025-04-03', '2025-05-03');

SELECT Type, Format, COUNT(*) AS TotalResources FROM Resources GROUP BY Type, Format;

SELECT m.Name, t.TierName FROM Members m LEFT JOIN MembershipTiers t ON m.TierID = t.TierID;

SELECT * FROM Resources WHERE AvailabilityStatus = 'Unavailable';

DELIMITER //
CREATE FUNCTION CountAvailableResourcesByFormat(resFormat VARCHAR(20))
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total FROM Resources WHERE Format = resFormat AND AvailabilityStatus = 'Available';
    RETURN total;
END; 
//
DELIMITER ;
SELECT CountAvailableResourcesByFormat('Digital') AS DigitalAvailableResources;

DELIMITER //
CREATE PROCEDURE ListResourcesByAvailability(IN status VARCHAR(20))
BEGIN
    SELECT * FROM Resources WHERE AvailabilityStatus = status;
END;
//
DELIMITER ;
CALL ListResourcesByAvailability('Available');

SELECT ResourceID, COUNT(*) AS TotalAccesses FROM DigitalAccess GROUP BY ResourceID;





CREATE TABLE Departments (DepartmentID INT PRIMARY KEY AUTO_INCREMENT, DepartmentName VARCHAR(100), Location VARCHAR(100));
CREATE TABLE Doctors (DoctorID INT PRIMARY KEY AUTO_INCREMENT, DoctorName VARCHAR(100), Specialty VARCHAR(100), DepartmentID INT, FOREIGN KEY (DepartmentID) REFERENCES Departments (DepartmentID));
CREATE TABLE Patients (PatientID INT PRIMARY KEY AUTO_INCREMENT, PatientName VARCHAR(100), DOB DATE, Address VARCHAR(200), PhoneNumber VARCHAR(15));
CREATE TABLE Appointments (AppointmentID INT PRIMARY KEY AUTO_INCREMENT, PatientID INT, DoctorID INT, AppointmentDate DATE, CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP, FOREIGN KEY (PatientID) REFERENCES Patients(PatientID), FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID));

INSERT INTO Departments (DepartmentName, Location) VALUES ('Cardiology', 'Block A'), ('Neurology', 'Block B'), ('Orthopedics', 'Block C'), ('General Medicine', 'Block D');
INSERT INTO Doctors (DoctorName, Specialty, DepartmentID) VALUES ('Dr. Smith', 'Cardiology', 1), ('Dr. Rose', 'Neurology', 2), ('Dr. Allen', 'Orthopedics', 3), ('Dr. Jake', 'General Medicine', 4), ('Dr. Sam', 'Cardiology', 1);
INSERT INTO Patients (PatientName, DOB, Address, PhoneNumber) VALUES ('Alice', '2000-05-14', '123 Street, NY', '1111111111'), ('Bob', '1990-09-10', '456 Lane, LA', '2222222222'), ('Charlie', '1985-12-25', '789 Avenue, TX', '3333333333');
INSERT INTO Appointments (PatientID, DoctorID, AppointmentDate) VALUES (1, 1, CURDATE()), (2, 2, CURDATE()), (3, 3, '2024-12-01');

SELECT * FROM Doctors WHERE Specialty IN ('Cardiology', 'Neurology', 'Orthopedics');

SELECT * FROM Patients ORDER BY DOB DESC;

SELECT COUNT(*) AS TodayAppointments FROM Appointments WHERE DATE(AppointmentDate) = CURDATE();

SELECT D.DoctorName, Dept.DepartmentName FROM Doctors D JOIN Departments Dept ON D.DepartmentID = Dept.DepartmentID;

DELIMITER //
CREATE PROCEDURE AddNewPatient(IN p_name VARCHAR(100), IN p_dob DATE, IN p_address VARCHAR(255), IN p_phone VARCHAR(15))
BEGIN
    INSERT INTO Patients (PatientName, DOB, Address, PhoneNumber) VALUES (p_name, p_dob, p_address, p_phone);
END;
//
DELIMITER ;
CALL AddNewPatient('Daisy', '1995-08-20', '12 Rose St, SF', '4444444444');
SELECT * FROM Patients;

DELIMITER //
CREATE TRIGGER trg_default_doctor_department
BEFORE INSERT ON Doctors
FOR EACH ROW
BEGIN
    IF NEW.DepartmentID IS NULL THEN
        SET NEW.DepartmentID = 1;
    END IF;
END;
//
DELIMITER ;
INSERT INTO Doctors (DoctorName, Specialty) VALUES ('Dr. John', 'Dermatology');
SELECT * FROM Doctors;


CREATE TABLE Departments (DepartmentID INT PRIMARY KEY, DepartmentName VARCHAR(100), Location VARCHAR(100));
CREATE TABLE Doctors (DoctorID INT PRIMARY KEY, DoctorName VARCHAR(100), Specialty VARCHAR(100), DepartmentID INT, FOREIGN KEY (DepartmentID) REFERENCES Departments (DepartmentID));
CREATE TABLE Patients (PatientID INT PRIMARY KEY, PatientName VARCHAR(100), DOB DATE, Address VARCHAR(255), PhoneNumber VARCHAR(15));
CREATE TABLE Appointments (AppointmentID INT PRIMARY KEY, PatientID INT, DoctorID INT, AppointmentDate DATE, FOREIGN KEY (PatientID) REFERENCES Patients (PatientID), FOREIGN KEY (DoctorID) REFERENCES Doctors (DoctorID));

INSERT INTO Departments VALUES (1, 'Cardiology', 'Block A'), (2, 'Neurology', 'Block B'), (3, 'Orthopedics', 'Block C');
INSERT INTO Doctors VALUES (101, 'Dr. Smith', 'Cardiologist', 1), (102, 'Dr. John', 'Neurologist', 2), (103, 'Dr. Lisa', 'Orthopedic Surgeon', 3), (104, 'Dr. James', 'Cardiologist', 1);
INSERT INTO Patients VALUES (201, 'Alice Brown', '1980-05-20', '123 Elm St', '555-1234'), (202, 'Bob Smith', '1975-09-15', '456 Oak St', '555-5678');
INSERT INTO Appointments VALUES (301, 201, 101, '2025-04-01'), (302, 202, 102, '2025-04-02'), (303, 201, 104, '2025-04-03');

SELECT DoctorName, Specialty, DepartmentID FROM Doctors ORDER BY Specialty ASC;

SELECT DepartmentID, COUNT(*) AS DoctorCount FROM Doctors GROUP BY DepartmentID;

SELECT DoctorID, COUNT(*) AS AppointmentCount FROM Appointments GROUP BY DoctorID;

SELECT p.PatientName, a.AppointmentDate FROM Patients p JOIN Appointments a ON p.PatientID = a.PatientID;

DELIMITER //
CREATE PROCEDURE GetDoctorInfo(IN doctor_id INT)
BEGIN
    SELECT DoctorName, Specialty FROM Doctors WHERE DoctorID = doctor_id;
END //
DELIMITER ;
CALL GetDoctorInfo(101);

DELIMITER //
CREATE TRIGGER SetAppointmentDate
BEFORE INSERT ON Appointments
FOR EACH ROW
BEGIN
    IF NEW.AppointmentDate IS NULL THEN
        SET NEW.AppointmentDate = CURRENT_DATE;
    END IF;
END //
DELIMITER ;
INSERT INTO Appointments (AppointmentID, PatientID, DoctorID) VALUES (304, 202, 103);
SELECT * FROM Appointments;


CREATE TABLE Patients (PatientID INT PRIMARY KEY, PatientName VARCHAR(100), Age INT, Address VARCHAR(255), PhoneNumber VARCHAR(15));
CREATE TABLE Departments (DepartmentID INT PRIMARY KEY, DepartmentName VARCHAR(100), Location VARCHAR(100));
CREATE TABLE Doctors (DoctorID INT PRIMARY KEY, DoctorName VARCHAR(100), Specialty VARCHAR(100), DepartmentID INT, FOREIGN KEY (DepartmentID) REFERENCES Departments (DepartmentID));
CREATE TABLE Appointments (AppointmentID INT PRIMARY KEY, PatientID INT, DoctorID INT, AppointmentDate DATE, FOREIGN KEY (PatientID) REFERENCES Patients (PatientID), FOREIGN KEY (DoctorID) REFERENCES Doctors (DoctorID));

INSERT INTO Patients VALUES (1, 'John Doe', 70, '123 Elm Street', '1234567890'), (2, 'Jane Smith', 30, '456 Oak Street', '0987654321'), (3, 'Mike Johnson', 67, '789 Pine Street', '1122334455'), (4, 'Alice Brown', 25, '321 Maple Street', '6677889900');
INSERT INTO Departments VALUES (1, 'Cardiology', 'Block A'), (2, 'Neurology', 'Block B'), (3, 'Pediatrics', 'Block C');
INSERT INTO Doctors VALUES (1, 'Dr. Adams', 'Cardiologist', 1), (2, 'Dr. Baker', 'Neurologist', 2), (3, 'Dr. Clark', 'Pediatrician', 3);
INSERT INTO Appointments VALUES (1, 1, 1, '2025-04-01'), (2, 2, 2, '2025-04-03'), (3, 3, 1, '2025-04-05'), (4, 4, 3, '2025-04-07');

SELECT PatientID, PatientName, Age, PhoneNumber FROM Patients WHERE Age > 65;

SELECT AVG(Age) AS AveragePatientAge FROM Patients;

SELECT d.DoctorName, dept.DepartmentName FROM Doctors d JOIN Departments dept ON d.DepartmentID = dept.DepartmentID;

DELIMITER //
CREATE PROCEDURE GetDoctorAppointments(IN doctor_id_param INT)
BEGIN
    SELECT AppointmentID, PatientID, AppointmentDate FROM Appointments WHERE DoctorID = doctor_id_param;
END //
DELIMITER ;
CALL GetDoctorAppointments(1);

SELECT PatientName, Age FROM Patients ORDER BY Age ASC LIMIT 1;

DELIMITER //
CREATE PROCEDURE ShowSeniorPatients()
BEGIN
    DECLARE done INT DEFAULT 0;
    DECLARE pName VARCHAR(100);
    DECLARE pAge INT;
    DECLARE pPhone VARCHAR(15);
    DECLARE patient_cursor CURSOR FOR SELECT PatientName, Age, PhoneNumber FROM Patients;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    
    OPEN patient_cursor;
    read_loop: LOOP
        FETCH patient_cursor INTO pName, pAge, pPhone;
        IF done THEN
            LEAVE read_loop;
        END IF;
        IF pAge > 60 THEN
            SELECT CONCAT('Name: ', pName, ' | Phone: ', pPhone) AS SeniorCitizen;
        END IF;
    END LOOP;
    CLOSE patient_cursor;
END //
DELIMITER ;
CALL ShowSeniorPatients();



CREATE TABLE Departments (DepartmentID INT PRIMARY KEY, DepartmentName VARCHAR(100), Location VARCHAR(100));
CREATE TABLE Doctors (DoctorID INT PRIMARY KEY, DoctorName VARCHAR(100), Specialty VARCHAR(100), DepartmentID INT, FOREIGN KEY (DepartmentID) REFERENCES Departments (DepartmentID));
CREATE TABLE Patients (PatientID INT PRIMARY KEY, PatientName VARCHAR(100), Age INT, Address VARCHAR(200), PhoneNumber VARCHAR(15));
CREATE TABLE Appointments (AppointmentID INT PRIMARY KEY, PatientID INT, DoctorID INT, AppointmentDate DATE, FOREIGN KEY (PatientID) REFERENCES Patients(PatientID), FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID));

INSERT INTO Departments VALUES (1, 'Cardiology', 'Block A'), (2, 'Neurology', 'Block B'), (3, 'Orthopedics', 'Block C');
INSERT INTO Doctors VALUES (101, 'Dr. Smith', 'Cardiologist', 1), (102, 'Dr. Adams', 'Neurologist', 2), (103, 'Dr. Ray', 'Orthopedic Surgeon', 3);
INSERT INTO Patients VALUES (201, 'John Doe', 45, '123 Elm St', '1234567890'), (202, 'Jane Roe', 32, '456 Oak St', '9876543210'), (203, 'Mary Lane', 60, '789 Pine St', '4561237890'), (204, 'Peter Hall', 50, '321 Birch St', '3216549870'), (205, 'Lucy Gray', 29, '654 Maple St', '7418529630');
INSERT INTO Appointments VALUES (301, 201, 101, '2025-04-01'), (302, 202, 101, '2025-04-02'), (303, 203, 103, '2025-04-03'), (304, 204, 102, '2025-04-04'), (305, 201, 101, '2025-04-05');

SELECT d.DoctorName, d.Specialty FROM Doctors d JOIN Departments dept ON d.DepartmentID = dept.DepartmentID WHERE dept.DepartmentName = 'Cardiology';

SELECT PatientName, Age FROM Patients WHERE Age BETWEEN 30 AND 50;

SELECT PatientName, Age FROM Patients ORDER BY Age DESC LIMIT 3;

DELIMITER //
CREATE PROCEDURE GetPatientAgeGroups()
BEGIN
    SELECT 
        CASE 
            WHEN Age < 18 THEN 'Under 18'
            WHEN Age BETWEEN 18 AND 30 THEN '18-30'
            WHEN Age BETWEEN 31 AND 50 THEN '31-50'
            WHEN Age BETWEEN 51 AND 70 THEN '51-70'
            ELSE 'Over 70' 
        END AS AgeGroup, 
        COUNT(*) AS PatientCount 
    FROM Patients 
    GROUP BY AgeGroup 
    ORDER BY MIN(Age);
END//
DELIMITER ;
CALL GetPatientAgeGroups();

CREATE VIEW PatientDirectory AS SELECT PatientID, PatientName, Age, PhoneNumber FROM Patients;
SELECT * FROM PatientDirectory;

DELIMITER //
CREATE FUNCTION CountAppointments(p_id INT) 
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE appt_count INT;
    SELECT COUNT(*) INTO appt_count FROM Appointments WHERE PatientID = p_id;
    RETURN appt_count;
END //
DELIMITER ;
SELECT PatientName, CountAppointments(PatientID) AS AppointmentCount FROM Patients;



* agents table

SELECT AGENT_NAME, WORKING_AREA, COMMISSION 
FROM agents 
ORDER BY AGENT_CODE ASC;


* Customer table

SELECT CUST_COUNTRY, GRADE, COUNT(CUSTOMER_ID) AS Num_Customers
FROM customer
GROUP BY CUST_COUNTRY, GRADE
HAVING COUNT(CUSTOMER_ID) > 2;


* supplier table

SELECT Sup_No, Sup_Name FROM Supplier WHERE Sup_Name LIKE 'R%';

SELECT Sup_Name FROM Supplier WHERE Item_Supplied = 'Processor' AND City = 'Delhi';

SELECT Sup_Name FROM Supplier 
WHERE Item_Supplied IN (SELECT Item_Supplied FROM Supplier WHERE Sup_Name = 'Ramesh') 
AND Sup_Name != 'Ramesh';

UPDATE Supplier SET Item_Price = Item_Price + 200 WHERE Item_Supplied = 'Keyboard';

SELECT Sup_No, Sup_Name, Item_Price FROM Supplier 
WHERE City = 'Delhi' 
ORDER BY Item_Price ASC;

ALTER TABLE Supplier ADD CONTACTNO VARCHAR(15);

DELETE FROM Supplier WHERE Item_Price = (SELECT MIN(Item_Price) FROM Supplier);

CREATE VIEW SupplierBasicInfo AS 
SELECT Sup_No, Sup_Name FROM Supplier;

SELECT * FROM Supplier ORDER BY Item_Supplied, Item_Price DESC;

SELECT * FROM Supplier WHERE Item_Supplied NOT IN ('Processor', 'Keyboard');



* employee and department table

SELECT * FROM Employee 
WHERE Salary > (SELECT AVG(Salary) FROM Employee);

SELECT e.Eid, e.Ename, d.Dname 
FROM Employee e 
JOIN Department d ON e.DeptId = d.DeptId;

SELECT DISTINCT Designation FROM Employee;

SELECT * FROM Employee 
ORDER BY DeptId, Salary ASC;

SELECT * FROM Employee 
WHERE DeptId = 'D2' AND Designation = 'Clerk';

SELECT * FROM Employee WHERE YEAR(JoinDate) = 2011;

SELECT * FROM Employee WHERE MONTH(JoinDate) = 2;

SELECT * FROM Employee WHERE Salary BETWEEN 30000 AND 45000;

SELECT *, TIMESTAMPDIFF(YEAR, JoinDate, CURDATE()) AS ExperienceYears 
FROM Employee;



* student table

SELECT * FROM Student 
WHERE State NOT IN ('Telangana', 'Andhra Pradesh');

CREATE VIEW TelanganaStudents AS
SELECT Sid, Sname FROM Student WHERE State = 'Telangana';

CREATE INDEX idx_sname ON Student(Sname);

SELECT * FROM Student 
WHERE Gender = 'F' AND Course = 'Comp' AND Category = 'OBC';

SELECT Sid, Sname, TIMESTAMPDIFF(YEAR, STR_TO_DATE(DOB, '%d-%b-%y'), CURDATE()) AS Age 
FROM Student;

SELECT * FROM Student 
ORDER BY Course, Sname ASC;

DELETE FROM Student 
WHERE Course = 'Comp' AND STR_TO_DATE(DOB, '%d-%b-%y') > '2002-12-31';

ALTER TABLE Student 
ADD Contactno VARCHAR(15), 
ADD Email VARCHAR(50);

SELECT Sname FROM Student WHERE LENGTH(Sname) = 5;



* library table


SELECT DISTINCT Author FROM Library WHERE Publisher = 'Himalaya';

SELECT Publisher, SUM(Price) AS TotalCost FROM Library GROUP BY Publisher;

SELECT COUNT(*) AS TotalBooks FROM Library WHERE Publisher = 'Kalyani';

ALTER TABLE Library RENAME COLUMN Publisher TO Publications;

SELECT * FROM Library ORDER BY STR_TO_DATE(DatePurchased, '%d-%b-%y') ASC;

CREATE INDEX idx_book_author ON Library(BookName, Author);

SELECT * FROM Library WHERE Price BETWEEN 500 AND 700;

UPDATE Library 
SET Price = Price + 200 
WHERE Publications NOT IN ('Himalaya', 'Kalyani');

SELECT * FROM Library WHERE Author LIKE '%Sharma%';

CREATE VIEW HimalayaBooks AS
SELECT BookId, BookName FROM Library WHERE Publications = 'Himalaya';




* salespeople

CREATE VIEW NewYorkSalespeople AS
SELECT * FROM Salesman 
WHERE CITY = 'New York';



* All sales person view

CREATE VIEW AllSalespersons AS
SELECT SALESMAN_ID, NAME, CITY 
FROM Salesman;



* customer grade count

CREATE VIEW CustomerGradeCount AS
SELECT GRADE, COUNT(CUSTOMER_ID) AS TotalCustomers
FROM Customer
GROUP BY GRADE;


* Avg purchase amount

SELECT AVG(PURCH_AMT) AS Average_Purchase_Amount 
FROM orders;


* unregistered nurses

SELECT EMPLOYEE_ID, NAME, POSITION, REGISTERED, SSN 
FROM nurse 
WHERE REGISTERED = 'FALSE';

* Nurse in charge

SELECT NAME AS "name", POSITION AS "Position" 
FROM nurse 
WHERE POSITION = 'Head Nurse';

* sort agents

SELECT AGENT_NAME, WORKING_AREA, COMMISSION 
FROM agents 
ORDER BY AGENT_CODE ASC;
