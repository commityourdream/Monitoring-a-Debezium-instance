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


![Screenshot 2023-03-10 at 4 51 38 PM](https://user-images.githubusercontent.com/42512407/224322168-1a8db3df-b709-4a8f-99a6-3e8e201a1ee3.jpg)
![Screenshot 2023-03-10 at 4 47 15 PM](https://user-images.githubusercontent.com/42512407/224322178-edae9dd3-e86d-4ad1-951a-bf5e76c5b81a.jpg)
![Screenshot 2023-03-10 at 4 41 43 PM](https://user-images.githubusercontent.com/42512407/224322183-b2a90b50-ce91-4bd2-b98e-4ba30b9dc7ca.jpg)
![Before-Deleting-Source Connector ](https://user-images.githubusercontent.com/42512407/224322186-bfe50ff3-8ebf-4f81-8aa2-db5d9a06e8cf.jpg)
