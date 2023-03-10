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




1. what happens if you DELETE your source connector and CREATE it again - ( does it perform snapshot again ? or it only capture streaming changes ?


   I deleted the source connector( curl -X DELETE http://localhost:8083/connectors/inventory-connector) and I have updated one record and observed that the    record is not available in Kafka (seen in kafdrop and grafana dashboard)

   Again I created a source connector and inserted one record then I have observed that previous and newly added records are available and the event count    increased by 2.




2. What will happen if you restart your KAFKA - and make change in your DB -- does your source connector works fine ?

   I restarted the KAFKA container and inserted one record then i have observed that records are available in Kafka and event count increased by 1 (seen in    kafdrop and grafana dashboard)



3. What will happend if restart kafka connect conatiner  and make change in source -- does your source connector works fine ?

   I restarted KAFKA connect container and inserted one record then I have observerd that records are available in Kafka and event count increased by 1        (seen in kafdrop and grafana dashboard)
