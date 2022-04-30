# openmrs
....................
create ubuntu ec2 instance 
in ubuntu install java, tomcat, mysql servers
sudo apt update -y
install java on ubuntu
sudo apt-get install -y openjdk-8-jdk
echo "JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64"
source /etc/environment
now install tomcat with following command
sudo apt-get install -y tomcat9
sudo apt-get install -y tomcat9-admin
sudo mkdir /var/lib/OpenMRS/
sudo chown -R tomcat /var/lib/OpenMRS/
sudo mkdir /usr/share/tomcat9/.OpenMRS
sudo chown -R tomcat /usr/share/tomcat9/.OpenMRS/
sudo mkdir /etc/systemd/system/tomcat9.service.d
now install mysql server
sudo apt-get install -y mysql-server
sudo apt-get install -y unzip
...........................................
Configure Tomcat and MySQL
Update the tomcat-use.xml file by removing all existing entries and add new roles, update the password (replace *** with password of your choice) and role assignment for the admin user as shown in the following screenshot
........sudo vi /etc/tomcat9/tomcat-users.xml........
......<!--
  <role rolename="tomcat"/>
  <role rolename="admin"/>
  <role rolename="admin-gui"/>
  <role rolename="manager"/>
  <role rolename="manager-gui"/>
  <user username="admin" 
  password="<MNMurthy@123>" 
  roles="tomcat,admin,admin-gui,manager,manager-gui"/>
-->
</tomcat-users>
now modify the tomcat9.service.d 
sudo vi /etc/systemd/system/tomcat9.service.d/logging-allow.conf
........[service]
        ReadWritePaths=var/lib/openmrs
        readwritepaths=var/lib/tomcat9
Restart Tomcat service.
sudo systemctl daemon-reload        
sudo systemctl restart tomcat9
Update MySQL root user password (replace *** with a password of your choice) and permission.
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY ‘***’;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
Exit;
now Deploy and Configure/Install OpenMRS Core 2.4.0 for Enterprise.
To deploy OpenMRS Core 2.4.0, run following commands from the AWS CLI:
      wget https://sourceforge.net/projects/openmrs/files/releases/OpenMRS_Platform_2.4.0/openmrs.war
    	sudo unzip openmrs.war -d /var/lib/tomcat9/webapps/openmrs
	    sudo chown -R tomcat /var/lib/tomcat9/webapps/openmrs/
      To start OpenMRS configuration, open a web browser and log into the following address (replace localhost with IPv4 Public IP of the Amazon EC2 instance):

http://localhost:8080/openmrs
