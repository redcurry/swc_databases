Tutorial based on Ben Morris tutorial.
https://github.com/bendmorris/swc_databases

Discuss why working on databases/SQL might be good
- Pull out specific information from database
- Can handle very large data sets
- Optimized to perform very fast
- Used in many places (e.g., scientific data sets like species data)

Open the provided database in Firefox plug-in.
- Visualize database
- Load and create database

On the left hand side, click on "main" (under "Tables"),
then click on "Browse & Search" to view all of the data.

Click on "Execute SQL" to start entering commands.
Click on the little arrow next to "main" to view its columns.

Put editor above SQL window and increase its size.

Enter the command (not case sensitive)

    select * from main

This selects all of the columns from the table main.
To select a specific column, such as species, enter

    select species from main

To select two or more columns, separate them by commas:

    select species, yr from main

Notice how there are repeats.
To select distinct values in the chosen columns:

    select distinct yr from main

Sort it by year in descending order:

    select distinct yr from main order by yr desc

Sort by year, then by month in ascending order:

    select distinct yr, mo from main order by yr desc, mo asc

To condition upon something:

    select distinct yr from main where (yr < 1995)

Perhaps more useful (select a different column):

    select distinct species from main where (yr < 1995)
    select distinct species from main where (yr < 1995) and (mo = 1)

You can combine conditions with and, or, and use of parenthesis.

(Write exercises and answers on EitherPad)

Excercise: Get the names of all distinct species found in June, July, and
August of 1990 and later. Sort them alphabetically by species name, then by
year (most recent first) and month (most recent first).

Answer: select distinct species from main
where (yr >= 1990) and ((mo = '6') or (mo = '7') or (mo = '8'))
order by species asc, yr desc, mo desc

Basic calculations/operations:

    select 2 * stake from main
    select distinct 2 * stake from main order by stake asc

Concatenate strings:

    select genus, species from mlh_species
    select genus || " " || species from mlh_species

Can do other mathematical operations. Point to website.
Aggregate functions (min, max, avg, count, sum)
* Go at least through count

    select max(stake) from main
    select avg(stake) from main
    select count(stake) from main
    select min(yr), max(yr) from main

Sometimes you want to calculate a certain statistic on groups.
Aggregate function can be combined with "group by":

    select avg(stake) from main group by yr
    select avg(stake) from main group by yr, mo

Exercise: For each year after 2000, find the number of individuals
and the average weight for each species trapped in that year.
Order by year, then by the number of individuals trapped.

Answer: select yr, count(species), avg(wgt) from main where (yr > 2000)
group by yr order by yr, count(species)

(Explain general concept of relational database)

For example, table main doesn't have the full species name, only abbreviation.
Table species has abbreviation and scientific name.
Let's say we want to show species name and year it was trapped.
We can reference one table's information and display it together with another.

    select scientificname,yr from main join species
    on main.species = species.new_code

Combine with previous things we've learned:

    select distinct scientificname,yr from main join species
    on main.species = species.new_code
    where (main.yr > 2000)

Exercise: For each rodent species in the portal database,
find their mass from the Mammal Life History dataset (mlh_species)
and also from the Masses of Mammals dataset (mom_species).
Display species name (Genus species) and both masses side by side as 
well as the absolute difference between them.

Answer:

select (mlh_species.genus || " " || mlh_species.species),mlh_species.mass_g,mom_species.comb_mass_g,abs(mlh_species.mass_g - mom_species.comb_mass_g)
    from  mlh_species join mom_species
    on (mlh_species.genus = mom_species.genus) 
    and (mlh_species.species = mom_species.species)
    where (mlh_species.sporder = "Rodentia")

Missing values (aka null values):

    select * from main

Notice the null values, don't display them:

    select * from main where age is not null

Exercise: Add appropriate lines so that mass_g and comb_mass_g are not null

    and (mlh_species.mass_g is not null)
    and (mom_species.comb_mass_g is not null)
