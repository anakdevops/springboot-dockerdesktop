
# springboot-dockerdesktop

# Deploy Spring Boot project "hello world" on top of kubernetes docker desktop
# Use maven to make integration with CI/CD (Jenkins)


Run Jenkins as container

```
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins
```


java 17

```
mvn clean
mvn package
java -jar target/spring-boot-hello-world-devops-0.0.1-SNAPSHOT.jar
```

```
docker build -t anakdevops .
docker run -d --name spring -p 82:8080 anakdevops
curl localhost:82
```

