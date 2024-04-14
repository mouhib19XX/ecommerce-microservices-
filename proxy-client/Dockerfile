
FROM openjdk:11
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} proxy-client.jar
ENTRYPOINT ["java","-jar","/proxy-client.jar"]
EXPOSE 8900