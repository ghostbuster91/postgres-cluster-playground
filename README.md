# postgres-cluster-playground


1. start master node
`docker-compose up -d master-db`

2. create bootstrapping backup for slave-1
`docker-compose up backup`

3. create bootstrapping backup for slave-2
`docker-compose up backup2`

4. start slave nodes
`docker-compose up -d slave-db`
`docker-compose up -d slave-db2`

5. stop master node
`docker-compose stop master-db`

6. uncomment synchronous_standby_names in master-config (and change it if you want to)

7. bring back master node
`docker-compose start master-db`

8. verify that the replication works
a) login into the master node
`docker-compose exec master-db psql -U postgres`
b) check replication stats
`select * from pg_stat_replication`
c) create some table and fill it with data:
`create table example_data(content text not null);`
`insert into example_data(content) values('first entry');`
d) login into slave-db and verify that the data was replicated
`docker-compose exec slave-db psql -U postgres`
`select * from example_data`