# Sqoop-Injection-Commands
Sqoop is an Data Injection tool to import data from Relational Databases such as Mysql, oracle to Hadoop HDFS(Hadoop Distributed File System) and also we can export data from HDFS to RDBMS, we can inject data with columnar format or conditionally from sqoop. 

========START MYSQL=========
we need to start Mysql first for data injection

mysql -u root -p
hr
show databases;
use Harshal;

select * from employees;

========RDBMS TO HDFS=========
sqoop import command is used to transfer data from mysql to HDFS, in which we need to provide username and password for database. 

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table employees --target-dir /sqoop/ex1

========COLUMNAR IMPORT=========
we can import data from Mysql as per specific columnwise also for that --column command is used

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table employees --columns 'FIRST_NAME,SALARY' --target-dir /sqoop/ex1

========CONDITIONAL IMPORT=========
we can import data from Mysql as per specific condition also for that --where command is used

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table employees --where 'SALARY > 9000' --target-dir /sqoop/ex3

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table employees --where 'FIRST_NAME LIKE "A%"' --target-dir /sqoop/ex

========SERIAL IMPORT=========
we can imprort data within one single part file also by using -M number_of_part_files

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table employees --target-dir /sqoop/ex5 
-m 1

========== SPLIT BY Clause===========
we can split the data while importing from Mysql, for that we can use --Split by clause, if any table which does not having any primary key on column in this condition we can apply split by on that spwcific column
--query clause is used to provide query in sqoop itself and getting result based on that query.

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --query 'select * from employees join DEPT ON (employees.DEPARTMENT_ID = DEPT.DID) WHERE $CONDITIONS' --target-dir /sqoop/ex5 --split-by 'EMPLOYEE_ID'

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --username root -P --query 'select * from employees join DEPT ON (employees.DEPARTMENT_ID = DEPT.DID) WHERE $CONDITIONS' --target-dir /sqoop/ex5 --split-by 'EMPLOYEE_ID'

=============PASSWORDLESS IMPORT===================
we can import data using 3 methods of password provision

1. Hardcode the password in query itsel
2. Using -P clause and enter password on console 
3. Using --password-file clause by giving password file path 

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --table employees --username root -P --target-dir /sqoop1/example1

sqoop import --connect jdbc:mysql://localhost:3306/Harshal --table employees --username root --password-file /data1/passw/pw.txt --target-dir /sqoop1/example1

===============IMPORT-ALL-TABLES==============
 import-all-tables clause is used to import all tables from database, while running this query sqoop finds MIN & MAX values for each table
 
sqoop import-all-tables --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --target-dir /sqoop/ex2

=============SQOOP JOB===================
job command is used to save any particular query for running multiple times, it having below commands

--delete
--exec 
--show 
--list

sqoop job --list     #used to list all jobs on console

sqoop job --create J2 -- import --table employees --connect jdbc:mysql://localhost:3306/Harshal --username root -P --target-dir /data/sq1    #used to create new job

sqoop job --exec J2  #used to execute new job

===============SQOOP EXPORT==============
export clause is used to export data from HDFS to RDBMS

sqoop export --connect jdbc:mysql://localhost:3306/Harshal --username root --password hr --table emp --export-dir /sqoop1/example1 --input-fields-terminated-by ','
