mysql-fix-utf8
==============

Convert a MySQL database from Latin-1 to UTF-8

This script is an implementation of the steps described in the excellent blog post by Stephen Balukoff [Getting out of MySQL Character Set Hell](http://www.bluebox.net/about/blog/2009/07/mysql_encoding/)


## Problem: 

You have a MySQL database that contains "broken special characters". Perhaps the database was running for a long time behind a Ruby or PHP application without much thought given to character encoding. Special characters appear corrupted in some places, and correctly in other places. You now want to fix your application and database to correctly handle special characters using Unicode. You've made the necessary changes in your application, but you still have to address the problem in your MySQL database. 


MySQL has historically used latin1 as its default character encoding. Using latin1 can result in international and special characters being encoded and displayed incorrectly in some instances, especially if parts of your application expect a different encoding. Using UTF-8 consistently, everywhere, is usually the recommended way to handle character data and avoid such problems. Fixing these problems after they've had time to grow can be a challenge. This script converts a MySQL database from latin1 to UTF8 by dropping and recreating the database, then looping through every table and every column to correct characters.



## Using this script:

First, read the article at https://www.bluebox.net/insight/blog-article/getting-out-of-mysql-character-set-hell. Do your research and decide on the best approach. Make any changes necessary for your application to use UTF-8 (which might include adding "encoding: utf8" to your database connection properties).

To use this script you will need a linux shell, Perl, and a recent version of MySQL.

Make a copy of your production database into a test environment. **Do not run this script on a live database.**  Using the copy of your database:

1. Edit mysql_fix_utf8.sh and change the lines at the top to provide the name of your database, password and username. The user must have the ability to drop and create databases (many shared hosting services restrict this ability). If you want to start with an existing mysqldump backup file instead of a MySQL database comment out the mysqldump line.

2. If you need to exclude certain tables or columns from the double-encoding fix modify procedure_fix_utf8.sql where indicated in the comments.

3. Run mysql_fix_utf8.sh: `chmod u+x mysql_fix_utf8.sh && ./mysql_fix_utf8.sh`

Test and examine the effect of the script on your data. Once you're satisfied, decide how you want to apply the change on your production database.


## YMMV

Naturally any situation will have its own unique set of variables. This script might do what you need, or you may need to modify it for your situation, or this script might be an example that helps you create your own script. This script can complete in a few minutes on a small database with a few hundred thousand rows. On a larger database you might want a script that uses an incremental approach.

The shell script mysql_fix_utf8.sh uses bash shell to run commands on mysql and mysqldump. It also uses Perl to do a find-and-replace. The decision to use bash and Perl was purely for my own convenience.


## Disclaimer

This code is public domain. It is provided with no warranty or guarantee of any kind. Use at your own risk.

