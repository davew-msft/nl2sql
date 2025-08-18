_This folder contains the code necessary to run SQL Server 2019 in a docker container with a pre-installed AdventureWorks sample database._ 

* [Location of AdventureWorks backup file](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)


To use this:  

```bash
cd adventureworks-sql-server-container

docker pull mcr.microsoft.com/mssql/server:2019-latest

docker run -e 'ACCEPT_EULA=Y' \
   -e 'SA_PASSWORD=Password01!!' \
   -p 1433:1433 \
   -h DockerSQL \
   --name DockerSQL \
   -d \
   mcr.microsoft.com/mssql/server:2019-latest

# now we need to install AdventureWorks in the container
docker exec -it --user root DockerSQL "bash"
# this will open up a prompt _from within the container_. 

wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak 
chmod +rwx AdventureWorks2019.bak
exit
#you should be back to your machine's prompt
#connect again as standard user so we can restore the db
docker exec -it DockerSQL "bash"
/opt/mssql-tools18/bin/sqlcmd -S localhost -C -U sa -P 'Password01!!'
#you should now be at 1> which is the sqlcmd prompt
#paste the following into this prompt
RESTORE DATABASE [AdventureWorks] FROM DISK = '/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2019' TO '/var/opt/mssql/data/AdventureWorks.mdf', MOVE 'AdventureWorks2019_log' TO '/var/opt/mssql/data/AdventureWorks_log.ldf';
GO
# you should see "RESTORE DATABASE successfully processed"
quit
# you should be back to the mssql@DockerSQL prompt
exit
# you should be back to your machine's prompt

# having issues? try one of these
docker logs DockerSQL -f
docker exec -it --user root DockerSQL "bash"
/opt/mssql-tools18/bin/sqlcmd
/opt/mssql-tools18/bin/sqlcmd -S localhost -C -U sa -P 'Password01!!'

# cleanup, when done using this for the workshop
docker stop DockerSQL
docker rm DockerSQL

```

## How do we know everything is working?  

1. install the `mssql extension for vscode`.  You should get a little icon like this:


![alt text](../img/image.png)

2. click the `+` for add connection.  
3. Choose `Load from Connection String`.  This should be the connection string you use:

`Data Source=127.0.0.1,1433;Initial Catalog=AdventureWorks;User ID=sa;Password=Password01!!;Pooling=False;Connect Timeout=30;Encrypt=False;Trust Server Certificate=True;Authentication=SqlPassword;Application Name=vscode-mssql;Application Intent=ReadWrite;Command Timeout=30`

You can now issue test queries, etc