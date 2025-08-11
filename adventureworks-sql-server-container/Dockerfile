FROM mcr.microsoft.com/mssql/server:2022-latest
ARG MSSQL_SA_PASSWORD
ENV MSSQL_SA_PASSWORD=${MSSQL_SA_PASSWORD}
EXPOSE 1433
WORKDIR /
ADD https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak ./adventureworks.bak
USER root
RUN chmod 077 ./adventureworks.bak
USER mssql
COPY ./install.sql .
COPY ./install.sh .
COPY ./startup.sh .
CMD /bin/bash ./startup.sh