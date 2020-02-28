# Sql Server

## SQLs

#### Default Guid value in column

```text
ALTER TABLE #Test ADD [GIANGGUID] uniqueidentifier NOT NULL DEFAULT NEWID();
```

#### sequence

```text
var sequence = 
    await _dbContainer.ExecuteScalar<long>($"SELECT NEXT VALUE FOR seq_id");
```

## Troubleshooting

### Cannot connect to .\SQLEXPRESS  Cannot open user default database. Login failed.

I've deleted my database and suddenly was unable to login as the deleted one was the default database for the login. I panicked for a while, but it was easy to fix, actually. Click on Options and change the database from Default to master

![](.gitbook/assets/image%20%287%29.png)

### Cannot open database "Database" requested by the login. The login failed. Login failed for user 'Company\MachineName$'.

Add NT AUTHORITY\NETWORK SERVICE user to the database

