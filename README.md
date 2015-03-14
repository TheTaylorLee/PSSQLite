Invoke-SQLiteQuery PowerShell Module
=============

This is a PowerShell module for working with SQLite.  It uses similar syntax to the [Invoke-Sqlcmd2](https://github.com/RamblingCookieMonster/PowerShell/blob/master/Invoke-Sqlcmd2.ps1) function from Chad Miller et al.

This covers limited functionality; contributions to this function or additional functions would be welcome!

Caveats:
* Minimal testing.
* Today was my first time working with SQLite

#Functionality

Create a SQLite database and table:
  * ![Create a SQLite database and table](/Media/Create.png)

Query a SQLite database, using parameters:
  * ![Query a SQLite database](/Media/Query.png)

#Instructions

```powershell
# One time setup
    # Download the repository
    # Unblock the zip
    # Extract the Invoke-SQLiteQuery folder to a module path (e.g. $env:USERPROFILE\Documents\WindowsPowerShell\Modules\)

# Import the module.
    Import-Module Invoke-SQLiteQuery    #Alternatively, Import-Module \\Path\To\Invoke-SQLiteQuery

# Get commands in the module
    Get-Command -Module Invoke-SQLiteQuery

# Get help for a command
    Get-Help Invoke-SQLiteQuery -Full

# Create a database and a table
    $Query = "CREATE TABLE NAMES (fullname VARCHAR(100) PRIMARY KEY, surname VARCHAR(50), givenname VARCHAR(50), BirthDate DATETIME)"
    $Database = "C:\Names.SQLite"

    Invoke-SqliteQuery -Query $Query -Database $Database
    
# View table info
    Invoke-SqliteQuery -Database $Database -Query "PRAGMA table_info(NAMES)"
    
# Insert some data, use parameters for the fullname and birthdate
    $query = "INSERT INTO NAMES (fullname, surname, givenname, birthdate) VALUES (@full, 'Cookie', 'Monster', @BD)"
    
    Invoke-SqliteQuery -Database $Database -Query $query -SqlParameters @{
        full = "Cookie Monster"
        BD   = (get-date).addyears(-3)
    }
    
# View the data
    Invoke-SqliteQuery -Database $Database -Query "SELECT * FROM NAMES"
    
```

#Notes

This isn't a fully featured module or function.

I'm planning to write about using SQL from a systems administrator or engineer standpoint.  I personally stick to [MSSQL and Invoke-Sqlcmd2](https://ramblingcookiemonster.wordpress.com/2014/03/12/sql-for-powershell-for-sql-newbies/), but want to provide an abstracted means to perform this without the prerequisite of an accessible MSSQL instance.