This repo uses Jenkins for CI/CD.

YouTube : https://www.youtube.com/watch?v=Kr70uvMKsyk&t=326s

Full article : https://medium.com/sfu-cspmp/machine-learning-deployment-a-storm-in-a-teacup-10541ec3b0d6

Software :

Docker : https://docs.docker.com/desktop/windows/install/

Jenkins : https://updates.jenkins-ci.org/download/war/

ngrok : https://ngrok.com/download

How to install Jenkins : https://www.blazemeter.com/blog/how-to-install-jenkins-with-a-war-file

java -jar jenkins.war (let this terminal be open)

Install Java if you don’t have : https://www.java.com/download/ie_manual.jsp

Once Jenkins installed :

##### Create Job

1- Create new job, name it docker-jenkins-flask, select freestyle project, click ok.

2- Select Git as Source code management, add git clone url in repository url.

3-Go down to the build section, add execute shell, add below commands

docker build --pull -t deploy .
docker run -p 5000:5000 deploy

save it

Now our job is created.

##### Build it

Just click build now on the left pane.

you must be inside dashboard -> docker-jenkins-flask

If you build is failing, look at output console, and if the error command sh not found, do the below.

This happens because Jenkins is not aware about the shell path.
In Manage Jenkins -> Configure System -> Shell, set the shell path as

> C:\Windows\system32\cmd.exe

https://stackoverflow.com/questions/15135771/hudson-on-windows-error-java-io-ioexception-cannot-run-program-sh

##### It must build your docker image, for me its not working in windows

##### Now creating test.py

import requests

url = 'http://192.168.99.100:5000/api'
feature = [[5.8, 4.0, 1.2, 0.2]]
labels = {
0 : "setosa",
1 : "versicolor",
2 : "virginica"
}

r = requests.post(url, json={'feature':feature})
print(r.json())

When I ran this test.py, I am not getting the result.

This is because shell command step where we mentioned docker build docker run commands are skipped while building.

Installing cygwin

##### Not worked

If it is working, Whenever there is a push to Git, Jenkins creates a docker image.











