# backup_and_restore

[ AWS Backup, AWS RDS, AWS EC2, MySQL ] Backup and restore of MySQL database

<br>

## 1. Simple backup and restore with mysqldump

### 1.1. Connect to a database

```
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
```
<img width="600" alt="Screenshot 2023-03-19 at 10 48 00" src="https://user-images.githubusercontent.com/104728608/226170408-0dd573a5-d621-4a3a-9427-36cfdaaac293.png">

<br><br>
### 1.2. Create a sample table

```
CREATE TABLE mytable (
    number TINYINT(1),
    name VARCHAR(255),
    surname VARCHAR(255)
);

INSERT INTO mytable (number, name, surname)
VALUES 
(0, 'John', 'Doe'),
(1, 'Jane', 'Smith'),
(0, 'Bob', 'Johnson'),
(1, 'Mary', 'Brown'),
(0, 'Tom', 'Wilson'),
(1, 'Emily', 'Davis'),
(0, 'Mike', 'Jones'),
(1, 'Sarah', 'Clark'),
(0, 'Chris', 'Lee'),
(1, 'Kelly', 'Taylor');
```

<img width="600" alt="Screenshot 2023-03-19 at 10 52 06" src="https://user-images.githubusercontent.com/104728608/226170561-b26eea6f-7513-4e99-9ee3-2d97d89f9fd9.png">

<br><br>

### 1.3. Create a backup file with mysqldump command

```
mysqldump -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
```

<img width="1090" alt="Screenshot 2023-03-18 at 21 32 03" src="https://user-images.githubusercontent.com/104728608/226141222-1e8b63d1-e04d-4a00-ac8a-1f0c37a1be62.png">

<br><br>

### 1.4. Fallover<br> 
Let's assume that entries with "0" were deleted or table dropped by mistake

```
DELETE FROM mytable WHERE number=0;
```
<img width="719" alt="Screenshot 2023-03-18 at 21 27 01" src="https://user-images.githubusercontent.com/104728608/226141016-1c5923a4-a4be-4739-8b08-47feec5ad507.png">

<br><br>

### 1.5. Restore<br>
Restore the database from the backup file

```
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
```
<img width="991" alt="Screenshot 2023-03-18 at 21 27 56" src="https://user-images.githubusercontent.com/104728608/226141044-9ff29ad3-dec8-409e-aedc-6db5eeb0584b.png">

<br><br>
### 1.6. Result
<img width="978" alt="Screenshot 2023-03-18 at 21 29 22" src="https://user-images.githubusercontent.com/104728608/226141118-d7e43415-5b35-4b7d-9892-6add7b36bfc2.png">

<br><br><br>

---

<br><br><br>
## 2. Production-wise cloud-based backup and restore with AWS Backup

### 2.1. Create an EC2 instance
<img width="700" alt="Screenshot 2023-03-18 at 18 55 46" src="https://user-images.githubusercontent.com/104728608/226130357-c209ea24-b8f4-4906-ac48-b97c92e6658b.png">

### 2.2. Create an RDS instance
<img width="700" alt="Screenshot 2023-03-18 at 18 54 30" src="https://user-images.githubusercontent.com/104728608/226130291-c479407a-4e05-40c5-bee6-dbd89ee5c16e.png">

### 2.3. Adjust the inbound and outbound rules to provide bi-directional acess for two instances

<img width="700" alt="Screenshot 2023-03-18 at 19 02 24" src="https://user-images.githubusercontent.com/104728608/226130726-ceb1f3ac-5e5b-472a-b321-111621ff5175.png">

### 2.4. SSH to an EC2 instance.

```
ssh -i "test_delete.pem" ec2-user@ec2-34-202-234-54.compute-1.amazonaws.com
```

### 2.5. Install MariaDB on EC2 (AWS recommends MariaDB which supports both MySQL and Postgres)

```
#  this command updates all packages to the latest version
sudo yum update -y 

# this command installs MySQL server on your machine, it also creates a systemd service
sudo yum install -y mariadb-server

# this command enables the service created in previous step
sudo systemctl enable mariadb

# this command starts the MySQL server service on your Linux instance
sudo systemctl start mariadb
```
<img width="891" alt="Screenshot 2023-03-18 at 19 16 43" src="https://user-images.githubusercontent.com/104728608/226131747-bae99df2-ce40-4cb1-8420-fd9b1b32be29.png">

### 2.5. Connect to the RDS instance

```
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
```
<img width="910" alt="Screenshot 2023-03-18 at 19 06 54" src="https://user-images.githubusercontent.com/104728608/226131194-63e309e4-9323-4ef8-84d2-94ed8508c5f8.png">

<img width="810" alt="Screenshot 2023-03-18 at 19 29 03" src="https://user-images.githubusercontent.com/104728608/226133184-85f44852-bc7b-45bf-bd05-2cb1cb1d76bb.png">

### 2.6. Create a database and switch to it
```
CREATE DATABASE test_database;

USE test_database;
```
<img width="829" alt="Screenshot 2023-03-18 at 19 32 41" src="https://user-images.githubusercontent.com/104728608/226133577-164aa199-5bd1-4c8f-b687-e604a6406622.png">

### 2.7. Create the table and insert the values

```
CREATE TABLE mytable (
    number TINYINT(1),
    name VARCHAR(255),
    surname VARCHAR(255)
);

INSERT INTO mytable (number, name, surname)
VALUES 
(0, 'John', 'Doe'),
(1, 'Jane', 'Smith'),
(0, 'Bob', 'Johnson'),
(1, 'Mary', 'Brown'),
(0, 'Tom', 'Wilson'),
(1, 'Emily', 'Davis'),
(0, 'Mike', 'Jones'),
(1, 'Sarah', 'Clark'),
(0, 'Chris', 'Lee'),
(1, 'Kelly', 'Taylor');
![image](https://user-images.githubusercontent.com/104728608/226133748-cf66866d-ac07-4323-a1f6-035d4ded892f.png)
```
### 2.6. Check the table

```
SELECT * FROM mytable;
```
<img width="648" alt="Screenshot 2023-03-18 at 19 39 02" src="https://user-images.githubusercontent.com/104728608/226133818-d640bb59-8741-41b2-a5b9-e01cd89b6837.png">

<br><br>

### 2.6. AWS Backup

<br><br>
AWS Backup Vault, AWS Backup Plan, and AWS Protected Resources are all AWS services related to data protection and recovery, but they have different functionalities.
<br><br>
AWS Backup Vault is a logical container that allows you to store and manage backups of your AWS resources, such as EC2 instances, RDS databases, and EBS volumes. It provides centralized backup management, retention policies, and access control for multiple resources.
<br><br>
AWS Backup Plan is a service that allows you to create and manage backup policies for your AWS resources. It enables you to define backup schedules, retention periods, and backup destinations, including AWS Backup Vaults. AWS Backup Plan also supports cross-region and cross-account backup and provides a centralized dashboard for backup monitoring and management.
<br><br>
AWS Protected Resources are a set of AWS resources that have additional security controls applied to them to protect against accidental or malicious deletion, modification, or encryption. These controls include write protection, deletion protection, and versioning. AWS Protected Resources are available for Amazon S3 buckets and DynamoDB tables.
<br><br>

### 2.7. This time we create an on-demand backup using AWS Protected Resources, but a backup can be launched on a regular basis using AWS Backup Plan

<img width="700" alt="Screenshot 2023-03-18 at 19 47 30" src="https://user-images.githubusercontent.com/104728608/226135255-8f5e4232-9574-4e9a-afb7-155c6ba5fc36.png">

<img width="700" alt="Screenshot 2023-03-18 at 19 51 42" src="https://user-images.githubusercontent.com/104728608/226135261-7be01c6d-58b1-4ef5-9161-6d2c9fb56d60.png">

### 2.7. Make sure the backup job is completed and the backup image created

<img width="700" alt="Screenshot 2023-03-18 at 20 06 27" src="https://user-images.githubusercontent.com/104728608/226135932-8484b564-7e06-4c0b-8ede-862a10eef0ba.png">

<img width="700" alt="Screenshot 2023-03-18 at 21 06 47" src="https://user-images.githubusercontent.com/104728608/226140139-90bfe15f-52e7-4df7-b3f9-387a02384131.png">


### 2.8. Imitate unwanted changes to the database

<img width="680" alt="Screenshot 2023-03-18 at 20 16 17" src="https://user-images.githubusercontent.com/104728608/226136279-ce983fb7-ba57-4e01-91f4-4df8faf93387.png">

### 2.9. Launch a restore job from the backup vault

<img width="1156" alt="Screenshot 2023-03-18 at 20 20 33" src="https://user-images.githubusercontent.com/104728608/226136672-ac26bcc2-cc98-4fc3-a7d3-c308c3a460a0.png">

### 2.10. At this point you can change some parameters to a database instance

<img width="814" alt="Screenshot 2023-03-18 at 20 23 58" src="https://user-images.githubusercontent.com/104728608/226136763-0fc0f09a-4e34-43ca-b8a7-81be52134095.png">

<img width="1037" alt="Screenshot 2023-03-18 at 20 36 08" src="https://user-images.githubusercontent.com/104728608/226137699-e90e6c1d-89c4-4350-b108-66829cc1452a.png">

<img width="1139" alt="Screenshot 2023-03-18 at 20 34 12" src="https://user-images.githubusercontent.com/104728608/226137329-83f91da2-d227-44f7-a9e1-7e6b14aa7fa6.png">



<img width="1184" alt="Screenshot 2023-03-18 at 20 38 41" src="https://user-images.githubusercontent.com/104728608/226137941-fa06d93f-eb56-4050-8bde-d72ed56486f2.png">

<img width="1116" alt="Screenshot 2023-03-18 at 20 40 21" src="https://user-images.githubusercontent.com/104728608/226138139-9ab1f7a7-afd5-4074-a7a1-a96d099f5e10.png">

<img width="825" alt="Screenshot 2023-03-18 at 20 50 43" src="https://user-images.githubusercontent.com/104728608/226138944-68a28a8d-8aaa-4164-910e-11dbdbaf43df.png">



```
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
clear
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
clear
mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
mysqldump -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
ls vi backupfile.sql
mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p < backupfile.sql
 mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 clear
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 clear
 mysqldump -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 clear
 mysqldump -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 ls
 vi backupfile.sql
 rm backupfile.sql
 mysqldump -h restored-db-instance.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
 ls
 clear
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 clear
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p test_database < backupfile.sql
 mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
 clear
 mysqldump -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin test_database mytable -p > backupfile.sql
 ls
 history
 history | awk '{$1=""; print $0}'
```





