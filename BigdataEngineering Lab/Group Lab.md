
## CM Install Lab

### System Configuration Checks

#### 1. Check vm.swappiness on all your nodes

#### 2. Show the mount attributes of your volume(s)

#### 3. If you have ext-based volumes, list the reserve space setting

#### 4. Disable transparent hugepage support

#### 5. List your network interface configuration

#### 6. Show that forward and reverse host lookups are correctly resolved

#### 7. Show the nscd service is running

####8. Show the ntpd service is running

## Cloudera Manager Install Lab

### Path B install using CM 5.15.x

#### • Install a supported Oracle JDK on your first node

#### • Install a supported JDBC connector on all nodes

#### • Create the databases and access grants you will need

####• Configure Cloudera Manager to connect to the database

####• Start your Cloudera Manager server -- debug as necessary

#### • Do not continue until you can browse your CM instance at port 7180

## MySQL/MariaDB Installation Lab

### Configure MySQL with a replica server

#### • You can use the steps documented here for MariaDB or here for MySQL.

#### • The steps below are MySQL-specific. (RHEL/CentOS 7.x -> use MariaDB.)

### MySQL installation - Plan Two Detail

#### 1. Download and implement the official MySQL repo

#### 2. You should not need to build a /etc/my.cnf file to start your MySQL server

#### 3. Start the mysqld service.

#### 4. Use /usr/bin/mysql_secure_installation to:

## Cloudera Manager Install Lab

### Install a cluster and deploy CDH

#### • Do not use Single User Mode. Do not. Don't do it.

#### • Ignore any steps in the CM wizard that are marked (Optional)

#### • Install the Data Hub Edition

#### • Install CDH using parcels

#### • Deploy only the Core set of CDH services.

#### • Deploy three ZooKeeper instances.
o CM does not tell you to do this but complains if you don't
