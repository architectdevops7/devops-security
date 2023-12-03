# SonarQube is an open-source tool for continuous code quality that measure and analyze the source code.

### Prerequisites:
SonarQube requires atleast 2 GB of Ram.

Install Java.
Install and configure mysql.
Create db and user for SonarQube.
Install and configure SonarQube.

### Step 1:- Install Java
yum install java-1.8*

### Step 2:- Install and configure mysql

### Manually Import GPG Key:
You can manually download the GPG key and import it. Here's an example using the RPM-GPG-KEY-mysql key:
```
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql
```

### Use a Different Mirror:
```
sudo vi /etc/yum.repos.d/mysql-community-source.repo
```
```
[mysql-connectors-community-source]
name=MySQL Connectors Community - Source
baseurl=http://repo.mysql.com/yum/mysql-connectors-community/el/7/SRPMS
enabled=0
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-tools-community-source]
name=MySQL Tools Community - Source
baseurl=http://repo.mysql.com/yum/mysql-tools-community/el/7/SRPMS
enabled=0
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql55-community-source]
name=MySQL 5.5 Community Server - Source
baseurl=http://repo.mysql.com/yum/mysql-5.5-community/el/7/SRPMS
enabled=0
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql56-community]
name=MySQL 5.6 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql


[mysql57-community-dmr-source]
name=MySQL 5.7 Community Server Development Milestone Release - Source
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/SRPMS
enabled=0
gpgcheck=1
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
```
yum update
```

### Install mysql server
```
sudo yum install mysql-community-server
sudo systemctl start mysqld
sudo systemctl status mysqld
```

### Configure mysql db by running mysql_secure_installation
```
mysql_secure_installation

mysql -u root -p
```

### Create db and user for SonarQube
```
CREATE DATABASE sonarqube_db;
CREATE USER 'sonarqube_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON sonarqube_db.* TO 'sonarqube_user'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
exit
```

### Step 3:- Install and configure SonarQube

Create a new user and set password for SonarQube
```
sudo useradd sonarqube
sudo passwd sonarqube
```

Download the latest version of SonarQube from the 
URL: https://www.sonarqube.org/downloads/
```
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip
sudo unzip sonarqube-6.7.7.zip
sudo mv sonarqube-6.7.7 sonarqube
```

Change the owner of the sonarqube directory
```
sudo chown -R sonarqube:sonarqube sonarqube
```

Open the sonarqube configuration file for changes
```
sudo vi /opt/sonarqube/conf/sonar.properties
```

Enter the database details below
```
sonar.jdbc.username=sonarqube_user
sonar.jdbc.password=password
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonarqube_db?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
```
RUN_AS_USER=sonarqube

Create a sonar.service file in system
sudo vi /etc/systemd/system/sonar.service
Add the below script
```
[Unit]
Description=SonarQube service
After=syslog.target network.target
[Service]
Type=forking
ExecStart=/u01/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/u01/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonarqube
Group=sonarqube
Restart=always
[Install]
WantedBy=multi-user.target
```

Now start and check the status of the sonar service
```
sudo systemctl start sonar
sudo systemctl status sonar
sudo systemctl enable sonar
```

check the sonar using URL http://ip_address:9000

### Help Guide
https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner-for-maven/








