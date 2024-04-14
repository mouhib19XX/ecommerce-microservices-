FROM openjdk:11
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} cloud-config.jar
ENTRYPOINT ["java","-jar","/cloud-config.jar"]
EXPOSE 9296