# backup_rds_with_AWS_backup

[ AWS Backup, AWS RDS, AWS EC2 ] Backup and recovery of an RDS MySQL instance using AWS Backup

<img width="1309" alt="Screenshot 2023-03-18 at 18 55 46" src="https://user-images.githubusercontent.com/104728608/226130357-c209ea24-b8f4-4906-ac48-b97c92e6658b.png">

<img width="1223" alt="Screenshot 2023-03-18 at 18 54 30" src="https://user-images.githubusercontent.com/104728608/226130291-c479407a-4e05-40c5-bee6-dbd89ee5c16e.png">

<img width="1150" alt="Screenshot 2023-03-18 at 19 02 24" src="https://user-images.githubusercontent.com/104728608/226130726-ceb1f3ac-5e5b-472a-b321-111621ff5175.png">

```
ssh -i "test_delete.pem" ec2-user@ec2-34-202-234-54.compute-1.amazonaws.com
```
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

<img width="910" alt="Screenshot 2023-03-18 at 19 06 54" src="https://user-images.githubusercontent.com/104728608/226131194-63e309e4-9323-4ef8-84d2-94ed8508c5f8.png">

```
mysql -h backup-tester-db.c9rglxpvlls0.us-east-1.rds.amazonaws.com -u admin -p
```
<img width="810" alt="Screenshot 2023-03-18 at 19 29 03" src="https://user-images.githubusercontent.com/104728608/226133184-85f44852-bc7b-45bf-bd05-2cb1cb1d76bb.png">

```
CREATE DATABASE test_database;

USE test_database;
```
<img width="829" alt="Screenshot 2023-03-18 at 19 32 41" src="https://user-images.githubusercontent.com/104728608/226133577-164aa199-5bd1-4c8f-b687-e604a6406622.png">

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
```
SELECT * FROM mytable;
```
<img width="648" alt="Screenshot 2023-03-18 at 19 39 02" src="https://user-images.githubusercontent.com/104728608/226133818-d640bb59-8741-41b2-a5b9-e01cd89b6837.png">
<br><br>
AWS Backup Vault, AWS Backup Plan, and AWS Protected Resources are all AWS services related to data protection and recovery, but they have different functionalities.
<br>
AWS Backup Vault is a logical container that allows you to store and manage backups of your AWS resources, such as EC2 instances, RDS databases, and EBS volumes. It provides centralized backup management, retention policies, and access control for multiple resources.
<br>
AWS Backup Plan is a service that allows you to create and manage backup policies for your AWS resources. It enables you to define backup schedules, retention periods, and backup destinations, including AWS Backup Vaults. AWS Backup Plan also supports cross-region and cross-account backup and provides a centralized dashboard for backup monitoring and management.
<br>
AWS Protected Resources are a set of AWS resources that have additional security controls applied to them to protect against accidental or malicious deletion, modification, or encryption. These controls include write protection, deletion protection, and versioning. AWS Protected Resources are available for Amazon S3 buckets and DynamoDB tables.
<br><br>

<img width="1151" alt="Screenshot 2023-03-18 at 19 47 30" src="https://user-images.githubusercontent.com/104728608/226135255-8f5e4232-9574-4e9a-afb7-155c6ba5fc36.png">

<img width="715" alt="Screenshot 2023-03-18 at 19 51 42" src="https://user-images.githubusercontent.com/104728608/226135261-7be01c6d-58b1-4ef5-9161-6d2c9fb56d60.png">





<img width="673" alt="Screenshot 2023-03-18 at 18 44 23" src="https://user-images.githubusercontent.com/104728608/226130114-f49dfe3d-45a9-46d1-83d1-43369554255f.png">

<img width="653" alt="Screenshot 2023-03-18 at 18 45 07" src="https://user-images.githubusercontent.com/104728608/226130111-f6bdb408-04c7-402c-b0e5-8740764bc0c7.png">
<img width="680" alt="Screenshot 2023-03-18 at 18 43 42" src="https://user-images.githubusercontent.com/104728608/226130112-a1a24c95-32cf-4242-ad7e-3f8539c5ca87.png">

<img width="734" alt="Screenshot 2023-03-18 at 18 43 56" src="https://user-images.githubusercontent.com/104728608/226130107-f52b49d9-c549-4a6b-9138-ab576258acea.png">
<img width="674" alt="Screenshot 2023-03-18 at 18 44 44" src="https://user-images.githubusercontent.com/104728608/226130110-54f1b164-1e46-4311-88e9-22cd647f4713.png">

<img width="668" alt="Screenshot 2023-03-18 at 18 43 26" src="https://user-images.githubusercontent.com/104728608/226130096-435f5e99-d0e9-4660-bd45-e0aa2841218e.png">




