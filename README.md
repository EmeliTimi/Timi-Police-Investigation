# 🕵️ Murder Mystery Investigation – SQL Case Study  
<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/82cda764-c5ad-46e2-8934-57a5084414ad" />


## Objective  
To solve a **murder case in SQL City (January 15, 2018)** using **SQL queries** on a relational police database, mirroring real-world detective workflows.  


## Problem Statement  
A murder occurred on **January 15, 2018**, in SQL City.  

The goals were to:  
- Identify the **witnesses**.  
- Use their **statements** to trace the **murder suspect**.  
- Uncover the **mastermind** behind the crime.  


## Investigative Process Followed

### 1. Initial Case Review
- Established crime parameters: **date, location, type of crime**.
```sql
/*
A crime has taken place and the detective needs your help. 
The detective gave you the crime scene report, but you somehow lost it. 
You vaguely remember that the crime was a murder that occurred sometime on Jan 15, 2018
and that it took place in SQL City. 
Start By retrieving the corresponding crime scene report from the police department’s database.
*/

SELECT *
FROM crime_scene_report
WHERE crime_type = 'murder'
AND date = 20180115
AND city = 'SQL City';
 ```

### 2. Crime Scene Analysis
- Extracted witness details from the **crime scene report**.
- Identified two key witnesses: **Morty Schapiro** and **Annabel Miller**.

### 3. Witness Identification & Statements
- Retrieved witness details from the **Persons** and **Interview** tables.
- Analyzed statements for leads on suspects.
  
```sql
-- Let's find the first witness from the person table
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC;

/*
First Witness Details:
The first witness' name is Morty Sschapiro with ID: 14887,  with license id: 118009
SSN: 111564949	
*/

-- Let's find the second witness based on details provided
SELECT * 
FROM person
WHERE name like '%Annabel%' 
AND address_street_name = 'Franklin Ave';

/*
Second Witness Details
The second witness name is Annabel Miller with ID: 16371, License ID: 490173
and SSN: 318771143
*/


--Get the witnesses Statements from the Interview Table
SELECT * 
FROM interview
WHERE person_id IN (14887,16371);
```

### 4. Suspect Identification
- Queried **gym membership records** (IDs starting with `48Z`, Gold status).
- Narrowed suspects to **Joe Germuska** and **Jeremy Bowers**.
- Cross-verified with **gym attendance logs (Jan 9, 2018)**.
```sql
-- Let's deep dig using the first witness interview statement
SELECT *
FROM get_fit_now_member 
WHERE ID LIKE '48Z%'
AND membership_status = 'gold';

/*
Based on the query with id and gold membership, I've been able to filter two results, which are

1. id: 48Z7A person_id: 28819, name: Joe Germuska, membership_start_date: 20160305 and membership_status: gold

2. id: 48Z55, person_id: 67318, name: Jeremy Bowers, membership_start_date: 20160101, and membership_status: gold

I still need to dig further using  sex: male, license_plate number that contains "H42W"
*/

SELECT * FROM drivers_license
WHERE gender = 'male'
AND plate_number LIKE '%H42W%';

/*
Based on the query above, I have these information: id: 423327,	age: 30	height: 70,	eye_color: "brown"	
hair_color: "brown", gender: "male",	plate_number: "0H42W2", 
car_make: "Chevrolet", car_model: "Spark LS" for the first person

Then for the second, I have: ID: 664760,	age: 21,	Height: 71	
eye_color: "black", hair_color: "black", gender: "male", plate_number: "4H42WR"
car_make: "Nissan", car_model: "Altima"

I need to check the second witness statement to finetune my result for the potential suspect

The second witness with ID: 16371 said "I saw the murder happen, and I recognized the killer from my gym 
when I was working out last week on January the 9th."

*/

-- Let's check the get_fit_now_check_in based on what the second witness said
SELECT membership_id, check_in_date
FROM get_fit_now_check_in
WHERE check_in_date = 20180109

-- To find the suspect, based on gym membership and checkin date, we use inner join
SELECT 
    gm.*,
    gc.check_in_date
FROM get_fit_now_member AS gm
JOIN get_fit_now_check_in AS gc
    ON gc.membership_id = gm.id
WHERE gm.id LIKE '48Z%'
  AND gm.membership_status = 'gold';

/* We now have two suspects and we can now filter based on License plate number.
But first, let's save the result as a new table
*/

-- Let's save our result as a new table

CREATE TABLE suspects AS
SELECT 
    gm.*,
    gc.check_in_date
FROM get_fit_now_member AS gm
JOIN get_fit_now_check_in AS gc
    ON gc.membership_id = gm.id
WHERE gm.id LIKE '48Z%'
  AND gm.membership_status = 'gold';
```

### 5. Vehicle Evidence
- Morty reported a vehicle plate containing **H42W**.
- Cross-referenced **DMV data** → matched with **Jeremy Bowers**.
 ```sql
-- Verify license_id 

SELECT * FROM person
WHERE license_id IN (423327,664760)

/*
Based on license plate, we've found additional information. 
We have two persons with id: 51739 and id: 67318 who are males and have that
pattern of plate number. Based on this, let's finally check which of these individuals
are the murderer from our suspects table.
*/
``` 

### 6. Prime Suspect Pinpointed
- **Jeremy Bowers** confirmed as the murderer (**gym + vehicle + witness evidence**).
```sql
-- Nail the Murderer
SELECT * FROM suspects
WHERE person_id IN (51739, 67318);

/*
We found our suspect, his name is Jeremy Bowers
*/
``` 
### 7. Mastermind Investigation
- Jeremy’s confession pointed to a **mastermind**.
- Queried **event attendance logs** (SQL Symphony Concerts, attended 3x in December).
- Identified **Miranda Priestly** as the mastermind.
```sql
-- Let's review the murderer's statement
SELECT * FROM interview
WHERE person_id = 67318;

/*
"I was hired by a woman with a lot of money. 
I don't know her name but I know she's around 5'5"" (65"") or 5'7"" (67""). 
She has red hair and she drives a Tesla Model S. 
I know that she attended the SQL Symphony Concert 3 times in December 2017.
"
Clues:
The real villain is a female, approximately 5'5" (65") or 5'7" (67") tall.
She has red hair and drives a Tesla Model S.
She attended the SQL Symphony Concert 3 times in December 2017.
*/
-- Let's dive deep to find the real villain
SELECT * FROM drivers_license
WHERE gender = 'female'
AND height BETWEEN 65 AND 67
AND hair_color = 'red'
AND car_make = 'Tesla'
AND car_model = 'Model S';

/*
We have three suspected villain already that fit the suspect's descriptions. 
Let's get their personal details
*/

-- Get personal details of the potential female Villains

SELECT * FROM person
WHERE license_id IN (202298, 291182,918773)

/*
The women's IDs are Red Korb, ID: 78881, Regina George, ID: 90700
Miranda Priestly, ID: 99716
*/

-- Now let's check which of them attend the SQL symphony 3x in december 2017

SELECT person_id,
count(*) as number_of_events_attended,
event_name
FROM facebook_event_checkin
WHERE person_id IN (78881, 90700, 99716)
GROUP BY person_id, event_name;


-- Now, let's find the master name of the mastermind 
SELECT p.*,
count(*) as number_of_events_attended,
fb.event_name
FROM person AS p
JOIN facebook_event_checkin AS fb
ON
p.id = fb.person_id
WHERE p.license_id IN (202298, 291182,918773)
AND fb.person_id IN (78881, 90700, 99716)
GROUP BY person_id, event_name, p.id;

/*
Based on the query above, the mastermind of this murder is Miranda Priestly with ID: 99716
*/
``` 


## Key Findings
1. Two key witnesses were identified: **Morty Schapiro** and **Annabel Miller**.  
2. Using witness statements, gym membership records, and DMV data, **Jeremy Bowers** was confirmed as the murderer.  
3. Jeremy’s confession revealed the involvement of a **mastermind**.  
4. Event attendance logs confirmed **Miranda Priestly** as the mastermind behind the crime.  
5. SQL queries enabled a structured, step-by-step approach to solving the case efficiently.  


## Recommendations
- **Database Integration**: Link police, DMV, gym, and event records to enable faster cross-referencing in investigations.  
- **Witness Management System**: Digitally store and connect witness statements with case records for quick retrieval.  
- **Event Surveillance**: Strengthen digital check-in and ID verification for high-profile events.  
- **SQL Query Templates**: Develop a library of investigative SQL templates to speed up future case analysis.  
- **Further Investigation**: Probe **Miranda Priestly’s** network for possible accomplices and financial backers.  

