## 使用docker安装了带有maven的jenkins容器，但是maven不能使用？

确定看到了maven的安装包，也配置了环境变量，但是就是不能使用maven命令，提示没有此命令。进入安装包的bin目录依旧是这个情况。

[Jenkins 2.375.3](https://www.jenkins.io/)

docker pull jenkins/jenkins:lts-jdk11

export MAVEN_HOME=/home/apache-maven-3.6.3

 export PATH=$MAVEN_HOME/bin:$PATH

docker update -v /var/jenkins_home:/var/jenkins_home jenkins

docker run -d --name jenkins -u root -p 8888:8080 -p 50000:50000 --privileged=true  -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker jenkins

```csharp
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json
```

sh ./deploy.sh base
sh ./deploy.sh modules