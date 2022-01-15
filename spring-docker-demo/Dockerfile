FROM openjdk:8

MAINTAINER yangsx<yangsx95@qq.com>
LABEL author=yangsx type=demo

WORKDIR /opt/app

ADD target/spring-docker-demo-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar","app.jar"]