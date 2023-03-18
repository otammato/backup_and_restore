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

<img width="673" alt="Screenshot 2023-03-18 at 18 44 23" src="https://user-images.githubusercontent.com/104728608/226130114-f49dfe3d-45a9-46d1-83d1-43369554255f.png">

<img width="653" alt="Screenshot 2023-03-18 at 18 45 07" src="https://user-images.githubusercontent.com/104728608/226130111-f6bdb408-04c7-402c-b0e5-8740764bc0c7.png">
<img width="680" alt="Screenshot 2023-03-18 at 18 43 42" src="https://user-images.githubusercontent.com/104728608/226130112-a1a24c95-32cf-4242-ad7e-3f8539c5ca87.png">

<img width="734" alt="Screenshot 2023-03-18 at 18 43 56" src="https://user-images.githubusercontent.com/104728608/226130107-f52b49d9-c549-4a6b-9138-ab576258acea.png">
<img width="674" alt="Screenshot 2023-03-18 at 18 44 44" src="https://user-images.githubusercontent.com/104728608/226130110-54f1b164-1e46-4311-88e9-22cd647f4713.png">

<img width="668" alt="Screenshot 2023-03-18 at 18 43 26" src="https://user-images.githubusercontent.com/104728608/226130096-435f5e99-d0e9-4660-bd45-e0aa2841218e.png">




