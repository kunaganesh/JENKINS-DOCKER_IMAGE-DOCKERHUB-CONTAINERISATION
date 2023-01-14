
# JENKINS => DOCKER IMAGE => DOCKER HUB => DEPLOYING ON DOCKER

## Step1:
#### => Install the Three Plugins as shown below :

![Screenshot (1) (1)](https://user-images.githubusercontent.com/98937778/212473433-edf8c2fe-2a41-42d7-a924-9e00e44ecc0d.png)

![Screenshot (2)](https://user-images.githubusercontent.com/98937778/212473508-26121eda-0f5f-4dd6-9db0-29ba560e76e6.png)

#### => Next Write a Dockerfile and push to your Git repository Where your are code is :

![Screenshot (3)](https://user-images.githubusercontent.com/98937778/212473933-2fc2e0ca-1cc8-4d96-b10d-251bd1d056ea.png)

#### => Now your Dockerfile looks like this :
```bash
 FROM tomcat:8-jre11

 LABEL "Author"="Ganesh"

 WORKDIR /usr/local/tomcat/webapps

 RUN rm -rf /usr/local/tomcat/webapps/*

 ADD target/hello-1.0.war /usr/local/tomcat/webapps/ROOT.war  // Make sure to give correct artifact jenkins which give.

 EXPOSE 8080

 CMD ["catalina.sh", "run"]

 VOLUME /usr/local/tomcat/webapps

```
#### => Next Install the Docker in Jenkins shell to build our image
#### => Now run the bellow Command on jenkins shell :
```bash
 sudo chown -R jenkins:jenkins /var/run/docker.sock
```
#### => Next add the Docker server with username and passwd in jenkins ssh servers. (To run our containers automatically)
#### => Next create a job in jenkins and fill the details one by one as below :

#### => Select the scm as GIT and paste repo URL :

![Screenshot (4)](https://user-images.githubusercontent.com/98937778/212474750-f3ba8cb0-b40d-4560-9942-dd9ec23f7df4.png)

#### => Next select the Github hook in Build triggers section :

![Screenshot (5)](https://user-images.githubusercontent.com/98937778/212474819-9d4073cc-8048-4e12-8569-e423a71e0440.png)

#### => Next select the Maven option i build section because it is a java project :

![Screenshot (6)](https://user-images.githubusercontent.com/98937778/212474976-b25f9b4a-4f0a-4005-91be-a0e4e588da22.png)

#### Next select the Maven which we added in path and select the goals as install :

![Screenshot (7)](https://user-images.githubusercontent.com/98937778/212475498-ad355081-c892-48fa-99fd-fe4b4c45ec49.png)

#### => Next again click on Add build step and select the option called 'Docker Build and Publish' :

![Screenshot (8)](https://user-images.githubusercontent.com/98937778/212475691-04fb27d8-da7d-469d-bf6b-56333f7f832d.png)

#### => Next give the Repository name as docker username/imagename to build image :

![Screenshot (9)](https://user-images.githubusercontent.com/98937778/212475979-30d73190-720f-4a54-8074-bfe08924b5fc.png)

#### => Next Scroll down and select the add credentials in Registy Credentials to add dockerhub credentials :

![Screenshot (10)](https://user-images.githubusercontent.com/98937778/212476152-402744d0-824a-4771-89ef-5942ce12d96e.png)

#### => Give the username and passwd of your dockerhub.
#### => After adding that credentials save that and select your credentials in the dropdown :

![Screenshot (11)](https://user-images.githubusercontent.com/98937778/212476318-d8577511-43eb-4efd-8cc8-21d36463b7de.png)

#### => Next scroll down and click on Add post build actions and select the option called Send build artifacts over ssh :

![Screenshot (12)](https://user-images.githubusercontent.com/98937778/212476464-c66119e2-8035-480b-80e5-1c21dbe0a87a.png)

#### => Next select the server as Docke_host which we added in ssh servers previously :

![Screenshot (13)](https://user-images.githubusercontent.com/98937778/212476577-bcb3cf02-6755-407b-be7d-033a0f576f1b.png)

#### => Next select the remote dir as /home/ubuntu and paste the commands in shell :

![Screenshot (14)](https://user-images.githubusercontent.com/98937778/212476715-c4bd9bd1-9d0f-4a5f-9f65-0c0ed15d0171.png)

#### => Click on apply and save.
#### => Now configure the jenkins url in github webhooks.
#### => Now run a build .
## => Now Jenkins will automatically fetch the code and build it after based on dockerfile it will build an image and running as a container in docker server .

