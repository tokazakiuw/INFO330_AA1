# Seattle Apartment Rental Market Database
In this project, we intend to work in the apartment rental industry in Seattle to understand market trends regarding property values and rental rates. From our data, we can investigate how properties differ in pricing across neighborhoods, which properties have the most units, which leases have the highest rent, and other questions as well. Each property has one landlord, but landlords can own multiple properties. For each property, there are multiple units. Each unit can have one or more leases and tenants. Each tenant paysrent for theunit. By leveraging this information and database into our project, renters and real estate professionals can make informed decisions that can lead to more successful transactions and better returns on investments.

## Authors
This project was developed by Ty Okazaki, Irene Hu, Joseph Tran, and Rebecca Jessup

# ER Diagram
## Initial Draft
![alt text](https://github.com/tokazakiuw/INFO330_AA1/blob/main/img/ERD%20Draft.png)

## Final Revision
![alt text](https://github.com/tokazakiuw/INFO330_AA1/blob/main/img/ERD%20Revised.png)

## SQL Tables
![alt text](https://github.com/tokazakiuw/INFO330_AA1/blob/main/img/SQL%20Tables.png)

# Code
```
-- Create Schema
-- Drop Tables
DROP TABLE Lease;
DROP TABLE PaymentTransaction;
DROP TABLE Tenant;
DROP TABLE Unit;
DROP TABLE Property;
DROP TABLE Landlord;

-- CREATE TABLES
CREATE TABLE Lease (
	lease_id SERIAL,
	unit_id SERIAL,
	tenant_id SERIAL,
	start_date DATE,
	end_date DATE,
	payment_frequency VARCHAR(200),
	payment_amount MONEY,
	PRIMARY KEY (lease_id, unit_id, tenant_id),
	FOREIGN KEY (unit_id) REFERENCES Unit(unit_id),
	FOREIGN KEY (tenant_id) REFERENCES Tenant(tenant_id)
);


CREATE TABLE PaymentTransaction (
	payment_id SERIAL,
	tenant_id SERIAL,
	date DATE,
	amount MONEY,
	late_fee MONEY,
	payment_desc TEXT,
	PRIMARY KEY (payment_id, tenant_id),
	FOREIGN KEY (tenant_id) REFERENCES Tenant(tenant_id)
);


CREATE TABLE Tenant (
	tenant_id SERIAL,
	unit_id INT,
	full_name VARCHAR(200),
	email VARCHAR(200),
	age_range VARCHAR(200),
	PRIMARY KEY (tenant_id),
	FOREIGN KEY (unit_id) REFERENCES Unit(Unit_id)
);

CREATE TABLE Unit (
	unit_id SERIAL,
	property_id SERIAL,
	num_bath SMALLINT,
	num_bed SMALLINT,
	sqft INT,
	PRIMARY KEY (unit_id, property_id),
	FOREIGN KEY (property_id) REFERENCES Property(property_id)
);

CREATE TABLE Property (
	property_id SERIAL PRIMARY KEY,
	landlord_id INT,
	last_sold_price MONEY,
	year_built DATE,
	address VARCHAR(200),
	current_value MONEY,
	neighborhood VARCHAR(200),
	FOREIGN KEY (landlord_id) REFERENCES Landlord(Landlord_id)
);

CREATE TABLE Landlord (
	landlord_id SERIAL PRIMARY KEY,
	full_name VARCHAR(200),
	email VARCHAR(200)
);

-- INSERT INTO Data for Lease
INSERT INTO Lease (unit_id, tenant_id, start_date, end_date, payment_frequency, payment_amount)
VALUES
	(1, 1, '2022-01-01', '2022-12-31', 'Monthly', 1000),
	(2, 2, '2022-01-01', '2022-12-31', 'Monthly', 1000),
	(3, 3, '2022-02-01', '2023-01-31', 'Monthly', 1200),
	(4, 4, '2022-02-01', '2023-01-31', 'Monthly', 1200),
	(5, 5, '2022-03-01', '2023-02-28', 'Monthly', 1500),
	(6, 6, '2022-03-01', '2023-02-28', 'Monthly', 1500),
	(7, 7, '2022-01-01', '2022-06-30', 'Monthly', 4500),
	(8, 8, '2022-01-01', '2022-03-31', 'Monthly', 3500),
	(9, 9, '2022-02-01', '2024-01-31', 'Monthly', 2000),
	(10, 10, '2022-02-01', '2023-01-31', 'Monthly', 3200),
	(11, 11, '2022-03-01', '2023-02-28', 'Monthly', 2100),
	(12, 12, '2022-03-01', '2023-02-28', 'Monthly', 1650),
	(13, 13, '2022-04-01', '2023-03-31', 'Monthly', 1800),
	(14, 14, '2022-05-01', '2023-04-30', 'Monthly', 1900),
	(15, 15, '2022-06-01', '2023-05-31', 'Monthly', 2000),
	(16, 16, '2022-07-01', '2023-06-30', 'Monthly', 2200),
	(17, 17, '2022-08-01', '2023-07-31', 'Monthly', 2300),
	(18, 18, '2022-09-01', '2023-08-31', 'Monthly', 2400),
	(19, 19, '2022-10-01', '2023-09-30', 'Monthly', 2500),
	(20, 20, '2022-11-01', '2023-10-31', 'Monthly', 2600),
	(21, 21, '2022-12-01', '2023-11-30', 'Monthly', 2700),
	(22, 22, '2023-01-01', '2023-12-31', 'Monthly', 2800),
	(23, 23, '2023-02-01', '2024-01-31', 'Monthly', 2900),
	(24, 24, '2023-03-01', '2024-02-29', 'Monthly', 3000);


-- INSERT INTO Data for PaymentTransactions
INSERT INTO PaymentTransaction (tenant_id, date, amount, late_fee, payment_desc)
VALUES
	(1, '2022-01-15', 1000, 50, 'Rent for January 2022'),
	(2, '2022-01-15', 1000, 50, 'Rent for January 2022'),
	(3, '2022-02-15', 1200, 60, 'Rent for February 2022'),
	(4, '2022-02-15', 1200, 60, 'Rent for February 2022'),
	(5, '2022-03-15', 1500, 75, 'Rent for March 2022'),
	(6, '2022-03-15', 1500, 75, 'Rent for March 2022'),
	(7, '2022-01-15', 4500, 0, 'Rent for January 2022'),
	(8, '2022-01-15', 3500, 0, 'Rent for January 2022'),
	(9, '2022-02-15', 2000, 60, 'Rent for February 2022'),
	(10, '2022-02-15', 3200, 0, 'Rent for February 2022'),
	(11, '2022-03-15', 2100, 75, 'Rent for March 2022'),
	(12, '2022-03-15', 1650, 75, 'Rent for March 2022'),
	(13, '2022-04-15', 1800, 0, 'Rent for April 2022'),
	(14, '2022-05-15', 1900, 0, 'Rent for May 2022'),
	(15, '2022-06-15', 2000, 100, 'Rent for June 2022'),
	(16, '2022-07-15', 2200, 0, 'Rent for July 2022'),
	(17, '2022-08-15', 2300, 0, 'Rent for August 2022'),
	(18, '2022-09-15', 2400, 20, 'Rent for September 2022'),
	(19, '2022-10-15', 2500, 0, 'Rent for October 2022'),
	(20, '2022-11-15', 2600, 10, 'Rent for November 2022'),
	(21, '2022-12-15', 2700, 50, 'Rent for December 2022'),
	(22, '2023-01-15', 2800, 0, 'Rent for January 2023'),
	(23, '2023-02-15', 2900, 300, 'Rent for February 2023'),
	(24, '2023-03-15', 3000, 0, 'Rent for March 2023');


-- INSERT INTO Data for Tenant
INSERT INTO Tenant (unit_id, full_name, email, age_range)
VALUES
	(1, 'John Smith', 'john.smith@gmail.com', '25-34'),
	(2, 'Jane Doe', 'jane.doe@gmail.com', '25-34'),
	(3, 'Bob Johnson', 'bob.johnson@gmail.com', '35-44'),
	(4, 'Alice Lee', 'alice.lee@gmail.com', '18-24'),
	(5, 'David Kim', 'david.kim@gmail.com', '45-54'),
	(6, 'Emily Chen', 'emily.chen@gmail.com', '25-34'),
	(7, 'Madison Martinez', 'madison.martinez@gmail.com', '18-24'),
	(8, 'Ben Reynolds', 'ben.reynolds@gmail.com', '18-24'),
	(9, 'Olivia Nguyen', 'olivia.nguyen@gmail.com', '35-44'),
	(10, 'William Lee', 'william.lee@gmail.com', '18-24'),
	(11, 'Mia Anderson', 'mia.anderson@gmail.com', '45-54'),
	(12, 'Emily Wright', 'emily.wright@gmail.com', '25-34'),
	(13, 'Jake Smith', 'jake.smith@gmail.com', '18-24'),
	(14, 'Sarah Johnson', 'sarah.johnson@gmail.com', '35-44'),
	(15, 'Alex Kim', 'alex.kim@gmail.com', '25-34'),
	(16, 'Lisa Davis', 'lisa.davis@gmail.com', '25-34'),
	(17, 'Maxwell Brown', 'maxwell.brown@gmail.com', '35-44'),
	(18, 'Sophia Perez', 'sophia.perez@gmail.com', '18-24'),
	(19, 'Ethan Garcia', 'ethan.garcia@gmail.com', '25-34'),
	(20, 'Ava Taylor', 'ava.taylor@gmail.com', '18-24');



-- INSERT INTO Data for Unit
INSERT INTO Unit (property_id, num_bath, num_bed, sqft)
VALUES
	(1, 2, 3, 1500),
	(1, 1, 2, 900),
	(2, 2, 2, 1100),
	(2, 1, 1, 700),
	(3, 3, 4, 2000),
	(3, 2, 2, 1200),
	(4, 3, 3, 2500),
	(4, 2, 3, 2200),
	(5, 1, 1, 2000),
	(5, 2, 2, 1800),
	(6, 3, 4, 2100),
	(6, 2, 1, 1650),
	(6, 2, 3, 2500),
	(7, 1, 1, 600),
	(7, 2, 2, 1000),
	(8, 2, 3, 1800),
	(8, 1, 2, 1200),
	(9, 3, 3, 2200),
	(9, 2, 2, 1300),
	(10, 2, 2, 1100),
	(10, 3, 3, 1600),
	(11, 1, 1, 800),
	(11, 2, 2, 1200),
	(12, 3, 4, 2400),
	(12, 1, 1, 700),
	(12, 2, 2, 1000),
	(13, 1, 1, 1850),
	(14, 4, 2, 1960),
	(15, 2, 3, 1705),
	(16, 1, 1, 800),
	(17, 1, 2, 1850),
	(18, 2, 4, 1650),
	(19, 1, 1, 506);


-- INSERT INTO Data for Property
INSERT INTO Property (landlord_id, last_sold_price, year_built, address, current_value, neighborhood)
VALUES
	(1, 200000, '2016-01-01', '1023 East Alder St', 250000, 'Capitol Hill'),
	(2, 300000, '2021-01-01', '4722 Fauntleroy Way SW', 350000, 'West Seattle'),
	(3, 400000, '2010-01-01', '526 Yale Ave N', 500000, 'Capitol Hill'),
	(1, 200000, '2020-01-01', '400 Queen Anne Ave N', 1250000, 'Queen Anne'),
	(1, 300000, '2010-01-01', '701 5th Ave N', 1500000, 'Queen Anne'),
	(3, 500000, '1989-01-01', '210 8th Ave N, Seattle', 750000, 'South Lake Union'),
	(4, 849000, '1999-03-06', '2718 31st Ave S', 1250000, 'Mount Baker'),
	(5, 250000, '2015-01-01', '1205 E Pike St', 350000, 'Capitol Hill'),
	(2, 350000, '2012-01-01', '6501 5th Ave NE', 450000, 'Green Lake'),
	(4, 500000, '1990-01-01', '4400 4th Ave SW', 650000, 'West Seattle'),
	(3, 200000, '2005-01-01', '1000 4th Ave', 800000, 'Downtown Seattle'),
	(6, 450000, '2018-01-01', '1618 15th Ave', 550000, 'Central District'),
	(2, 357000, '2009-08-24', '7346 15th Ave NW', 925000, 'U-District'),
	(2, 900000, '1968-02-21', '6516 47th Ave NE', 975000, 'Ballard'),
	(5, 389900, '1992-10-07', '2225 Eastlake Ave E', 850000, 'Capitol Hill'),
	(5, 292000, '1964-10-03', '3042 34th Ave SW', 799900, 'West Seattle'),
	(6, 520000, '1973-06-24', '716 NW 60th St', 720000, 'Wedgwood'),
	(2, 340000, '1938-03-17', '1015 W Fulton St', 889000, 'Ballard'),
	(2, 4322000, '2020-08-04', '3104 Western Ave', 4400000, 'Downtown Seattle'),
	(4, 600000, '1984-03-21', '3429 33rd Ave W', 1110000, 'Ballard');


-- INSERT INTO Data for Landlord
INSERT INTO Landlord (full_name, email)
VALUES
	('Marley Cox', 'marleycox@gmail.com'),
	('Grayson Simpson', 'graysonsimpson@yahoo.com'),
	('Robert Davis', 'robert.davis@hotmail.com'),
	('Emily Wilson', 'emilywilson@gmail.com'),
	('Adam Patel', 'adam.patel@yahoo.com'),
	('Sarah Kim', 'sarah.kim@hotmail.com');


-- Implement Queries
-- How many units are currently available to rent for each property? (Real estate agent, Landlord, Tenant)
SELECT Property.property_id, Property.address, COUNT(Unit.unit_id) - COUNT(Lease.unit_id) AS vacant_units
FROM Property
JOIN Unit
	ON Property.property_id = Unit.property_id
LEFT JOIN Lease
	ON Unit.unit_id = Lease.unit_id
GROUP BY Property.property_id
ORDER BY vacant_units DESC;

-- Which properties have the highest property value ordered by year built? (Landlord, real estate agent)

SELECT property_id, address, current_value, year_built
FROM Property
ORDER BY current_value DESC, year_built ASC;

-- What are the names and email addresses of tenants that have had to pay late fees? (Landlord)
SELECT full_name, email
FROM Tenant
JOIN PaymentTransaction
	ON Tenant.tenant_id = PaymentTransaction.tenant_id
WHERE late_fee > 0::money
GROUP BY Tenant.tenant_id;

-- What is the average monthly rent for properties in each neighborhood? (Landlord, potential tenant, real estate agent)
SELECT neighborhood, ROUND(AVG(payment_amount::NUMERIC), 2) AS avg_monthly_rent
FROM Property
JOIN Unit
	ON Property.property_id = Unit.property_id
JOIN Lease
	ON Unit.unit_id = Lease.unit_id
GROUP BY neighborhood;

-- How many new tenants start their lease every month? (Landlord)
SELECT DATE_PART('month', start_date) AS lease_month, COUNT(*)
FROM Lease
GROUP BY lease_month;

-- What is the average number of bedrooms across all units that are leased? (Landlord, potential tenant, Real Estate Agent)
SELECT ROUND(AVG(num_bed)) AS avg_num_bedroom
FROM Unit
ORDER BY avg_num_bedroom DESC;

-- How many properties are owned by the same landlord in a certain neighborhood? (Landlord, potential tenant)
SELECT p.neighborhood, l.full_name AS owner, COUNT(*) as num_property
FROM Property p
JOIN Landlord l ON p.landlord_id = l.landlord_id
GROUP BY p.neighborhood, owner
ORDER BY num_property DESC;

-- What is the longest recorded lease period in days and how much did the tenant pay monthly? (Landlord)
SELECT MAX(end_date - start_date) AS longest_lease_days, payment_amount
FROM Lease
GROUP BY payment_amount
ORDER BY longest_lease_days DESC
LIMIT 1;

-- Which landlords have the most properties? (Landlord, developers)
SELECT l.full_name, COUNT(*) AS property_count
FROM Landlord l
JOIN Property p
	ON l.landlord_id = p.landlord_id
GROUP BY l.full_name
ORDER BY property_count DESC;

-- Which neighborhood has the highest number of tenants? (Landlord, Person Renting/Leasing)
SELECT neighborhood, COUNT(*) AS num_tenants
FROM Unit
JOIN Property
	ON Unit.property_id = Property.property_id
GROUP BY neighborhood
ORDER BY num_tenants DESC
LIMIT 5;

-- Demo Queries
-- How many units are currently available to rent for each property? (Real estate agent, Landlord, Tenant)
SELECT Property.property_id, Property.address, COUNT(Unit.unit_id) - COUNT(Lease.unit_id) AS vacant_units
FROM Property
JOIN Unit
	ON Property.property_id = Unit.property_id
LEFT JOIN Lease
	ON Unit.unit_id = Lease.unit_id
GROUP BY Property.property_id
ORDER BY vacant_units DESC
LIMIT 10;


-- Output
-- property_id		address				vacant_units
-- 12			"1618 15th Ave"				2
-- 19			"3104 Western Ave"			1
-- 13			"7346 15th Ave NW"			1
-- 17			"716 NW 60th St"			1
-- 14			"6516 47th Ave NE"			1
-- 16			"3042 34th Ave SW"			1
-- 18			"1015 W Fulton St"			1
-- 15			"2225 Eastlake Ave E"		1
-- 1			"1023 East Alder St"		0
-- 5			"701 5th Ave N"				0


-- What is the average monthly rent for properties in each neighborhood?
SELECT neighborhood, ROUND(AVG(payment_amount::NUMERIC), 2) AS avg_monthly_rent
FROM Property
JOIN Unit
	ON Property.property_id = Unit.property_id
JOIN Lease
	ON Unit.unit_id = Lease.unit_id
GROUP BY neighborhood
LIMIT 10;

-- Output
-- neighborhood 	avg_monthly_rent
-- "Green Lake"			2450.00
-- "Downtown Seattle"	2850.00
-- "Mount Baker"		1950.00
-- "Central District"	3000.00
-- "Queen Anne"			3300.00
-- "South Lake Union"	1850.00
-- "Capitol Hill"		1583.33
-- "West Seattle"		1925.00


-- How many properties are owned by the same landlord in a certain neighborhood?
SELECT p.neighborhood, l.full_name AS owner, COUNT(*) as num_property
FROM Property p
JOIN Landlord l ON p.landlord_id = l.landlord_id
GROUP BY p.neighborhood, owner
ORDER BY num_property DESC
LIMIT 10;

-- Output
-- neighborhood				owner			num_property
-- "Queen Anne"			"Marley Cox"			2
-- "Capitol Hill"		"Adam Patel"			2
-- "Ballard"			"Grayson Simpson"		2
-- "West Seattle"		"Grayson Simpson"		1
-- "Capitol Hill"		"Robert Davis"			1
-- "South Lake Union"	"Robert Davis"			1
-- "Ballard"			"Emily Wilson"			1
-- "Wedgwood"			"Sarah Kim"				1
-- "West Seattle"		"Adam Patel"			1
-- "Capitol Hill"		"Marley Cox"			1


-- Which neighborhood has the highest number of tenants?
SELECT neighborhood, COUNT(*) AS num_tenants
FROM Unit
JOIN Property
	ON Unit.property_id = Property.property_id
GROUP BY neighborhood
ORDER BY num_tenants DESC
LIMIT 10;

-- Output
-- neighborhood		num_tenants
-- "Capitol Hill"		7
-- "West Seattle"		5
-- "Queen Anne"			4
-- "Central District"	3
-- "South Lake Union"	3
-- "Downtown Seattle"	3
-- "Ballard"			2
-- "Mount Baker"		2
-- "Green Lake"			2
-- "Wedgwood"			1
```

# License
This repository is licensed under the MIT license. Please see the LICENSE file for more information.
