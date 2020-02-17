# Introduction to Spatial Databases

This page provides resources and some guidelines on how to complete a GIS311 practical  (on spatial databases. Note that this page provides some information, not step-by-step instructions. You have to adapt information available online for your purposes to complete the task.

## Section 1 
### Creating a spatial database in pgAdmin 4 
[pgAdmin](https://www.pgadmin.org) is the most popular and feature rich Open Source administration and development platform for PostgreSQL, the most advanced Open Source database in the world. You use pgAdmin to manage and interact with the data in the database. 

To open pgAdmin, go to the `Start` --> `Applications` --> `PostgreSQL 10.x`, and select pgAdmin4. The pgAdmin interface will now open (you should see an elephant on a splash screen). The image below shows what the interface looks like.

![pgAdmin Loading screen](https://www.postgresql-archive.org/attachment/6003956/0/Screenshot%20from%202018-02-01%2008-46-00.png)


To create a new database, use the menu at the top, `Object` --> `Create` --> `Database`. Add a database name that is appropriate and under the **Definition** tab, select the postgis template (see below). The general rule of thumb is to ***NOT*** use capital letters, special characters and spaces. This creates complications, so try to avoid this.

[[File:CreateNewDatabase.png|center|500px|pgAdmin4 interface]]


There is two ways to check if you successfully created a spatial database (i.e. meaning that you will be able to load geometries into the database):
1. Expand the **Extensions** section, you should see the **postgis** and **postgis_topology** extensions.
2. Under **schemas**, you should have a **topology** schema and in the public schema there will be table called **spatial_ref_sys**.

[[File:SpatialDatabases.png|center|300px|pgAdmin4 interface]]


If you missed the step to create a spatial database, click on Extensions and add the **postgis** and **postgis_topology** extensions.

### What is a CSV?
A comma-separated values (CSV) file is a delimited text file that uses a comma to separate values. A CSV file stores tabular data (numbers and text) in plain text. Each line of the file is a data record. Each record consists of one or more fields, separated by commas. The use of the comma as a field separator is the source of the name for this file format.

This is a very popular format and it is important that you know how to use CSVs. You can read more about CSVs [here](https://www.howtogeek.com/348960/what-is-a-csv-file-and-how-do-i-open-it/).

### Loading a CSV into a new table
[This tutorial](http://www.postgresqltutorial.com/import-csv-file-into-posgresql-table/) provides a good example of how to load a CSV into a database. You should adapt the information in the tutorial to work with your data.

In pgAdmin4, you can create and alter your tables using SQL but also the GUI. To access the GUI, right click on your table name and select **Properties*.

### Basic SQL notation and operations
Structured Query Language (SQL) is a standard language for accessing and manipulating databases. SQL operates through simple, declarative statements. This keeps data accurate and secure, and it helps maintain the integrity of databases, regardless of size.

An SQL statement generally consists of three part:
SELECT   --> What is the results? or What would you like to see?
FROM      --> Which tables are you using?
WHERE   --> Is there any conditions?

I consider [https://www.w3schools.com/sql/sql_intro.asp w3schools] the best resource when writing SQL statements.

Here are some examples:
* List all the record in the table:
:This would be a basic SQL SELECT statement. Here is an example, [https://www.w3schools.com/sql/sql_select.asp https://www.w3schools.com/sql/sql_select.asp].
* Selecting all records that fulfils a specific condition:
: This would require using the WHERE statement and maybe adding operators, such as AND or OR. Here is an example, [https://www.w3schools.com/sql/sql_and_or.asp https://www.w3schools.com/sql/sql_and_or.asp].
* Calculating a sum of records that fulfil a specific condition (i.e. aggregating data):
: This would require using aggregation operator, such as COUNT(), SUM() or AVG(). Here is an example, [https://www.w3schools.com/sql/sql_count_avg_sum.asp https://www.w3schools.com/sql/sql_count_avg_sum.asp].

## Section 2 
=== Connection a PostGIS database to QGIS? ===
In order to load QGIS layers into a PostGIS table or view a PostGIS table in QGIS, the first step is to create a connection between QGIS and PostgreSQL. To create a new connection, go to, ''Layer'' --> ''Add layer'' --> ''Add PostGIS layers''... Next, select ''New''. The following popup will appear.

[[File:Qgis_connection.png|center|250px|pgAdmin4 interface]]

Add the following information:
* Name --> this is the name of the connection.
* Host --> In your case this would be localhost. If the database is on an off site server, you would add the IP of the server here.
* Port --> The default port is 5432, this should be the same unless you changed it during installation.
* Database --> This is the database that you would like to connect to (i.e. the database you created earlier).
* Authentication --> Select the ''Basic'' tab, and add the username and password. In the lab the username and password would be '''postgres'''.

After you have added all the information, you can test the connection, and thereafter complete the process by pressing OK.

### Loading a QGIS layer into a database
The first step is to open the layer in QGIS that you would like to import into your database. I also assume you have created the PostGIS connection describe in the previous section.

Once you are set, in the main menu go to ''Database'' --> DB ''Manager''. In the DB Manager, navigate to the connection and schema. See an example below.

[[File:Db_manager.png|center|400px|pgAdmin4 interface]]

Next click on the ''import'' option. The ''Import vector layer'' popup will appear. Please complete the following:
* Input --> Select the layer you would like to import into your database.
* Schema --> This is the schema you will use. In most cases this will be public.
* Select the following options:
** Geometry column --> leave this as geom
** Source SRID
** Target SRID
** Encoding
** Convert field names to lowercase
** Create spatial index --> this make access to the table in PostgreSQL faster

[[File:Import_vector.png|center|300px|pgAdmin4 interface]]

It is important to add your SRID, else you will not be able to execute spatial queries. A spatial reference identifier (SRID) is a unique identifier associated with a specific coordinate system, tolerance, and resolution. If you don't know what the SRID is for your shapefile, use [https://epsg.io this site] to select an appropriate SRID.

### Joining tables using SQL
A JOIN clause is used to combine rows from two or more tables, based on a related column between them. In our case, we will find a column that is similar in both tables and JOIN based on that column. You can learn more about JOINS here, [https://www.w3schools.com/sql/sql_join.asp https://www.w3schools.com/sql/sql_join.asp].

You will have to use on of the variations of the JOIN when completing the task.

### Basics of spatial SQL
Spatial SQL is an extension of SQL with spatial function. You can identify these function easily as they start with '''ST_'''.

Below is an example of how you would run an intersect in spatial SQL:

SELECT a.*
FROM table1 a, table2 b
WHERE ST_Intersects(a.geom, b.geom)

You can learn more about spatial SQL from the [http://postgis.net/workshops/postgis-intro/index.html Boundless PostGIS workshop]. This tutorial uses the previous version of pgAdmin, but focus on the SQL statement and how they work.

### pgAdmin 4 geometry viewer
When you run a spatial SQL statement in pgAdmin, you often want to view the results. As of late-2018 pgAdmin now has a built-in geometry viewer. You can access the geometry viewer by clicking the eye icon in the geom column.

### Storing query results in a new table
An easy way to store your results in a new table is to use the INTO operation. Learn more here, [https://www.w3schools.com/sql/sql_select_into.asp https://www.w3schools.com/sql/sql_select_into.asp].

## Other resources 
''SQL resources:''
* SQL Quick Reference From W3Schools, https://www.w3schools.com/sql/sql_quickref.asp
* SQL Quick Guide - TutorialPoint, https://www.tutorialspoint.com/sql/sql-quick-guide.htm
* Intro to SQL: Querying and managing data, https://www.khanacademy.org/computing/computer-programming/sql

''PostGIS resources:''
* Getting Started With PostGIS: An almost Idiot's Guide - Boston GIS, http://www.bostongis.com/?content_name=postgis_tut01
* PostGIS documents, https://postgis.net/docs/manual-2.5/
* PostGIS Reference, https://postgis.net/docs/manual-2.0/reference.html
* PostGIS workshop (previously Boundless), http://postgis.net/workshops/postgis-intro/
