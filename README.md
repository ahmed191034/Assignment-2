# Assignment-2
SQL script queries pet data, like owner-pet relations, procedures, and categorizations. Involves JOINs, aggregations, CASE, and window functions for comprehensive pet analytics.
/*1-List the names of all pet owners along with the names of their pets.*/
select * from petowners;
select * from pets;

select PO.OWNERID AS Owner_Id, concat(po.Name," ",po.surname) as Owner_Name,p.Name as Pet_Name
from petowners po
inner join pets p on po.OwnerID=p.OwnerID ORDER BY OWNER_ID ASC ;


/*2-List all pets and their owner names, including pets that don't have recorded
owners.*/
select * from petowners;
select * from pets;


select concat(po.Name," ",po.surname) as Owner_Name,p.Name as Pet_Name
from pets p
left join petowners po on po.OwnerID=p.OwnerID;


/*3-Combine the information of pets and their owners, including those pets
without owners and owners without pets.*/
select * from petowners;
select * from pets;


SELECT COALESCE(P.name, 'No Pet') AS pet_name, O.name
FROM pets P
left JOIN petowners O ON P.ownerid = O.ownerid;


/*4-Find the names of pets along with their owners' names and the details of the
procedures they have undergone.*/
select * from procedureshistory;
select * from petowners;
select * from pets;
select distinct proceduretype from procedureshistory;
select concat(po.name," ",po.surname) as Owners_Name,p.Name as Pet_name,
count(distinct case when Ph.procedureType="Vaccination" then procedureType end) as Vaccination,
count(distinct case when Ph.procedureType="Hospitalization" then procedureType end) as Hospitalization,
count(distinct case when Ph.procedureType="General Surgeries" then procedureType end) as General_Surgeries,
count(distinct case when Ph.procedureType="Grooming" then procedureType end) as Grooming,
count(distinct case when Ph.procedureType="Orthopedic" then proceduretype end) as Orthopedic
from procedureshistory ph
inner join  pets p on p.PetID=ph.PetID
inner join petowners po on po.ownerid=p.ownerid
group by Owners_Name,p.Name,ph.ProcedureType;


select concat(po.name," ",po.surname) as Owners_Name,p.Name, count(ph.ProcedureType)
from procedureshistory ph
inner join  pets p on p.PetID=ph.PetID
inner join petowners po on po.ownerid=p.ownerid
group by Owners_Name,p.Name,ph.ProcedureType;


select ph.ProcedureType as Procedure_Types ,count(distinctrow ph.PetID) as Counts_of_Procedure,
p.NAME AS Pet_Name from pets p
left join procedureshistory ph on p.PetID=ph.PetID
group by ph.ProcedureType,Pet_Name
having ph.ProcedureType is Null;

# 5-List all pet owners and the number of dogs they own.
select * from petowners;
select * from pets;


select concat(po.Name," ",po.surname) as Owner_Name, count(p.kind) as Number_Of_Dogs
from petowners po
inner join pets p on p.OwnerID=po.OwnerID
group by Owner_Name,p.kind
having p.kind="Dog" order by Number_of_Dogs asc;

# 6-Identify pets that have not had any procedures.
select * from procedureshistory;
select * from pets;


select ph.ProcedureType as Procedure_Types ,p.NAME AS Pet_Name from pets p
left join procedureshistory ph on p.PetID=ph.PetID
group by ph.ProcedureType,Pet_Name
having ph.ProcedureType is Null;

# 7-Find the name of the oldest pet.
SELECT Name, kind, Gender, MAX(age) AS OldestAge
FROM pets
GROUP BY Name, kind, Gender
order by OldestAge desc limit 3;

/*8-List all pets who had procedures that cost more than the average cost of all
procedures.*/
select * from pets;
select * from procedureshistory;
select * from proceduresdetails;

select  p.Name as Pet_Name,p.kind,pd.price as Average_Cost_On_Procedure from pets p
inner join procedureshistory ph on p.petid=ph.petid
inner join proceduresdetails pd on ph.ProcedureSubcode=pd.ProcedureSubcode
where pd.Price>(select avg(price) from proceduresdetails);


#9-Find the details of procedures performed on 'Cuddles'.
select * from pets;
select * from procedureshistory;
select p.name,P.kind,P.Age,p.Gender,ph.ProcedureType from pets p
inner join procedureshistory ph on p.petid=ph.petid
where Name="Cuddles";

/*10-Create a list of pet owners along with the total cost they have spent on
procedures and display only those who have spent above the average
spending.*/

select * from petowners;
select * from pets;
select * from procedureshistory;
select * from proceduresdetails;

select po.ownerid as Owner_ID, 
concat(po.name," ",po.surname) AS Owner_name,
sum(pd.price) as Total_Cost_Spend_On_Procedures
from petowners po
join pets p on po.ownerid=p.ownerid
join  procedureshistory ph on p.petid=ph.petid
join proceduresdetails pd on ph.proceduresubcode=pd.proceduresubcode
group by po.OwnerID,Owner_name
having sum(pd.price)>(Select avg(price) from proceduresdetails)
order by Total_Cost_Spend_On_Procedures asc;

# 11-List the pets who have undergone a procedure called 'VACCINATIONS'.

select * from pets;
select * from procedureshistory;

select distinct p.Name,p.kind,ph.Proceduretype
from pets p
inner join procedureshistory ph on p.PetID=ph.PetID
where ProcedureType="Vaccinations";

# 12-Find the owners of pets who have had a procedure called 'EMERGENCY'.
select * from petowners ;
select * from pets;
select * from procedureshistory;

select distinct po.Name,ph.Proceduretype
from petowners po
inner join pets p on p.ownerid=po.ownerid
inner join procedureshistory ph on p.PetID=ph.PetID
where ProcedureType="EMERGENCY";

# 13-Calculate the total cost spent by each pet owner on procedures.

select * from petowners ;
select * from pets;
select * from proceduresdetails;
select * from procedureshistory;

select concat(po.Name," ",po.Surname) as Owner_Name, Sum(pd.Price) as Total_Cost
from  petowners po
inner join pets p on  p.ownerid=po.ownerid
inner join  procedureshistory ph on p.petid=ph.petid
inner join proceduresdetails pd  on pd.proceduresubcode=ph.proceduresubcode
group by owner_Name order by Owner_Name  ASC;

# 14-Count the number of pets of each kind.
SELECT * FROM PETS;

Select kind as Type_of_Pets,count(Kind) as Number_of_Pets from pets
group by kind;

/*
15-Group pets by their kind and gender and count the number of pets in each
group.
*/
SELECT * FROM PETS;
Select kind as Type_of_Pets,Gender,count(Gender) as Number_of_Pets
from pets
group by kind,Gender order by Kind asc;

/*16.Show the average age of pets for each kind, but only for kinds that have more
than 5 pets.*/
select * from pets;
SELECT Name, kind AS Type_of_Pets, ROUND(AVG(Age)) AS Age
FROM pets
GROUP BY NAME, kind
HAVING avg(age) > 5 order by Name asc;

# 17-Find the types of procedures that have an average cost greater than $50.
select * from proceduresdetails;
select ProcedureType as Procedure_Types,round(avg(Price),2) as Price
from proceduresdetails
group by ProcedureType
having avg(price)>50;
/*
18-Classify pets as 'Young', 'Adult', or 'Senior' based on their age. Age less then
3 Young, Age between 3and 8 Adult, else Senior.
*/

select * from pets;

select Name,Kind,Gender,Age,
case 
	when Age<=3 then "Young"
    when age between 3 and 8 then "Adult"
    when age >8 then "Senior"
end as Age_Classification
from pets
order by age asc;   

/*19-Calculate the total spending of each pet owner on procedures, labeling them
as 'Low Spender' for spending under $100, 'Moderate Spender' for spending
between $100 and $500, and 'High Spender' for spending over $500.*/

select * from petowners;
select * from pets;
select * from procedureshistory;
select * from proceduresdetails;

select Owner_name,Total_Cost,
case 
	when Total_Cost<100 then "Low Spender"
    when Total_Cost between 100 and 500 then "Moderate Spender"
    when Total_Cost>500 then "High Spender"
    end as Price_Labelling
from  (
select concat(po.Name," ",po.Surname) as Owner_Name, Sum(pd.Price) as Total_Cost
from petowners po
inner join pets p on  p.ownerid=po.ownerid
inner join  procedureshistory ph on p.petid=ph.petid
inner join proceduresdetails pd  on pd.proceduresubcode=ph.proceduresubcode
group by owner_Name
) as CostSummary;


# 20-Show the gender of pets with a custom label ('Boy' for male, 'Girl' for female).

select * from pets;

select Name,Kind,Gender,
case
	when Gender="Male" then "Boy"
    when Gender="Female" then "Girl"
end as Gender_Label    
from pets;

/*
21-For each pet, display the pet's name, the number of procedures they've had,
and a status label: 'Regular' for pets with 1 to 3 procedures, 'Frequent' for 4 to
7 procedures, and 'Super User' for more than 7 procedures.
*/

select * from pets;
select * from proceduresdetails;
select * from procedureshistory;


select pet_name,Number_Of_Procedure,
case
	when Number_Of_Procedure between 1 and 3 then "Regular"
    when Number_Of_Procedure between 4 and 7 then "Frequent"
    when Number_Of_Procedure >7 then "Super User"
end as Procedure_Category
from(
select p.Name as Pet_Name,p.Kind,P.Gender,P.age,count(ph.procedureType) as Number_Of_Procedure
from  pets p
inner join  procedureshistory ph on p.petid=ph.petid
inner join proceduresdetails pd  on pd.proceduresubcode=ph.proceduresubcode
group by  Pet_Name, p.Kind, P.Gender, P.age
) as Procedure_Summray;

# 22-Rank pets by age within each kind.

select * from pets;

SELECT Name,Kind,Age,
RANK() OVER(PARTITION BY Kind ORDER BY Age) AS Age_Rank
FROM pets;

# 23-Assign a dense rank to pets based on their age, regardless of kind.
select * from pets;

SELECT Name,Kind,Age,
dense_RANK() OVER(ORDER BY Age) AS Age_Rank
FROM pets;

# 24-For each pet, show the name of the next and previous pet in alphabetical order.
select * from pets;


SELECT Name AS Current_Pet,
LAG(Name) OVER (ORDER BY Name) AS Previous_Pet,
LEAD(Name) OVER (ORDER BY Name) AS Next_Pet
FROM pets
ORDER BY Name asc;

#25.Show the average age of pets, partitioned by their kind.
select * from pets;


select kind,round(Avg(age),1)
from pets
group by kind;


# 26-Create a CTE that lists all pets, then select pets older than 5 years from the CTE.
select * from pets;

WITH AllPets AS (SELECT Name,Kind,Gender,Age FROM pets)
SELECT Name,Kind,Gender,Age
FROM allPets
WHERE Age > 5;
