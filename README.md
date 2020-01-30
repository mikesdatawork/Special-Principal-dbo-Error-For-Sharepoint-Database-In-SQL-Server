![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Special Principal 'dbo' Error For Sharepoint Database In SQL Server
**Post Date: August 3, 2016**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>If you arrived here then you've already seen that pesky "…cannot use the special principal 'dbo'" error whenever you try to update, or edit permissions for the Sharepoint Config Database. OMG annoying right??!!
Rather than giving you a long drawn-out verbose description of the Microsoft Security Architecture between Sharepoint, Powershell, and SQL Server, why don't I just tell you a quick fix that might work for you.
You'll need to remove the existing Database Owner of your Sharepoint Configuration Database then update the permissions afterwords. Just as you normally would… Nothing special.
In this scenario I have a Sharepoint 2013 configuration on an environment with SQL Server 2014 using AlwaysOn. I'm using an example domain called 'MyDomain', and a Sharepoint Service Account called MyDomainSP_Admin. The Sharepoint Config database is SharePoint2013_Config.</p>   

![Find User Mapping]( https://mikesdatawork.files.wordpress.com/2016/08/image001.png "Find User Mapping")
 
Here's the logic I used to quickly update the permissions for DB_Owner, Sharepoint_Shell_Access, and SPDataAccess. I'm doing all three for consistency in this example even though I'm well aware of the permissions hierarchy in this situation.



## SQL-Logic
```SQL
use master;
set nocount on
exec [Sharepoint2013_Config]..sp_changedbowner 'sa';
 
use [Sharepoint2013_Config];
create user [MyDomainsp_admin] for login [MyDomainsp_admin]
alter role [db_owner] add member [MyDomainsp_admin]
alter role [sharepoint_shell_access] add member [MyDomainsp_admin]
alter role [spdataaccess] add member [MyDomainsp_admin]
go
```

![Sample SQL Query]( https://mikesdatawork.files.wordpress.com/2016/08/image002.png "Sample SQL Query")
 




[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

   
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

