FROM openjdk:11
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} favourite-service.jar
ENTRYPOINT ["java","-jar","/favourite-service.jar"]
EXPOSE 8800