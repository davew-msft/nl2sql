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
docker exec -it sql-server-nl2sql /bin/bash
# this will open up a prompt _from within the container_.  



# having issues? try one of these
docker logs sql-server-nl2sql -f
docker exec -it sql-server-nl2sql /bin/bash
/opt/mssql-tools18/bin/sqlcmd
/opt/mssql-tools18/bin/sqlcmd -S localhost -C -U sa -P "Pawws0rd01!!" 

# how do we know this is working?  


# cleanup, when done using this
docker stop DockerSQL
docker rm DockerSQL

```