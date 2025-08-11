_This folder contains the code necessary to run SQL Server 2019 in a docker container with a pre-installed AdventureWorks sample database._ 

* [Location of AdventureWorks backup file](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)


To use this:  

```bash
cd adventureworks-sql-server-container

# it will throw warnings b/c I'm lazy.  Sorry.  
docker build -t sql-server-nl2sql:latest . \
    -e "MSSQL_SA_PASSWORD=Pawws0rd01!!"

docker run -d --rm \
    --name sql-server-nl2sql \
    -e "ACCEPT_EULA=Y" \
    -e "MSSQL_SA_PASSWORD=Pawws0rd01!!" \
    -p 5533:1433 sql-server-nl2sql:latest 

# having issues? try one of these
docker logs sql-server-nl2sql -f
docker exec -it sql-server-nl2sql /bin/bash
/opt/mssql-tools18/bin/sqlcmd
/opt/mssql-tools18/bin/sqlcmd -S localhost -C -U sa -P "Pawws0rd01!!" 

# how do we know this is working?  


# cleanup, when done using this
docker stop sql-server-nl2sql
docker rm sql-server-nl2sql
docker image rm sql-server-nl2sql
```