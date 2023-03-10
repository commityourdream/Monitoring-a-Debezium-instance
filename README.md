# Monitoring-a-Debezium-instance

export DEBEZIUM_VERSION=2.0
docker-compose up --build

# Initialize database and insert test data
cat inventory.sql | docker exec -i monitoring_sqlserver_1 bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD'

# Start SQL Server connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver.json

## Modify records in the database via SQL Server client (do not forget to add `GO` command to execute the statement)
docker-compose exec sqlserver bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD -d testDB'



Open a web browser and got to the Grafana UI at http://localhost:3000. Login into the console as user admin with password admin. When asked either change the password (you also can skip this step). Click on Home icon and select Debezium dashboard. You should see a dashboard similar to the one on the screenshot below.
