# CCDD
Core Flight System (CFS) Command and Data Dictionary (CCDD) utility

*** CCDD Version 1.5.37 ***

*** CCDD works with JAVA 7-13 ***

CCDD is a software tool for managing the command and telemetry data for CFS and CFS applications.  CCDD is written in Java™ and interacts with a PostgreSQL database, so it can be used on any operating system that supports the Java Runtime Environment (JRE) and PostgreSQL.  CCDD is released as open source software under the NASA Open Source Software Agreement, version 1.3, and is hosted on GitHub.

The CCDD application uses tables, similar to a spreadsheet, to display and allow manipulation of telemetry data structures, command information, and other data pertinent to a CFS project.  The data is stored in a PostgreSQL database for manipulation and data security.  The PostgreSQL database server can be run locally or centralized on a remote host for easier access by multiple users.  Data can be imported into the application from files in comma-separated values (CSV), JavaScript Object Notation (JSON), electronic data sheet (EDS), and extensible markup language (XML) telemetric and command exchange (XTCE) formats.  Data can be exported from the application to files in CSV, JSON, EDS, and XTCE formats.  The CCDD tables also allow simple cut and paste operations from the host operating system’s clipboard.  To make use of the project’s data, CCDD can interact with Java Virtual Machine (JVM)-based scripting languages via a set of supplied data access methods.  Using scripts, the user can translate the data stored in the CCDD’s database into output files.  Example scripts for creating common CFS related output files are provided in four of these scripting languages.  An embedded web server can be activated, allowing web-based application access to the data.

See the CCDD user's guide for details on set up and use.

## CCDD version 2 

Version 2 redefines the behavior of command tables.  Command arguments are no defined as columns within a command table.  Instead, the command table has a column that is a reference to a structure table; this structure defines the command argument(s).  The version 2 user's guide is updated to provide further details.

When version 2 attempts to open a version 1.x.x version project database then a dialog appears asking to convert the project.  Unlike previous patches, this patch alters user-defined tables and table definitions, and creates new ones.  The argument columns in any command tables are replaced with the argument structure reference column, and the argument structure is created and populated using the original argument information.  Many of the command table script data access methods no longer exist, so existing scripts may need to be updated. Before this patch is applied to the version 1.x.x database a backup will be performed to ensure no data loss on the chance that something does not work as anticipated. 

*** Noteable updates to CCDD V2 ***

* CCDD version 2 works with JAVA 7-13
* psql 8.4 - 11 is now supported. You can now create a .dbu backup on a machine with psql 11 and restore it on a machine with psql 8.4 or vice versa.
* CCDD version 2 has changed the way that the json import/export works. You can now import and export entire databases. Check CCDDv2 users guide for more detail
* Added a new checkbox to the export dialog that allows the user to decide if they would like to clear the contents of the target directory before an export.
* CSV import/export functionality has been updated to match the new JSON import/export functionality. You can now export entire databases, including internal tables, to JSON or CSV files. The data can be placed in separate files, every table in the database will get its own file, or one large file. This is useful for tracking changes to the database as these files can be easily checked into git. This also allows the user to make small changes to these files then perform an import which will update the database as needed.
* CCDD can now detect and better report issues related to unexpected errors during an import
* Multiple performance optimizations that greatly increase the speed at which a database can be opened or verifived and the speed at which CCDD can process a change in very large macros.

*** Version 2.0.23 has been released ***

Below is a brief description of what has changed from version 2.0.22 to 2.0.23.
* CCDD can now import and verify 3d arrays
* CCDD can now 'replace existing associations' during an import.
* Fixed a few issues related to changing the size of a macro during an import. Could previously cause errors if the macro was used to define the size of an array.
* Fixed a few issues related to data fields being duplicated for many tables after an import.
* Added a new option to the export command that is used within scripts. For the 'tablepaths' sub-command you can now specify 'all' to export ALL tables within a database.
* Fixed a few issues that prevented importing xtce files.
* Fixed a few issues that were causing array members not to update during an import if they were a member of a non-root prototype table.
* Added new functionality to CCDD so that a user can now import new table type or changes to an existing table type if they wish. All associated files will be updated during the import. 
* Updated the users guide to include information related to the new 'tablepaths' option and the new 'replaceExistingAssociations' flags.


*** Version 2.0.24 has been released ***

* CCDD has 2 new data types which can be assigned in the data type manager which are 'Structure' and 'Enum'. The Structure data type was added so that users do not have to define a table for every 'Structure' based data type they wish to use. So if you have a Structure that is not defined within your database, like CFE structures that are often shared, but you wish to assign the type to a variable you can now add that type to the data type manager without creating a table and defining all the elements that the structure consists of. The same goes for the 'Enum' type but this can be used for 'typedef enum' structures. 
* A new table type was added to CCDD called 'Enum'. This new table allows users to store the names of all of the enums that make up a 'typedef enum' structure. Once this table is defined users can assign the data type to any variable they wish.
* Two new data access methods were added for the new 'Enum' table type so that a user can retrieve all defined Enum tables and their members via scripts. More details available in the users guide.
* The copy and paste functionality has been fixed. 

*** Version 2.0.25 has been released ***

Below is a brief description of what has changed from version 2.0.24 to 2.0.25.
* Addressed a bug that prevented CCDD from auto-correcting data field related issues found during the verification process
* Addressed a bug that prevented users from importing JSON or CSV files that contained info for tables without any rows of data
* Addressed a bug that prevented group data from being exported in CSV format when any of the tables were array members
* Refactored the assign-message-id dialog. Users can now use a table tree to select/omit tables when assigning message ids. Details can be found in the users guide.
* Broke the users guide up to make it more approachable and moved all documents into the 'Docs' directory.
* Updated the CCDD tutorial
* Refactored the code that imports an entire database from a single JSON/CSV file to be more efficient
* Introduced new data access methods in an attempt to make data easier to access in the future while retaining all current data access methods to prevent breaking any existing   scripts that projects may be using.
* Addressed a bug that prevented a new root and a child of the new root from being imported at the same time
* Refactored the code that imports data fields as users were encountering many issues when importing various data field changes.
* Refactored the code that clears a directory prior to an export so that it can no longer delete folders. It can only delete JSON or CSV files now. * This is to prevent accidental deletion of unrelated files if an improper directory is selected for an export
* Addressed a performance issue that caused some scripts to take an abnormally long time to finish
* The external C_Header_To_CSV script is no longer needed when attempting to import C header files into CCDD. A user can now directly import them into CCDD by selecting the C_Header import option within CCDD. More details can be found in the users guide.
* Addressed performance issues that were causing the script manager to take an abnormally long time to launch when a large CCDD database was open.
* Addressed performance issues that were causing the 'Filter By Group' option to take an abnormally long time to rebuild the table tree for large databases.
* Addressed an issue that prevented group data fields from being imported via JSON or CSV
* Merged the 'Show variables' and 'Search variables' dialogs
* Addressed a bug that was preventing the table type data from being exported unless the entire database was being exported

*** Version 2.0.26 has been released ***

Below is a brief description of what has changed from version 2.0.25 to 2.0.26.

* Updated the copy and paste functionality so that users can now copy and paste data to the array size column while also fixing an issue preventing users from pasting large amounts of data all at once.
* Addressed an issue with the copy and paste functionality that prevented users from copying and pasting booleans
* Addressed an issue with pre-loaded table members that was causing an error when opening a new database immediately after closing another
* Addressed an issue that prevented importing changes directly into a table editor
* Addressed an issue where the 'All Tables' group was not being updated properly
* Updated CCDD to export Reserved Message ID's and Project Data Fields to their own individual files.
* No longer need to select files from the table tree when exporting the entire database. Just click the 'Export Entire Database' checkbox and all contents of the database are exported
* Addressed a bug that was causing duplicated data fields when converting a root table to a child table
* CCDD can now accurately track the size of an enum table by looking at the user defined size field of the table
* Added a values column to the enum table type
* CCDD now properly exports file creation info
* Addressed an issue that was causing scripts to be grayed out due to missing table members caused by an improper search term
* Changes to the table type editor are now properly applied to the rest of the database. This includes any open table editors
* When data fields are imported all open table editors are properly updated. Fixed the 'Append Existing Fields' and 'Use Existing Fields' functionality.
* Copy and paste no longer allows you to modify immutable data.
* Added file headers to each file in CCDD that includes a file description and updated copyright message
* Addressed a performance issue related to the script manager.
* Addressed an issue related to a XTCE export error when exporting arrays
* Increased import performance when importing entire databases from a single JSON or CSV file
* A non-root, non-primitive data type that is converted to a root is now updated as expected.
* Addressed an issue where the C-Header import was not capturing macros
* Addressed an issue that was causing some JSON exports to not include the final brace and bracket. This was preventing users from importing the files.
* Addressed an issue involving renaming tables that caused the table tree not to reflect the new name
* CCDD now checks for duplicate message IDs during the verification process.
* CCDD can now restore a database from JSON or CSV files. Check the users guide for details
