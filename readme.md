
~~
About this solution template
This template retrieves a list of source database partitions to copy from an external control table. Then it iterates over each partition in the source database and copies the data to the destination.

The template contains three activities:

Lookup retrieves the list of sure database partitions from an external control table.
ForEach gets the partition list from the Lookup activity and iterates each partition to the Copy activity.
Copy copies each partition from the source database store to the destination store.
The template defines following parameters:

Control_Table_Name is your external control table, which stores the partition list for the source database.
Control_Table_Schema_PartitionID is the name of the column name in your external control table that stores each partition ID. Make sure that the partition ID is unique for each partition in the source database.
Control_Table_Schema_SourceTableName is your external control table that stores each table name from the source database.
Control_Table_Schema_FilterQuery is the name of the column in your external control table that stores the filter query to get the data from each partition in the source database. For example, if you partitioned the data by year, the query that's stored in each row might be similar to ‘select * from datasource where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''.
Data_Destination_Folder_Path is the path where the data is copied into your destination store (applicable when the destination that you choose is "File System" or "Azure Data Lake Storage Gen1").
Data_Destination_Container is the root folder path where the data is copied to in your destination store.
Data_Destination_Directory is the directory path under the root where the data is copied into your destination store.





How to use this solution template
Create a control table in SQL Server or Azure SQL Database to store the source database partition list for bulk copy. In the following example, there are five partitions in the source database. Three partitions are for the datasource_table, and two are for the project_table. The column LastModifytime is used to partition the data in table datasource_table from the source database. The query that's used to read the first partition is 'select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''. You can use a similar query to read data from other partitions.


Create table ControlTableForTemplate
 		(
 		PartitionID int,
 		SourceTableName  varchar(255),
 		FilterQuery varchar(255)
 		);

 		INSERT INTO ControlTableForTemplate
 		(PartitionID, SourceTableName, FilterQuery)
 		VALUES
 		(1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
 		(2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
 		(3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
 		(4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
 		(5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');