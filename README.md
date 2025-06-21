# Tomcat Installation

source => https://tomcat.apache.org/download-90.cgi

####  Installation of openJDK
## Amazon Linux
```
sudo dnf update -y
sudo yum install java-17-amazon-corretto-devel -y
``` 

####  Create Tomcat User
```
sudo groupadd tomcat
sudo useradd -g tomcat -d /opt/tomcat -s /bin/false tomcat
``` 
#### Download and Install Tomcat
```
cd /tmp
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.95/bin/apache-tomcat-9.0.95.tar.gz
sudo tar -xvf apache-tomcat-9.0.95.tar.gz -C /opt/tomcat --strip-components=1
```
#### Configure Permissions
```
sudo chown -R tomcat:tomcat /opt/tomcat
sudo chmod -R 755 /opt/tomcat
```

#### Start/Stop Manually
```
cd /opt/tomcat/bin
sudo chmod +x startup.sh
sudo chmod +x shutdown.sh
```

#### Create Service File
sudo vi /etc/systemd/system/tomcat.service
Add these Lines
```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/jre"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

#### Reload Systemd and enable tomcat
```
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl start tomcat


```



