# Apache Cassandra
 This page introduces you how to install Apache Cassandra 
 to do big data tasks e.g., data scientist and data engineering tasks.
## Apache Cassandra Installation Locally
If you want to install Ubuntu using Vagrant use this command: <br>
`vagrant init ubuntu/trusty64; vagrant up --provider virtualbox`

### Install Java
- `sudo apt-add-repository ppa:webupd8team/java`
- `sudo apt-get update`
- `sudo apt-get install oracle-java8-installer`

## Install Cassandra
- Download Cassandra zip `wget http://www.eu.apache.org/dist/cassandra/2.2.1/apache-cassandra-2.2.1-bin.tar.gz`
- Goto root and make the folder called "cassandra"  `cd `
- Create the folder `mkdir cassandra `
- Get into the folder `cd cassdandra`
- Move the zip file to the current folder `mv ~/apache-cassandra-2.2.1-bin.tar.gz .`
- Unzip `tar -xvzf apache-cassandra-2.2.1-bin.tar.gz`
- Delete the tarball `rm apache-cassandra-2.2.1-bin.tar.gz`
- `cd apache-cassandra-2.2.1`
- Modify the config file /conf/cassandra.yml  `vim conf/cassandra.yaml ` for your purpose
- Example: 
- `data_file_directories:` <br>
    `- /var/lib/cassandra`
- `commitlog_directory: /var/log/cassandra`
  
### Setup the folder for Cassandra
- Folder, where data is written to: `sudo mkdir /var/lib/cassandra`
- Folder, where log is written to:  `sudo mkdir /var/log/cassandra`
- Setup permission for the folder lib: `sudo chown -R $USER:$GROUP /var/lib/cassandra`
- Setup permission for the folder log: `sudo chown -R $USER:$GROUP /var/log/cassandra`

### Start / Stop Cassandra
- Run Cassandra in foreground mode using hypen   `bin/cassandra -f `
- Stop Cassandra using `ctrl + c `

### Check status of Cassandra
 - `bin/nodetool status`
 - `bin/nodetool info`
 - `bin/nodetool ring`
 
### Cassandra CQL
CQL is an option SQL like in Cassandra, you can also use Datastax API or astyanax <br>
CQL is similar to SQL  without join functionalities. To use join funtionlatities, 
we can use Apache Spark QL. 
 - Run cql  `bin/cqlsh`
 - Type e.g.,  `DESCRIBE CLUSTER;` to  see the cluster information
 - Type `DESCRIBE KEYSPACES;`to see all the keyspaces
 - Type `DESCRIBE KEYSPACE system;`, where `system` is the keyspace name
 - Type `exit` to exit the cql console

## Cassandra Keyspaces (Databases)
The name of keyspace is case-sensitive and in general, we specify the keyspaces in lower case, however if required to have in upper case, you need to put the keypaces name in the qutation e.g., "MyKeySpace".
#### Cassandra Create Keyspaces (For more than one node)
 - Type the following to allow a keyspace to span across more than one data center (dc) with the number of its replication factor:
 - `CREATE KEYSPACE user`
   `WITH REPLICATION = { 'class':'NetworkTopologyStrategy','dc1':3,'dc2':2};`

#### Cassandra Create Keyspaces (For one node)
 - Type the following  if you have only one node:
 - `CREATE KEYSPACE user`
   `WITH REPLICATION = { 'class':'SimpleStrategy','replication_factor': 1};`

#### Drop Keyspaces
 - `DROP KEYSPACE user`
 
## Cassandra Create  Tables 
 - `USE user` to use the keyspace `user`
 - `CREATE TABLE  userTable(`<br>
`user_id text,`<br>
`datetime timestamp,`<br>
`information text,`<br>
`PRIMARY KEY (user_id, datetime)`  (known as compound primary key) <br>
`) WITH CLUSTERING ORDER BY (datetime DESC);`<br>
,where `user_id`is the partition key (the first column). 
Please note: Writing in decending order take longer time, but it improves read performance when decending order is required by applications.
 - Create composite partition key  using this command:
  - `CREATE TABLE  userTable(`<br>
`user_id text,`<br>
`date text,`<br>
`time timestamp,`<br>
`information text,`<br>
`PRIMARY KEY ((user_id, date), time)`  (known as composite partition key) <br>
`) WITH CLUSTERING ORDER BY (time DESC);`<br>


## Cassandra  Write Data
 - `INSERT INTO`command
 - `COPY`command
 - sstableloader tool for bulk loader

### INSERT INTO command
 - `INSERT INTO usertable (user_id,datetime,information) VALUES ( 'ID1234', '2015-08-21 07:20:12','Information 1');`
 - Insert with TTL 30 seconds: `INSERT INTO usertable (user_id,datetime,information) VALUES ( 'ID1234', '2015-08-21 07:20:12','Information 2') USING TTL 30 ;`. 
 Here, you can specify the TTL to seconds, hours, or days.
 - `SELECT * FROM usertable ;``
 - `SELECT * FROM usertable where  information = 'Information 1';` This will not work, because it's not indexed by default
 
### Secondary Indexes
The secondary indexes in Cassandra does not increase the performance of the query like in relational databases. It privdes us to be able to use them in the `WHERE `clause.
 - CREATE INDEX information_index ON usertable (information);
 - Now you can query: `select * from usertable  where user_id = 'ID1234' and  information = 'Information 1';`



### COPYcommand
 - `COPY usertable(user_id,datetime,information)`<br>
`FROM '/home/log/data.csv' `<br>
`WITH header = true AND delimiter = '|'; `

