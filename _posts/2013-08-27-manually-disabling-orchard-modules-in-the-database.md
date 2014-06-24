---
layout: post
title: Manually Disabling Orchard Modules in the Database 
permalink: manually-disabling-orchard-modules-in-the-database 
---
Sometimes when doing upgrades or tracking down production issues, I've needed to disable a module manually without the command line or web interface.  This is tricky because there seems to be three different places where this information is held.  Two of them are database tables (Settings_ShellFeatureRecord, Settings_ShellFeatureStateRecord).  The other is a cache.dat file in the App_Data folder.  So, my steps for disabling a module manually are as follows.

1. Stop the website in IIS or development web server.
2. Delete the records from the database where the name field is the name of your module. (Settings_ShellFeatureRecord, Settings_ShellFeatureStateRecord)
3. Delete the cache.dat file from App_Data
4. Start the website back up

Now your module should be disabled.  
