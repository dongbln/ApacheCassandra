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
- `data_file_directories:`
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
