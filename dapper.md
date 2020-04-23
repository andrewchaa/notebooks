# Dapper

## Performance

A hiddne performance issue

When a string is passed as a parameter, Dapper assumes the type of it as NVARCHAR even if the column in your DB is VARCHAR. If you have used the parameter in your WHERE clause, your DB have to compare a VARCHAR column with an NVARCHAR data. Seeing this, the DB will simply decide that it cannot use the index on the table. It will do a Scan and convert each row of the table to NVARCHAR for comparing \(from [https://medium.com/@jithilmt/sql-server-hidden-load-evil-performance-issue-with-dapper-465a08f922f6](https://medium.com/@jithilmt/sql-server-hidden-load-evil-performance-issue-with-dapper-465a08f922f6)\)



