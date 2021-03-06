ForgeUpgrade
============

**ForgeUpgrade** aims to propose a convenient way to perform automated upgrade of a Forge.

See the [spec](https://codendi.org/wiki/index.php?pagename=UpgradeAutomation&group_id=104) on Codendi.org

Install
=======

As of today, only Codendi db driver is developed. Before being able to use migration tool, you must
initialize the database (TODO: use ForgeUpgrade to install itself):
- execute db/install-mysql.sql
- set CODENDI_LOCAL_INC to the path toward your codendi local.inc file

then, all your migrations commands will looks like:
php migration.php --dbdriver=examples/CustomDrivers/ForgeUpgrade_Db_Driver_Codendi.php ...

Features
========
The command line tool now allows to select where to look for migration scripts:
<pre>
$> php migration.php --path=tests/_fixtures/CoreAndPlugins --include="db/updates" check-update
</pre>

This will look for migrations in "tests/_fixtures/CoreAndPlugins" subdirectory
but only in "db/updates" subpath.
It will match:
tests/_fixtures/CoreAndPlugins/src/db/updates/...
tests/_fixtures/CoreAndPlugins/plugins/foobar/db/updates/...

Usage example
=============
<pre>
$> php migration.php
Wed Apr 14 18:01:40 2010,781 [30794] INFO ForgeUpgrade - [Pre Up] Run pre up checks
Wed Apr 14 18:01:40 2010,787 [30794] INFO ForgeUpgrade - [Pre Up] OK : AddTablesForDocmanWatermarking
Wed Apr 14 18:01:40 2010,789 [30794] INFO ForgeUpgrade - [Pre Up] SKIP: AddDateColumnToItem depends on a migration not already applied
Wed Apr 14 18:01:40 2010,790 [30794] INFO ForgeUpgrade - [Pre Up] Global: OK
Wed Apr 14 18:01:40 2010,791 [30794] INFO ForgeUpgrade - [Up] Start running migrations...
Wed Apr 14 18:01:40 2010,791 [30794] INFO ForgeUpgrade - [Up] AddTablesForDocmanWatermarking
Add tables to docman pdf watermarking plugin in order to
allow admins to disable watermarking on selected documents.
Wed Apr 14 18:01:40 2010,792 [30794] INFO ForgeUpgrade - [Up] AddTablesForDocmanWatermarking PreUp OK
Wed Apr 14 18:01:40 2010,792 [30794] INFO ForgeUpgrade - Add table plugin_docmanwatermark_item_excluded
Wed Apr 14 18:01:40 2010,796 [30794] INFO ForgeUpgrade - plugin_docmanwatermark_item_excluded already exists
Wed Apr 14 18:01:40 2010,797 [30794] INFO ForgeUpgrade - Add table plugin_docmanwatermark_item_excluded_log
Wed Apr 14 18:01:40 2010,800 [30794] INFO ForgeUpgrade - plugin_docmanwatermark_item_excluded_log already exists
Wed Apr 14 18:01:40 2010,801 [30794] INFO ForgeUpgrade - [Up] AddTablesForDocmanWatermarking Up OK
Wed Apr 14 18:01:40 2010,807 [30794] INFO ForgeUpgrade - [Up] AddTablesForDocmanWatermarking Done
Wed Apr 14 18:01:40 2010,807 [30794] INFO ForgeUpgrade - [Up] AddDateColumnToItem
Add column to DocmanWatermarking table.
Wed Apr 14 18:01:40 2010,810 [30794] INFO ForgeUpgrade - [Up] AddDateColumnToItem PreUp OK
Wed Apr 14 18:01:40 2010,811 [30794] INFO ForgeUpgrade - [Up] AddDateColumnToItem Up OK
Wed Apr 14 18:01:40 2010,812 [30794] INFO ForgeUpgrade - [Up] AddDateColumnToItem Done
</pre>

Dedicated db driver
===================

Instead of writing down your db crendentials in the config.ini file, you can
develop your own db driver that will be able to read your application db config
file.

This allows to keep these sensitive informations in only one place (more secure
and avoid the changes).

To develop your own driver, just create a file that inherits from
ForgeUpgrade_Db_Driver_Abstract. The file must be named after the class name plus
.php extension. For instance:
ForgeUpgrade_Db_Driver_Codendi & ForgeUpgrade_Db_Driver_Codendi.php

Then, just use --dbdriver= option with the path of the file to use as driver

You can have a look at examples/CustomDrivers/ForgeUpgrade_Db_Driver_Codendi.php
for more details
