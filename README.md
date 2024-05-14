# SQL-PRECTICE

Show first name, last name, and gender of patients whose gender is 'M'

select first_name ,last_name,gender from patients
where gender like 'M';


Show first name and last name of patients who does not have allergies. (null)
select first_name ,last_name from patients
where allergies is null;


Show first name of patients that start with the letter 'C'
select first_name from  patients
where first_name like'c%';


Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)
select first_name ,last_name from  patients
where weight between 100 and 120;

Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

update patients
set allergies ='NKA'
where allergies is null;

Show first name and last name concatinated into one column to show their full name.

select concat(first_name," ",last_name) from patients;


Show first name, last name, and the full province name of each patient.

Example: 'Ontario' instead of 'ON'
SELECT 
   patients. first_name,
     patients.last_name,
    province_names.province_name
FROM 
    patients 
JOIN 
    province_names ON province_names.province_id = patients.province_id;



    Show how many patients have a birth_date with 2010 as the birth year.
    SELECT COUNT(*) AS num_patients_2010_birth_year
FROM patients
WHERE YEAR(birth_date) = 2010;


Show the first_name, last_name, and height of the patient with the greatest height.
select first_name ,last_name,height  from patients

order by  height desc
limit 1;
Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000

select * from patients 
where patient_id in  (1,45,534,879,1000);

Show the total number of admissions

select count(admission_date) as total_admissions from admissions;

Show all the columns from admissions where the patient was admitted and discharged on the same day.

select * from admissions
where admission_date = discharge_date;

Show the patient id and the total number of admissions for patient_id 579.

select patient_id ,count(admission_date) from admissions
where patient_id  = 579;

Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?
select distinct(city) from patients
where province_id ='NS';


Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70
select first_name ,last_name ,birth_date from patients
where height>160 and weight>70;

Write a query to find list of patients first_name, last_name, and allergies where allergies are not null and are from the city of 'Hamilton'

select first_name,last_name ,allergies from patients
where allergies is not null and city ='Hamilton';



Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.
select first_name ,last_name, birth_date from patients
where year(birth_date) between 1970 and 1979

order by birth_date asc;

Show unique first names from the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(*) = 1;

Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

select patient_id ,first_name from patients
where  first_name like 's%____s';


Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.

Primary diagnosis is stored in the admissions table.

SELECT patients.patient_id, patients.first_name, patients.last_name 
FROM patients
JOIN admissions ON patients.patient_id = admissions.patient_id
WHERE admissions.diagnosis = 'Dementia';


Display every patient's first_name.
Order the list by the length of each name and then by alphabetically.

select first_name from patients
order by len(first_name),first_name;

Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.

SELECT COUNT(CASE WHEN gender = 'M' THEN 1 END) AS male_count,
       COUNT(CASE WHEN gender = 'F' THEN 1 END) AS female_count
FROM patients;

Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
SELECT 
    first_name, 
    last_name, 
    allergies 
FROM 
    patients
WHERE 
    allergies IN ('Penicillin', 'Morphine')
ORDER BY 
    allergies ASC, 
    first_name ASC, 
    last_name ASC;

    Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
    select patient_id ,diagnosis from admissions
group by  patient_id,diagnosis
having   count(*)>1;

Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.

select  city, count(distinct patient_id)  as num_patients from patients
group by city

order by num_patients desc,city asc;


Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"

SELECT first_name, last_name, 'Patient' AS role
FROM patients
UNION all
SELECT first_name, last_name, 'Doctor' AS role
FROM doctors;



Show all allergies ordered by popularity. Remove NULL values from query.

SELECT allergies, COUNT(*) AS total_diagnosis
FROM patients
WHERE allergies IS NOT NULL
GROUP BY allergies
ORDER BY total_diagnosis DESC;

Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

select first_name ,last_name, birth_date from patients
where year(birth_date) between 1970 and 1979

order by birth_date asc;


We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane

SELECT CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS new_name_format
FROM patients
ORDER BY first_name DESC;

Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

select province_id , sum(height) as sum_height from patients
group by province_id
having sum(height)>=7000
order by  sum_height asc;


Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
select max(weight)-min(weight)  as weight_delta from patients
where last_name is 'Maroni';

Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.

SELECT DAY(admission_date) AS day_number, COUNT(*) AS num_of_admissions 
FROM admissions
GROUP BY DAY(admission_date)
ORDER BY num_of_admissions DESC, day_number desc;

Show all columns for patient_id 542's most recent admission_date.

SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1;


Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

 SELECT patient_id, attending_doctor_id, diagnosis  
FROM admissions 
WHERE 
    (patient_id % 2 != 0 AND attending_doctor_id IN (1, 5, 19))
    OR 
    (attending_doctor_id LIKE '%2%' AND LENGTH(patient_id) = 3)
ORDER BY attending_doctor_id;


Show first_name, last_name, and the total number of admissions attended for each doctor.

Every admission has been attended by a doctor.

SELECT doctors.first_name, doctors.last_name, COUNT(admissions.admission_date) AS total_admissions
FROM doctors
JOIN admissions ON doctors.doctor_id = admissions.attending_doctor_id
GROUP BY doctors.first_name, doctors.last_name;


For each doctor, display their id, full name, and the first and last admission date they attended.


SELECT 
    doctors.doctor_id, 
    CONCAT(doctors.first_name, ' ', doctors.last_name) AS full_name,
    MIN(admissions.admission_date) AS first_admission,
    MAX(admissions.admission_date) AS last_admission
FROM 
    doctors
JOIN 
    admissions ON doctors.doctor_id = admissions.attending_doctor_id
GROUP BY 
    doctors.doctor_id, 
    full_name;

    Display the total amount of patients for each province. Order by descending.

    SELECT 
    province_names.province_name,
    COUNT(*) AS patients_counts
FROM 
    patients
JOIN 
    province_names ON patients.province_id = province_names.province_id
GROUP BY 
    province_names.province_name
ORDER BY 
    patients_counts DESC;

    For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

    SELECT CONCAT(patients.first_name, ' ', patients.last_name) AS patient_name,
       admissions.diagnosis,
       CONCAT(doctors.first_name, ' ', doctors.last_name) AS doctor_name
FROM admissions
JOIN patients ON patients.patient_id = admissions.patient_id
JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id;

display the first name, last name and number of duplicate patients based on their first name and last name.

Ex: A patient with an identical name can be considered a duplicate

select  first_name ,last_name,count(*) as  num_of_duplicate from patients
group by first_name,last_name
having count(*)>1;

Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.

Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.

SELECT 
    CONCAT(first_name, ' ', last_name) AS 'Full Name',
    ROUND(height / 30.48, 1) AS 'Height',
    ROUND(weight * 2.205, 0) AS 'Weight',
    birth_date,

    CASE
        WHEN gender = 'M' THEN 'male'
        WHEN gender = 'F' THEN 'female'
    END
FROM 
    patients;








    Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)

    SELECT patient_id, first_name, last_name 
FROM patients
WHERE patient_id NOT IN (SELECT patient_id FROM admissions);



Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.

For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.
select floor(patients.weight/10)*10 as weight_group, count(*) as patients_in_group
from patients
group by floor(patients.weight/10)
order by floor(patients.weight/10) desc;


Show patient_id, weight, height, isObese from the patients table.

Display isObese as a boolean 0 or 1.

Obese is defined as weight(kg)/(height(m)2) >= 30.

weight is in units kg.

height is in units cm.

SELECT 
    patient_id,
    weight,
    height,
    CASE WHEN (weight / POWER(height / 100.0, 2)) >= 30 THEN 1 ELSE 0 END AS isObese
FROM 
    patients;

    
Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'

Check patients, admissions, and doctors tables for required information.

SELECT patients.patient_id, patients.first_name, patients.last_name, doctors.specialty
FROM patients

JOIN admissions ON patients.patient_id = admissions.patient_id
JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id

WHERE diagnosis = 'Epilepsy' AND doctors.first_name LIKE '%Lisa%';

All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

The password must be the following, in order:
1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date

SELECT
  patients.patient_id,
  CONCAT(
    patients.patient_id,
    LEN(patients.last_name),
    YEAR(birth_date)
  ) AS temp_password
FROM patients 
  join admissions on patients.patient_id =admissions.patient_id
GROUP BY patients.patient_id;




Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group

SELECT
  'Yes' AS has_insurance,
  COUNT (*) * 10 cost_after_insurance
FROM admissions
WHERE patient_id % 2 = 0
UNION
SELECT
  'No' AS has_insurance,
  COUNT (*) * 50 cost_after_insurance
FROM admissions
WHERE patient_id % 2 != 0


Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name
SELECT 
    province_names.province_name
FROM 
    patients
JOIN 
    province_names 
ON 
    patients.province_id = province_names.province_id
GROUP BY 
    province_names.province_name
HAVING 
    COUNT(CASE WHEN patients.gender = 'M' THEN 1 END) > COUNT(CASE WHEN patients.gender = 'F' THEN 1 END);



    We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:
- First_name contains an 'r' after the first two letters.
- Identifies their gender as 'F'
- Born in February, May, or December
- Their weight would be between 60kg and 80kg
- Their patient_id is an odd number
- They are from the city 'Kingston

SELECT *
FROM patients
WHERE first_name LIKE '__r%'
  AND gender = 'F'
  AND MONTH(birth_date) IN (2, 5, 12)
  AND weight BETWEEN 60 AND 80
  AND MOD(patient_id, 2) != 0

  Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.
SELECT
concat( round(100 * avg(gender = 'M'), 2), '%') 
AS percent_of_male_patients
FROM patients;


For each day display the total amount of admissions on that day. Display the amount changed from the previous date.
SELECT 
    admission_date, 
    COUNT(admission_date) AS admission_count,
    COUNT(admission_date) - LAG(COUNT(admission_date), 1) OVER (ORDER BY admission_date) AS change_from_previous_day
FROM 
    admissions 
GROUP BY 
    admission_date 
ORDER BY 
    admission_date ASC;


    Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.

    select province_name from province_names  
order by
(case when province_name='Ontario' then 0 else 1 end),
province_name


We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.

SELECT
  d.doctor_id as doctor_id,
  CONCAT(d.first_name,' ', d.last_name) as doctor_name,
  d.specialty,
  YEAR(a.admission_date) as selected_year,
  COUNT(*) as total_admissions
FROM doctors as d
  LEFT JOIN admissions as a ON d.doctor_id = a.attending_doctor_id
GROUP BY
  doctor_name,
  selected_year
ORDER BY doctor_id, selected_year
  AND city = 'Kingston';

  
