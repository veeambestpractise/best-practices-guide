# Domino

Veeam supports the backup and restore of IBM Lotus Domino.

## Background

To protect the IBM Lotus Domino following files and folder are required to be backup:

•	Domino server data files
•	All databases
•	Template files
•	notes.ini file
•	ID files
•	Mail.box

As IBM Lotus Domaino is non VSS-aware application, to take the consistent backup of Lotus Domino, We need to adopt the mechanism to stop and start the IBM Lotus Domino service with scripts for the backup.

Procedure: 

Windows:

Follow the steps below:

Pre Backup Script:
1. open notepad
2. Copy net stop "Lotus Domino Server (CDominodata)"
3. Save as .bat file, name the script as Shutdown.bat

Post/Thaw Script:

1. open notepad
2. Copy net start "Lotus Domino Server (CDominodata)"
3. Save as .bat file, name the script as Shutdown.bat

Linux:
