
*step 1:* create a t2 large instance
*step 2:* Install Jenkins,Docker,Trivy,sonarqube
*step 3:* Install plugins JDK,sonarqube scanner,Maven,OWSAP Dependency Check
*step 4:* Create declarative jenkins pipeline
-------------------------------------------------------------------------
# Install Jenkins
```
sudo apt-get install openjdk-11-jdk -y
java -version

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

sudo systemctl start jenkins.service
sudo systemctl enable jenkins.service
sudo systemctl status jenkins.service
cat /var/lib/jenkins/secrets/initialAdminPassword
```

-------------------------------------------------------------------------
# Install Docker
```
sudo apt-get install docker.io
sudo usermod -aG docker $USER
newgrp docker
docker ps
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
docker run -d --name sonar -p 9000:9000 -v sonarqube_data:/opt/sonarqube/data sonarqube:lts-community
```
----------------------------------------------------------------------
# Install Email plugins in Jenkins
```
search for the email in the plugins 
- select the Email Extension Template install it
- inside the google account management
- set the 2 step verification
at the Google management account. 
Open two step verification. 
Then after Search for app password. 
Give the app name and click on.  



- open settings
- check for E-mail Notification
- SMTP server: smtp.gmail.com
- click on advanced
    - Use SMTP Authentication: checked
    - User Name: <your gmail id>
    - Password: <app password>
    - Use SSL: checked
    - SMTP Port: 465
    - Reply-To Address: <your gmail id>
- click on Test configuration
   - Test e-mail recipient: <your gmail id>
    - click on Test configuration
    - save it
    - email will be sent check the mail 

- Extended E-mail Notification
    - smtp server: smtp.gmail.com
    - SMTP Port: 465
- save it
- click on advanced
    - Credentials:
        - Kind: Username with password
        - Username: <your gmail id>
        - Password: <app password>
- Use SSL: checked
- Default Content Type: HTML
- below the Additional groovy classpath
- Default Trigger: 
        Always : checked
        Failure Always : checked
- Check once :
    - Test configuration by sending test e-mail
    - enter the email id


```
-------------------------------------------------------------------------
# Plugins to install 

```- Eclipse Temurin installerVersion
- SonarQube ScannerVersion
- OWASP Dependency-Check
- OWASP Dependency-TrackVersion
```

-------------------------------------------------------------------------
# Check the sonarqube state
```
docker ps
login to the sonaqube 
http://<ip>:9000
- click on administartion
- security
- users
- under the token click on the bars


- click on the administration
- configuration
- webhooks
- create webhooks
- name: jenkins
- url: http://<private_ip>:8080/sonarqube-webhook/


```
-------------------------------------------------------------------------
# Setup jdk,maven,sonarqube on jenkins
```
- Tools
    - JDK installations: Add JDK
            - Name: jdk17
            - Install automatically: checked
            - Version: JDK 17
            - install from adoptium.net(becoz of this Eclipse Temurin installerVersion plugin we get this option)
    - Maven Installations:
        - name: maven3
        - install automatically: checked
        - version: 3.6.3
    - Dependency-Check installations:
        - install from github
        - version: 6.5.1
    - Sonarscanner:
        - Add sonarqube scanner
        - name: sonar-scanner
        - install automatically: checked
        - select latest version
        
```
-------------------------------------------------------------------------
# Adding sonarqube credentials
```
- get the token from the sonarqube
- in the credentials click on the global credentials
- create credentials
- kind: secret text
- scope: global
- provide the name,some description, token and click on ok
```
- SonarQube servers:
    - name: sonar-server
    - Server URL: http://<private_ip>:9000
    - Server authentication token: <token>