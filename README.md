# Gdpr
Script that removes sensitive data from db  
To be executed on db before providing qualisytems customer database.

Script that Masks\Changes:
-	User Names.
-	Passwords.
-	Emails.

Usage:
1.	Open SQL Server Management Studio (SSMS).
2.	Backup the DB.
For specific instructions how to do Backup and restore please refer to the Microsoft guide located in the following Link:
https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/quickstart-backup-restore-database?view=sql-server-ver15
3.	Restore the Backup file – make sure to rename it.
4.	Open the attached script.
5.	Enter the relevant DB Name instead of [DatabaseName] within the script.
6.	Please Notice that you need to set @RetainRowsCount to higher value in order to Retain Data in table [dbo].EventLog which is used for debugging issues in Cloudshell.
This will affect the Run Time of the script.
7.	Execute the script.
8.	After the script will finish running please run the following SQL statement and verify user data was changed\Masked.
        select * from UserInfo
9.	Create a backup file from the DB.

The effect on tables:
-	AttributeInfo
o	Setting User\Password Attributes String Value or Default Value to ‘******’ for Resources.
-	HttpSimpleRepository  
o	This Table will be deleted entirely.
-	TestShelObjectLocker 
o	This Table will be deleted entirely.
-	UserInfo 
o	Admin Username – Admin Sid and Password  will be changed for us to be able to use that user when we will get the DB. (The Password is Quali!Pass@Fail3)
o	Email’s - Change the user’s Emails to fake ones.  (fake@qualisystems.com)
o	Username – The User Names will be replaced with the id column as a prefix + superuser as follows id_superuser (Example: 39_superuser)
o	Display Name - The Display Names will be replaced with the id column as a prefix + superuser as follows id_superuser (Example: 39_superuser)
-	Topology
o	Setting any Email occurrence to ‘fakevalue’ 
-	JobTopologyLink
o	Setting any Email occurrence to ‘fakevalue’ 
-	PublishedTopologyParameter
o	Setting any Email occurrence to ‘fakevalue’ 
-	PublishedProperty
o	Setting any Email occurrence to ‘fakevalue’ 
-	CommandOutput
o	Setting any Email occurrence to ‘fakevalue’ 
-	EventLog
o	Storing latest 1,000,000 rows and dropping the rest for debugging purposes. (by setting a Parameter within the script named @RetainRowsCount)  
