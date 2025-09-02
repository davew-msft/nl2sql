# Build Your Own NL2SQL engine with LangChain Family of Tools

How?

* We'll try to keep this as simple as possible and not get lost in the weeds
* You will be able to ask a question and the NL2SQL engine will be able to figure out the schema of the db and generate the SQL
* The SQL will be executed by _the agent_.  For that, we'll use LangChain

We'll try to do all of this in Jupyter notebooks.  Hopefully you can follow along:  

We'll load up and use a subset of the titanic dataset from Kaggle.  

You'll need to install:
* vscode
* jupyter notebook extension
* open the [nl2sql jupyter notebook](./nl2sql.ipynb) and follow along

## If you need a sql server for testing ...

...and you are ok with Docker...

_you may need to adjust these commands if you do NOT use WSL_

```bash

docker pull mcr.microsoft.com/mssql/server:2019-latest

# basic syntax
docker run -e 'ACCEPT_EULA=Y' \
   -e 'SA_PASSWORD=Password01!!' \
   -p 1433:1433 \
   -h DockerSQL \
   --name DockerSQL \
   -d \
   mcr.microsoft.com/mssql/server:2019-latest

docker ps -a

```