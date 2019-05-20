# Shrink the MySQL ibdata1 file
Bash shell script to shrink the MySQL metadata file and restore disk space.  This script should work with all MySQL variants such as Percona and MariaDB.

## Overview:
Over time, MySQL databases are added and tables are populated which can cause the /var/lib/mysql/ibdata1 metadata file to grow.  This script makes it easy to shrink
this file and recover valuable disk space.  The script is based loosely on the steps outlined here: http://dba.stackexchange.com/questions/8982/what-is-the-best-way-to-reduce-the-size-of-ibdata-in-mysql.

## Installation:
```
$ git clone https://github.com/uberhacker/shrink-ibdata1.git
$ sudo mv shrink-ibdata1/shrink /usr/local/bin
```
## Usage:
```
$ shrink [-k]
```
Use the -k switch to keep backups instead of restoring the databases

## Examples:
To shrink the MySQL metadata file and restore the databases:
```
$ shrink
```
To shrink the MySQL metadata file and keep the backups instead of restoring the databases:
```
$ shrink -k
```
You might want to do this if you wish to clear out MySQL of any existing databases.

### Warning: Make a backup of your databases just in case
```
Although this script will perform a backup and restore of the databases, it is still advisable to make your own backup of /var/lib/mysql and all the databases prior to using.
```

### Possible error amb les rutines (funcions i stored procedures)
I got the following error message while restoring the database on the destination server:
```
ERROR 1419 (HY000) at line 1140: You do not have the SUPER privilege and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
The error message is pretty clear. Binary logging needs to be enabled in order to restore the functions. There are two ways for doing this:
```
Execute the following command into the MySQL console:
```
SET GLOBAL log_bin_trust_function_creators = 1;
```
Add the line log_bin_trust_function_creators = 1 to the mysql.ini configuration file
