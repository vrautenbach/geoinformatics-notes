- [Overview](#overview)
- [Creating a spatial database in pgAdmin 4](#creating-a-spatial-database-in-pgadmin-4)
- [Loading a CSV into a new table](#loading-a-csv-into-a-new-table)
- [Loading spatial data into a table using QGIS](#loading-spatial-data-into-a-table-using-qgis)
  * [Connection a PostGIS database to QGIS](#connection-a-postgis-database-to-qgis)
  * [Loading a QGIS layer into a database](#loading-a-qgis-layer-into-a-database)
  * [Basics of Spatial SQL](#basics-of-spatial-sql)
- [Overview of SQL and Spatial SQL](#overview-of-sql-and-spatial-sql)
  * [Basic SQL notation and operations](#basic-sql-notation-and-operations)
  * [Basic examples:](#basic-examples-)
  * [Joining tables using SQL](#joining-tables-using-sql)
  * [Storing query results in a new table](#storing-query-results-in-a-new-table)
- [pgAdmin 4 Geometry Viewer](#pgadmin-4-geometry-viewer)
- [Other resources](#other-resources)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# Overview

This page provides resources and some guidelines on how to get started with PostGIS. Note that this page provides a basic overview and is not meant to be step-by-step instructions. You have to adapt information available online for your purposes to complete the task.

# Creating a spatial database in pgAdmin 4 
[pgAdmin](https://www.pgadmin.org) is the most popular and feature rich Open Source administration and development platform for PostgreSQL, the most advanced Open Source database in the world. You use pgAdmin to manage and interact with the data in the database. 

To open pgAdmin, go to the `Start` --> `Applications` --> `PostgreSQL 10.x`, and select pgAdmin4. The pgAdmin interface will now open (you should see an elephant on a splash screen). The image below shows what the interface looks like.

![pgAdmin Loading screen](https://github.com/vrautenbach/geoinformatics-notes/blob/master/images/SplashScreen.png)


To create a new database, use the menu at the top, `Object` --> `Create` --> `Database`. Add a database name that is appropriate and under the **Definition** tab, select the postgis template (see below). The general rule of thumb is to ***NOT*** use capital letters, special characters and spaces. This creates complications, so try to avoid this.

![Create New Database](https://github.com/vrautenbach/gis311-notes/blob/master/images/CreateNewDatabase.png)


There is two ways to check if you successfully created a spatial database (i.e. meaning that you will be able to load geometries into the database):
1. Expand the **Extensions** section, you should see the **postgis** and **postgis_topology** extensions.
2. Under **schemas**, you should have a **topology** schema and in the public schema there will be table called **spatial_ref_sys**.

![Spatial Databases](https://github.com/vrautenbach/gis311-notes/blob/master/images/SpatialDatabases.png)


If you missed the step to create a spatial database, click on Extensions and add the **postgis** and **postgis_topology** extensions.

# Loading a CSV into a new table
[This tutorial](http://www.postgresqltutorial.com/import-csv-file-into-posgresql-table/) provides a good example of how to load a CSV into a database. You should adapt the information in the tutorial to work with your data.

In pgAdmin4, you can create and alter your tables using SQL but also the GUI. To access the GUI, right click on your table name and select **Properties**.

# Loading spatial data into a table using QGIS
## Connection a PostGIS database to QGIS
In order to load QGIS layers into a PostGIS table or view a PostGIS table in QGIS, the first step is to create a connection between QGIS and PostgreSQL. To create a new connection, go to, `Layer` --> `Add layer` --> `Add PostGIS layers`. Next, select **New**. The following popup will appear.

![QGIS Connection](https://github.com/vrautenbach/gis311-notes/blob/master/images/QGIS_Connection.png)

Add the following information:
* *Name* --> this is the name of the connection.
* *Host* --> In your case this would be localhost. If the database is on an off site server, you would add the IP of the server here.
* *Port* --> The default port is 5432, this should be the same unless you changed it during installation.
* *Database* --> This is the database that you would like to connect to (i.e. the database you created earlier).
* *Authentication* --> Select the **Basic** tab, and add the username and password. In the lab the username and password would be ***postgres***.

After you have added all the information, you can test the connection, and thereafter complete the process by pressing OK.

## Loading a QGIS layer into a database
The first step is to open the layer in QGIS that you would like to import into your database. I also assume you have created the PostGIS connection describe in the previous section.

Once you are set, in the main menu go to `Database` --> `DB Manager`. In the DB Manager, navigate to the connection and schema. See an example below.

![DB Manager](https://github.com/vrautenbach/gis311-notes/blob/master/images/Db_manager.png)

Next click on the `import` option. The `Import vector layer` popup will appear. Please complete the following:
* *Input* --> Select the layer you would like to import into your database.
* *Schema* --> This is the schema you will use. In most cases this will be public.
* Select the following options:
    * *Geometry column* --> leave this as geom
    * *Source SRID*
    * *Target SRID*
    * *Encoding*
    * *Convert field names to lowercase*
    * *Create spatial index* --> this make access to the table in PostgreSQL faster

[Import Vector](https://github.com/vrautenbach/gis311-notes/blob/master/images/Import_vector.png)

It is important to add your SRID, else you will not be able to execute spatial queries. A spatial reference identifier (SRID) is a unique identifier associated with a specific coordinate system, tolerance, and resolution. If you don't know what the SRID is for your shapefile, use [this site](https://epsg.io) to select an appropriate SRID.


## Basics of Spatial SQL
Spatial SQL is an extension of SQL with spatial function. You can identify these function easily as they start with ***ST_***.

Below is an example of how you would run an intersect in spatial SQL:

```
SELECT a.*
FROM table1 a, table2 b
WHERE ST_Intersects(a.geom, b.geom)
```

You can learn more about spatial SQL from the [Boundless PostGIS workshop.](http://postgis.net/workshops/postgis-intro/index.html) This tutorial uses the previous version of pgAdmin, but focus on the SQL statements and how they work.

# Overview of SQL and Spatial SQL
## Basic SQL notation and operations
Structured Query Language (SQL) is a standard language for accessing and manipulating databases. SQL operates through simple, declarative statements. This keeps data accurate and secure, and it helps maintain the integrity of databases, regardless of size.

An SQL statement generally consists of three parts:

*SELECT*   --> What is the results? What would you like to see?

*FROM*      --> Which tables are you using?

*WHERE*   --> Are there any conditions?

I consider [w3schools](https://www.w3schools.com/sql/sql_intro.asp) the best resource when writing SQL statements.

## Basic examples:
* List all the record in the table:
This would be a basic SQL SELECT statement. Here is an [example.](https://www.w3schools.com/sql/sql_select.asp)

* Selecting all records that fulfils a specific condition:
This would require using the *WHERE* statement and maybe adding operators, such as *AND* or *OR*. Here is an [example](https://www.w3schools.com/sql/sql_and_or.asp).

* Calculating a sum of records that fulfil a specific condition (i.e. aggregating data):
This would require using aggregation operator, such as *COUNT()*, *SUM()* or *AVG()*. Here is an [example](https://www.w3schools.com/sql/sql_count_avg_sum.asp).

## Joining tables using SQL
A ***JOIN*** clause is used to combine rows from two or more tables, based on a related column between them. In our case, we will find a column that is similar in both tables and ***JOIN*** based on that column. You can learn more about ***JOINS*** [here.](https://www.w3schools.com/sql/sql_join.asp)

You will have to use on of the variations of the ***JOIN*** when completing the task.

## Storing query results in a new table
An easy way to store your results in a new table is to use the INTO operation. Learn more [here.](https://www.w3schools.com/sql/sql_select_into.asp)

# pgAdmin 4 Geometry Viewer
When you run a spatial SQL statement in pgAdmin, you often want to view the results. As of late-2018 pgAdmin now has a built-in geometry viewer. You can access the geometry viewer by clicking the eye icon in the geom column.

# Other resources 
**SQL resources:**
* [SQL Quick Reference From W3Schools](https://www.w3schools.com/sql/sql_quickref.asp)
* [SQL Quick Guide - TutorialPoint](https://www.tutorialspoint.com/sql/sql-quick-guide.html)
* [Intro to SQL: Querying and managing data](https://www.khanacademy.org/computing/computer-programming/sql)

**PostGIS resources:**
* [Getting Started With PostGIS: An almost Idiot's Guide - Boston GIS](http://www.bostongis.com/?content_name=postgis_tut01)
* [PostGIS documents](https://postgis.net/docs/manual-2.5/)
* [PostGIS Reference](https://postgis.net/docs/manual-2.0/reference.html)
* [PostGIS workshop (previously Boundless)](http://postgis.net/workshops/postgis-intro/)
