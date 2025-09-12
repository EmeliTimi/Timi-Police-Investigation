# 🕵️ Murder Mystery Investigation – SQL Case Study  

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

### 2. Crime Scene Analysis
- Extracted witness details from the **crime scene report**.
- Identified two key witnesses: **Morty Schapiro** and **Annabel Miller**.

### 3. Witness Identification & Statements
- Retrieved witness details from the **Persons** and **Interview** tables.
- Analyzed statements for leads on suspects.

### 4. Suspect Identification
- Queried **gym membership records** (IDs starting with `48Z`, Gold status).
- Narrowed suspects to **Joe Germuska** and **Jeremy Bowers**.
- Cross-verified with **gym attendance logs (Jan 9, 2018)**.

### 5. Vehicle Evidence
- Morty reported a vehicle plate containing **H42W**.
- Cross-referenced **DMV data** → matched with **Jeremy Bowers**.

### 6. Prime Suspect Pinpointed
- **Jeremy Bowers** confirmed as the murderer (**gym + vehicle + witness evidence**).

### 7. Mastermind Investigation
- Jeremy’s confession pointed to a **mastermind**.
- Queried **event attendance logs** (SQL Symphony Concerts, attended 3x in December).
- Identified **Miranda Priestly** as the mastermind.

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

