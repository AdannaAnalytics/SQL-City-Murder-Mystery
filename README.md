# SQL-City-Murder-Mystery
Helping the residents of SQL City raise funds for its annual local festival while solving a murder in the community!


--STEP 1: CREATING THE DATABASE SQL CITY
--CODE SYNTAX:
CREATE DATABASE &quot;sql city&quot;;

--STEP 2: RESTORING THE SQL CITY DATABASE
/*
1. Right-click on sql city database
2. Select RESTORE...
3. The Restore (Database:sql city) dialogue box opens up
4. Select Custom or tar as the FORMAT
5. Click on the Folder icon within the FILENAME and follow folder path directory to retrieve the
database from where it is stored
6. Click on RESTORE
7. Database is successfully restored once the restoring backup process is started and
completed
*/
--STEP 3: DEFINING PARAMETERS
/*
MY ROLE: Community Database Analyst
EVENT: Annual Local Festival
TASK: Approach Wealthy Members of SQL City for Donations towards the Festival
TO DO: Return the
Name

Address Number
Address Street Name
Annual Income
of the Top 25 Highest Earning Individuals in SQL City

*/

--QUESTION 1 SOLUTION STEPS:
--1. View all records in the PERSON table
--CODE SYNTAX
SELECT *
FROM person;
--2. View all records in the INCOME table
--CODE SYNTAX
SELECT *
FROM income;
--3. Identify matching column
-- ssn

--4. LEFT JOIN person table to income table
--CODE SYNTAX

SELECT person.name,
person.address_number,
person.address_street_name,
income.annual_income

FROM
person
LEFT JOIN
income
ON
income.ssn = person.ssn;

--5. Rename the joined table as &#39;joined_personincome&#39;
--CODE SYNTAX

CREATE TABLE joined_personincome AS
(
SELECT person.name,
person.address_number,
person.address_street_name,
income.annual_income
FROM
person
LEFT JOIN
income
ON
income.ssn = person.ssn
);

--6. View the newly created JOINED_PERSONINCOME table
--CODE SYNTAX
SELECT *
FROM joined_personincome;

--7. Order the ANNUAL INCOME in descending order and limit to top 25 individuals

--CODE SYNTAX
SELECT *
FROM joined_personincome
ORDER BY annual_income desc
LIMIT 25;

--8. Save output as a new table called TOP 25 HIGHEST EARNERS
CREATE TABLE top25_highest_earners AS
(
SELECT *
FROM joined_personincome
ORDER BY annual_income desc
LIMIT 25
);

--6. View the newly created TOP25 HIGHEST EARNERS table
--CODE SYNTAX
SELECT *
FROM top25_highest_earners;

------------------------------------------------------------------------------------
--ALTERNATIVE CODE FOR JOINED PERSON AND INCOME TABLE,
--ORDERED BY INCOME AND LIMITED TO TOP 25 EARNERS
SELECT person.name,
person.address_number,
person.address_street_name,
income.annual_income,

income.ssn
FROM
person
LEFT JOIN
income
ON
income.ssn = person.ssn
ORDER BY annual_income desc
LIMIT 25;
---------------------------------------------------------------------------------------

--QUESTION 2 SOLUTION STEPS:
--1. View all records in the DRIVERS LICENSE table
--CODE SYNTAX
SELECT *
FROM drivers_license;

--2. View COUNT of different brands of cars driven in the community
--CODE SYNTAX
SELECT car_make, COUNT(car_make) AS number_of_people_driving_car_make
FROM drivers_license
GROUP BY car_make
ORDER BY COUNT(car_make) desc;

--3. Save output as CAR MAKE COUNT
--CODE SYNTAX

CREATE TABLE car_make_count AS
(
SELECT car_make, COUNT(car_make) AS number_of_people_driving_car_make
FROM drivers_license
GROUP BY car_make
ORDER BY COUNT(car_make) desc
);

--4. View the newly created CARE MAKE COUNT table
--CODE SYNTAX
SELECT *
FROM car_make_count;

--QUESTION 3 SOLUTION STEPS:
--1. View all records in the DRIVERS LICENSE table
--CODE SYNTAX
SELECT *
FROM drivers_license;

--2. Filter the DRIVERS LICENSE tabel to return ALL
/* Females who have
red hair
are between 65 and 67 inches tall and
drive a Tesla Model S

*/
--CODE SYNTAX
SELECT *
FROM drivers_license
WHERE gender = &#39;female&#39;
AND hair_color = &#39;red&#39;
AND
height BETWEEN 65 AND 67
AND
car_make = &#39;Tesla&#39;
AND
car_model = &#39;Model S&#39;;

--3. Save the output as a new table called FEMALE SUSPECTS
--CODE SYNTAX
CREATE TABLE female_suspects AS (
SELECT *
FROM drivers_license
WHERE gender = &#39;female&#39;
AND hair_color = &#39;red&#39;
AND
height BETWEEN 65 AND 67
AND
car_make = &#39;Tesla&#39;
AND
car_model = &#39;Model S&#39;
);

--4. View the new table FEMALE SUSPECTS
--CODE SYNTAX

SELECT *
FROM female_suspects;
--IMPORTANT NOTES
/*
1. There are 3 female suspects who meet this criteria
i.e. have red hair
has a height between 65&quot; and 67&quot; and
drives a Tesla Model S
2. Their licence_id number are:
202298
291182
918773

However, this is named as simply &#39;id&#39; in the table.

--5. Rename ID column name to LICENSE ID in FEMALE SUSPECTS table
*/

--CODE SYNTAX
ALTER TABLE female_suspects
RENAME COLUMN id TO license_id;

--6. VIEW altered table FEMALE SUSPECTS
SELECT *
FROM female_suspects;

--7. View FACEBOOK EVENT CHECKIN table

SELECT *
FROM facebook_event_checkin;
--CODE SYNTAX
--8. Filter FACEBOOK EVENT CHECKIN table to people who
/*
Attended the SQL Symphony Concert event
3 times in December 2017 (i.e between 1st Dec. - 31st Dec. 2017)
*/

--CODE SYNTAX
SELECT *
FROM facebook_event_checkin
WHERE date BETWEEN 20171201 AND 20171231
AND
event_name =&#39;SQL Symphony Concert&#39;;

--9. Save output as a new table called SQL SYMPHONY SUSPECTS

--CODE SYNTAX
CREATE TABLE sql_symphony_suspects AS (
SELECT *
FROM facebook_event_checkin
WHERE date BETWEEN 20171201 AND 20171231
AND
event_name =&#39;SQL Symphony Concert&#39;
);

--10. VIew SQL SYMPHONY SUSPECTS table
--CODE SYNTAX
SELECT *
FROM sql_symphony_suspects;

--11. Using the COUNT AGGREGATE function, determine the number of people who attended
-- the SQL SYMPHONY CONCERT event 3 times in December 2017
--CODE SYNTAX
SELECT person_id, COUNT(person_id)
FROM sql_symphony_suspects
GROUP BY person_id
ORDER BY COUNT(person_id) desc;
--IMPORTANT NOTES
/* 2 people attended the SQL SYMPHONY CONCERT event 3 times each in December 2017
Their person_id are
99716
24556
These are the PRIME SUSPECTS!
*/

--12. LIMIT the SQL SYMPHONY CONCERT table to show only the IDs of the PRIMARY
SUSPECTS

--CODE SYNTAX
SELECT person_id, COUNT(person_id)

FROM sql_symphony_suspects
GROUP BY person_id
ORDER BY COUNT(person_id) desc
LIMIT 2;

--13. Save output as PRIME SUSPECTS

--CODE SYNTAX
CREATE TABLE prime_suspects AS (
SELECT person_id, COUNT(person_id)
FROM sql_symphony_suspects
GROUP BY person_id
ORDER BY COUNT(person_id) desc
LIMIT 2
);

--14. View PRIME SUSPECTS table
--CODE SYNTAX
SELECT *
FROM prime_suspects;

--14. Using the PERSON IDs of the PRIME SUSPECTS, filter the PERSON table to those 2
suspects
-- (Note: ID in person table is the same as PERSON ID in prime suspect table)
--CODE SYNTAX
SELECT *
FROM person;

SELECT *
FROM person
WHERE id = 99716
OR id = 24556;

--15. Save output as PRIME SUSPECTS NAMED
--CODE SYNTAX

CREATE TABLE prime_suspects_named AS (
SELECT *
FROM person
WHERE id = 99716
OR id = 24556
);

--16. View PRIME SUSPECTS NAMED table
--CODE SYNTAX
SELECT *
FROM prime_suspects_named;

--17. Track the MURDERER from the PRIME SUSPECTS NAME table
-- by comparing the LICENSE ID in the PRIME SUSPECTS NAME table
-- with the LICENSE ID in the FEMALE SUSPECTS table
-- The matching license id is the KILLER!

--CODE SYNTAX

SELECT *
FROM prime_suspects_named;

SELECT *
FROM female_suspects;

--18. Matching LICENSE ID
/*
The matching license ID is 202298 and this belongs to MIRANDA PRIESTLY
Therefore,
MIRANDA PRIESTLY is the KILLER!
*/

SELECT *
FROM person
WHERE id = 99716
SELECT *
FROM drivers_license
WHERE id = 202298;
--19. Locate and save the KILLER&#39;S full details from the PERSON table using the LICENSE ID
--CODE SYNTAX
CREATE TABLE killer AS (
SELECT *
FROM drivers_license
WHERE id = 202298
);

--20. View the KILLERS profile
--CODE SYNTAX

SELECT *
FROM killer;

--21. Send the SQL City Police Department to arrest MIRANDA PRIESTLY!
/*
Her Details:
Name: MIRANDA PRIESTLEY
Gender: Female
Age: 68 years
Social Security Number: 987756388
House Address: 1883, Golden Ave
Height: 66 inches
Hair Color: Red
Eye Color: Green
Car Make: Tesla
Car Model: Model S
Plate Number: 500123
License ID: 202298
*/
